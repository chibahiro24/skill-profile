# デプロイメントガイド

このガイドでは、Skill Profileのデプロイ方法を説明します。

## 目次

- [環境要件](#環境要件)
- [ローカル開発環境](#ローカル開発環境)
- [Dockerによるデプロイ](#dockerによるデプロイ)
- [クラウドデプロイ](#クラウドデプロイ)
- [環境変数](#環境変数)
- [データベースのセットアップ](#データベースのセットアップ)
- [セキュリティ設定](#セキュリティ設定)
- [モニタリング](#モニタリング)
- [トラブルシューティング](#トラブルシューティング)

---

## 環境要件

### 最小要件

- **Node.js**: 18.x以上
- **PostgreSQL**: 14.x以上
- **Redis**: 7.x以上
- **RAM**: 2GB以上
- **ストレージ**: 10GB以上

### 推奨環境

- **Node.js**: 20.x LTS
- **PostgreSQL**: 15.x
- **Redis**: 7.x
- **MongoDB**: 7.x（ログストレージ用）
- **RAM**: 4GB以上
- **ストレージ**: 50GB以上（SSD推奨）

### 対応OS

- Linux (Ubuntu 22.04 LTS, Debian 11, CentOS 8)
- macOS 12+
- Windows 10/11 (WSL2推奨)

---

## ローカル開発環境

### 1. リポジトリのクローン

```bash
# HTTPSでクローン
git clone https://github.com/YOUR_ORG/skill-profile.git
cd skill-profile

# または SSH
git clone git@github.com:YOUR_ORG/skill-profile.git
cd skill-profile
```

### 2. 依存関係のインストール

```bash
# npm
npm install

# または yarn
yarn install

# または pnpm
pnpm install
```

### 3. 環境変数の設定

```bash
# .envファイルをコピー
cp .env.example .env

# .envファイルを編集
nano .env  # または vim, code 等
```

最小限の`.env`設定：

```env
# アプリケーション
NODE_ENV=development
PORT=3000
APP_URL=http://localhost:3000

# データベース
DATABASE_URL=postgresql://user:password@localhost:5432/skill_profile
REDIS_URL=redis://localhost:6379

# セッション
SESSION_SECRET=your-secret-key-change-this

# JWT
JWT_SECRET=your-jwt-secret-change-this
JWT_EXPIRES_IN=7d

# ログレベル
LOG_LEVEL=debug
```

### 4. データベースのセットアップ

```bash
# PostgreSQLの起動（ローカル）
brew services start postgresql@15  # macOS
sudo systemctl start postgresql    # Linux

# データベースの作成
createdb skill_profile

# マイグレーションの実行
npm run db:migrate

# シードデータの投入（オプション）
npm run db:seed
```

### 5. 開発サーバーの起動

```bash
# フロントエンド＋バックエンド同時起動
npm run dev

# または個別起動
npm run dev:frontend  # ポート3000
npm run dev:backend   # ポート8080
```

ブラウザで http://localhost:3000 にアクセス

---

## Dockerによるデプロイ

### Docker Compose（推奨）

#### 1. 必要なファイル

`docker-compose.yml`:

```yaml
version: '3.8'

services:
  # Webアプリケーション
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://skillprofile:password@postgres:5432/skill_profile
      - REDIS_URL=redis://redis:6379
      - MONGODB_URL=mongodb://mongo:27017/skill_profile_logs
    depends_on:
      - postgres
      - redis
      - mongo
    volumes:
      - ./uploads:/app/uploads
    restart: unless-stopped

  # PostgreSQL
  postgres:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: skillprofile
      POSTGRES_PASSWORD: password
      POSTGRES_DB: skill_profile
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: unless-stopped

  # Redis
  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data
    restart: unless-stopped

  # MongoDB（ログストレージ）
  mongo:
    image: mongo:7
    environment:
      MONGO_INITDB_DATABASE: skill_profile_logs
    volumes:
      - mongo_data:/data/db
    restart: unless-stopped

  # Nginx（リバースプロキシ）
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./nginx/ssl:/etc/nginx/ssl:ro
    depends_on:
      - app
    restart: unless-stopped

volumes:
  postgres_data:
  redis_data:
  mongo_data:
```

`Dockerfile`:

```dockerfile
# ビルドステージ
FROM node:20-alpine AS builder

WORKDIR /app

# 依存関係のインストール
COPY package*.json ./
RUN npm ci --only=production

# ソースコードのコピー
COPY . .

# ビルド
RUN npm run build

# 本番ステージ
FROM node:20-alpine

WORKDIR /app

# 本番用の依存関係のみコピー
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/package*.json ./

# 非rootユーザーの作成
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nextjs -u 1001

USER nextjs

EXPOSE 3000

CMD ["node", "dist/main.js"]
```

#### 2. 起動

```bash
# イメージのビルドと起動
docker-compose up -d

# ログの確認
docker-compose logs -f app

# データベースマイグレーション
docker-compose exec app npm run db:migrate

# 停止
docker-compose down

# 完全削除（ボリュームも削除）
docker-compose down -v
```

### Docker単体

```bash
# イメージのビルド
docker build -t skill-profile:latest .

# コンテナの起動
docker run -d \
  --name skill-profile \
  -p 3000:3000 \
  -e DATABASE_URL=postgresql://... \
  -e REDIS_URL=redis://... \
  skill-profile:latest

# ログの確認
docker logs -f skill-profile

# コンテナの停止
docker stop skill-profile

# コンテナの削除
docker rm skill-profile
```

---

## クラウドデプロイ

### AWS (Elastic Beanstalk + RDS)

#### 1. 事前準備

```bash
# AWS CLI のインストール
brew install awscli  # macOS
pip install awscli   # その他

# 認証情報の設定
aws configure
```

#### 2. Elastic Beanstalk のセットアップ

```bash
# EB CLI のインストール
pip install awsebcli

# アプリケーションの初期化
eb init skill-profile \
  --platform node.js \
  --region ap-northeast-1

# 環境の作成
eb create skill-profile-prod \
  --database.engine postgres \
  --database.size 10 \
  --instance-type t3.small \
  --envvars \
    NODE_ENV=production,\
    SESSION_SECRET=your-secret
```

#### 3. デプロイ

```bash
# デプロイ
eb deploy

# ステータス確認
eb status

# ログの確認
eb logs

# 環境を開く
eb open
```

#### 4. RDS の設定

```bash
# RDS の作成（別途）
aws rds create-db-instance \
  --db-instance-identifier skill-profile-db \
  --db-instance-class db.t3.micro \
  --engine postgres \
  --master-username admin \
  --master-user-password YourPassword123 \
  --allocated-storage 20

# 接続情報を環境変数に設定
eb setenv DATABASE_URL=postgresql://admin:YourPassword123@xxx.rds.amazonaws.com:5432/skill_profile
```

### Google Cloud Platform (Cloud Run + Cloud SQL)

#### 1. 事前準備

```bash
# gcloud CLI のインストール
curl https://sdk.cloud.google.com | bash

# 初期化
gcloud init

# プロジェクトの設定
gcloud config set project YOUR_PROJECT_ID
```

#### 2. Cloud SQL のセットアップ

```bash
# Cloud SQL インスタンスの作成
gcloud sql instances create skill-profile-db \
  --database-version=POSTGRES_15 \
  --tier=db-f1-micro \
  --region=asia-northeast1

# データベースの作成
gcloud sql databases create skill_profile \
  --instance=skill-profile-db

# ユーザーの作成
gcloud sql users create skillprofile \
  --instance=skill-profile-db \
  --password=YourPassword123
```

#### 3. Cloud Run へのデプロイ

```bash
# Container Registry に push
gcloud builds submit --tag gcr.io/YOUR_PROJECT_ID/skill-profile

# Cloud Run にデプロイ
gcloud run deploy skill-profile \
  --image gcr.io/YOUR_PROJECT_ID/skill-profile \
  --platform managed \
  --region asia-northeast1 \
  --allow-unauthenticated \
  --set-env-vars NODE_ENV=production \
  --add-cloudsql-instances YOUR_PROJECT_ID:asia-northeast1:skill-profile-db

# URL の確認
gcloud run services describe skill-profile \
  --platform managed \
  --region asia-northeast1 \
  --format 'value(status.url)'
```

### Azure (App Service + Azure Database for PostgreSQL)

#### 1. 事前準備

```bash
# Azure CLI のインストール
brew install azure-cli  # macOS
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash  # Ubuntu

# ログイン
az login
```

#### 2. リソースの作成

```bash
# リソースグループの作成
az group create \
  --name skill-profile-rg \
  --location japaneast

# App Service プランの作成
az appservice plan create \
  --name skill-profile-plan \
  --resource-group skill-profile-rg \
  --sku B1 \
  --is-linux

# Web App の作成
az webapp create \
  --name skill-profile \
  --resource-group skill-profile-rg \
  --plan skill-profile-plan \
  --runtime "NODE|20-lts"
```

#### 3. PostgreSQL のセットアップ

```bash
# PostgreSQL サーバーの作成
az postgres flexible-server create \
  --resource-group skill-profile-rg \
  --name skill-profile-db \
  --location japaneast \
  --admin-user adminuser \
  --admin-password YourPassword123 \
  --sku-name Standard_B1ms \
  --tier Burstable \
  --version 15

# ファイアウォールルールの設定
az postgres flexible-server firewall-rule create \
  --resource-group skill-profile-rg \
  --name skill-profile-db \
  --rule-name AllowAzureServices \
  --start-ip-address 0.0.0.0 \
  --end-ip-address 0.0.0.0
```

#### 4. デプロイ

```bash
# コードのデプロイ
az webapp deployment source config \
  --name skill-profile \
  --resource-group skill-profile-rg \
  --repo-url https://github.com/YOUR_ORG/skill-profile \
  --branch main \
  --manual-integration

# 環境変数の設定
az webapp config appsettings set \
  --name skill-profile \
  --resource-group skill-profile-rg \
  --settings \
    NODE_ENV=production \
    DATABASE_URL="postgresql://adminuser:YourPassword123@skill-profile-db.postgres.database.azure.com:5432/skill_profile"
```

### Kubernetes (K8s)

#### `k8s/deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: skill-profile
  labels:
    app: skill-profile
spec:
  replicas: 3
  selector:
    matchLabels:
      app: skill-profile
  template:
    metadata:
      labels:
        app: skill-profile
    spec:
      containers:
      - name: app
        image: skill-profile:latest
        ports:
        - containerPort: 3000
        env:
        - name: NODE_ENV
          value: "production"
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: skill-profile-secrets
              key: database-url
        - name: REDIS_URL
          valueFrom:
            secretKeyRef:
              name: skill-profile-secrets
              key: redis-url
        resources:
          requests:
            memory: "512Mi"
            cpu: "500m"
          limits:
            memory: "1Gi"
            cpu: "1000m"
        livenessProbe:
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 3000
          initialDelaySeconds: 10
          periodSeconds: 5
---
apiVersion: v1
kind: Service
metadata:
  name: skill-profile-service
spec:
  selector:
    app: skill-profile
  ports:
  - protocol: TCP
    port: 80
    targetPort: 3000
  type: LoadBalancer
---
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: skill-profile-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: skill-profile
  minReplicas: 3
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

#### デプロイ

```bash
# シークレットの作成
kubectl create secret generic skill-profile-secrets \
  --from-literal=database-url='postgresql://...' \
  --from-literal=redis-url='redis://...'

# デプロイ
kubectl apply -f k8s/deployment.yaml

# ステータス確認
kubectl get pods
kubectl get services

# ログの確認
kubectl logs -f deployment/skill-profile

# スケーリング
kubectl scale deployment skill-profile --replicas=5
```

---

## 環境変数

### 必須変数

```env
# アプリケーション
NODE_ENV=production                      # 環境（development/production）
PORT=3000                                # ポート番号
APP_URL=https://skillprofile.example.com # アプリケーションURL

# データベース
DATABASE_URL=postgresql://user:pass@host:5432/db  # PostgreSQL接続URL
REDIS_URL=redis://host:6379                       # Redis接続URL
MONGODB_URL=mongodb://host:27017/logs            # MongoDB接続URL（オプション）

# セキュリティ
SESSION_SECRET=your-session-secret-32-chars-min   # セッション暗号化キー
JWT_SECRET=your-jwt-secret-32-chars-min          # JWT署名キー
JWT_EXPIRES_IN=7d                                # JWTの有効期限

# 暗号化
ENCRYPTION_KEY=your-encryption-key-32-bytes      # データ暗号化キー
```

### オプション変数

```env
# AI連携
OPENAI_API_KEY=sk-...                   # OpenAI APIキー
ANTHROPIC_API_KEY=...                   # Anthropic APIキー
GOOGLE_AI_API_KEY=...                   # Google AI APIキー

# ストレージ
AWS_S3_BUCKET=skill-profile-uploads     # S3バケット名
AWS_REGION=ap-northeast-1               # AWSリージョン
AWS_ACCESS_KEY_ID=...                   # AWS アクセスキー
AWS_SECRET_ACCESS_KEY=...               # AWS シークレットキー

# メール
SMTP_HOST=smtp.gmail.com                # SMTPホスト
SMTP_PORT=587                           # SMTPポート
SMTP_USER=noreply@example.com           # SMTPユーザー
SMTP_PASSWORD=...                       # SMTPパスワード
EMAIL_FROM=noreply@skillprofile.com     # 送信元メールアドレス

# OAuth
GITHUB_CLIENT_ID=...                    # GitHub OAuth クライアントID
GITHUB_CLIENT_SECRET=...                # GitHub OAuth シークレット
GOOGLE_CLIENT_ID=...                    # Google OAuth クライアントID
GOOGLE_CLIENT_SECRET=...                # Google OAuth シークレット

# モニタリング
SENTRY_DSN=https://...@sentry.io/...    # Sentry DSN
LOG_LEVEL=info                          # ログレベル（debug/info/warn/error）

# その他
RATE_LIMIT_MAX=100                      # レート制限（リクエスト/分）
RATE_LIMIT_WINDOW=60000                 # レート制限ウィンドウ（ミリ秒）
```

---

## データベースのセットアップ

### PostgreSQL のマイグレーション

```bash
# マイグレーションファイルの作成
npm run migration:create -- AddUserTable

# マイグレーションの実行
npm run migration:run

# マイグレーションのロールバック
npm run migration:revert

# マイグレーションステータスの確認
npm run migration:show
```

### Redis のセットアップ

```bash
# Redisの起動
redis-server

# または Docker
docker run -d -p 6379:6379 redis:7-alpine

# 接続テスト
redis-cli ping
# 出力: PONG
```

### バックアップとリストア

#### PostgreSQL

```bash
# バックアップ
pg_dump -U user -h host -d skill_profile -F c -f backup.dump

# リストア
pg_restore -U user -h host -d skill_profile backup.dump

# 自動バックアップスクリプト
cat > backup.sh << 'EOF'
#!/bin/bash
DATE=$(date +%Y%m%d_%H%M%S)
pg_dump -U user -h host -d skill_profile -F c -f /backups/skill_profile_$DATE.dump
# 7日以上前のバックアップを削除
find /backups -name "skill_profile_*.dump" -mtime +7 -delete
EOF

chmod +x backup.sh

# cronで毎日実行
crontab -e
# 0 2 * * * /path/to/backup.sh
```

---

## セキュリティ設定

### SSL/TLS の設定

#### Let's Encrypt (Certbot)

```bash
# Certbot のインストール
sudo apt install certbot python3-certbot-nginx

# 証明書の取得
sudo certbot --nginx -d skillprofile.example.com

# 自動更新の設定（cron）
sudo certbot renew --dry-run
```

#### nginx.conf with SSL

```nginx
server {
    listen 80;
    server_name skillprofile.example.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name skillprofile.example.com;

    ssl_certificate /etc/letsencrypt/live/skillprofile.example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/skillprofile.example.com/privkey.pem;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    location / {
        proxy_pass http://app:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### ファイアウォール設定

```bash
# UFW (Ubuntu)
sudo ufw allow 22/tcp   # SSH
sudo ufw allow 80/tcp   # HTTP
sudo ufw allow 443/tcp  # HTTPS
sudo ufw enable

# ファイアウォールの確認
sudo ufw status
```

---

## モニタリング

### ヘルスチェックエンドポイント

アプリケーションに以下のエンドポイントを実装：

```typescript
// /health - 基本的なヘルスチェック
app.get('/health', (req, res) => {
  res.json({ status: 'ok', timestamp: new Date() });
});

// /ready - 依存サービスを含むヘルスチェック
app.get('/ready', async (req, res) => {
  const checks = {
    database: await checkDatabase(),
    redis: await checkRedis(),
    mongodb: await checkMongoDB()
  };
  
  const isReady = Object.values(checks).every(check => check === true);
  
  res.status(isReady ? 200 : 503).json({
    ready: isReady,
    checks
  });
});
```

### ログ管理

```bash
# アプリケーションログ
tail -f logs/app.log

# Dockerログ
docker logs -f skill-profile

# systemd (Linux)
journalctl -u skill-profile -f
```

### メトリクス収集 (Prometheus)

`prometheus.yml`:

```yaml
scrape_configs:
  - job_name: 'skill-profile'
    static_configs:
      - targets: ['localhost:3000']
    metrics_path: '/metrics'
```

---

## トラブルシューティング

### データベース接続エラー

```bash
# PostgreSQLの接続テスト
psql -h localhost -U user -d skill_profile

# 接続パラメータの確認
echo $DATABASE_URL

# ファイアウォールの確認
sudo ufw status
telnet localhost 5432
```

### メモリ不足

```bash
# メモリ使用量の確認
free -h

# プロセスのメモリ使用量
ps aux --sort=-%mem | head

# Node.jsのメモリ制限を増やす
NODE_OPTIONS="--max-old-space-size=4096" npm start
```

### パフォーマンス問題

```bash
# スロークエリの確認（PostgreSQL）
SELECT * FROM pg_stat_statements 
ORDER BY total_time DESC 
LIMIT 10;

# Redisの統計
redis-cli INFO stats

# アプリケーションのプロファイリング
node --prof dist/main.js
```

---

## 次のステップ

- [SECURITY.md](SECURITY.md) でセキュリティ設定を確認
- [MONITORING.md](docs/MONITORING.md) で監視設定を確認
- [SCALING.md](docs/SCALING.md) でスケーリング戦略を確認

---

**ヘルプが必要な場合は [GitHub Discussions](https://github.com/YOUR_ORG/skill-profile/discussions) で質問してください。**
