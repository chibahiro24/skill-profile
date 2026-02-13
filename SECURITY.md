# セキュリティポリシー

## 脆弱性の報告

Skill Profileのセキュリティを真剣に考えています。セキュリティ上の脆弱性を発見した場合は、責任ある開示プロセスに従って報告してください。

### 報告方法

**重要: 公開のGitHub Issueでセキュリティ脆弱性を報告しないでください。**

セキュリティ脆弱性を発見した場合は、以下の方法で報告してください：

1. **GitHub Security Advisory** (推奨)
   - リポジトリの "Security" タブから "Report a vulnerability" を選択
   - プライベートに脆弱性の詳細を報告

2. **メール**
   - security@skillprofile.example.com
   - 件名に [SECURITY] を含める
   - PGP暗号化推奨（公開鍵: [リンク]）

3. **暗号化されたメッセージ**
   - Keybase: @skillprofile
   - Signal: [連絡先]

### 報告に含めるべき情報

脆弱性を効果的に評価し対処するため、以下の情報を含めてください：

- **脆弱性の種類** (例: SQL injection, XSS, CSRF等)
- **影響を受けるコンポーネント** (ファイル名、関数名、行番号等)
- **再現手順** (できるだけ詳細に)
- **概念実証コード** (PoC) またはスクリーンショット
- **影響範囲** (どのような攻撃が可能か、どのようなデータが漏洩する可能性があるか)
- **推奨される修正方法** (可能であれば)
- **発見者の情報** (クレジット表記を希望する場合）

### 報告後のプロセス

1. **受領確認** - 48時間以内に受領確認メールを送信
2. **初期評価** - 5営業日以内に初期評価を実施
3. **調査** - 脆弱性の詳細な調査と検証
4. **修正** - 修正パッチの作成とテスト
5. **公開** - 修正版のリリースと脆弱性の公開
6. **クレジット** - 報告者のクレジット表記（希望する場合）

### タイムライン

- **重大な脆弱性**: 7日以内に修正版をリリース
- **高度な脆弱性**: 30日以内に修正版をリリース
- **中程度の脆弱性**: 90日以内に修正版をリリース
- **低度の脆弱性**: 次のメジャーリリースで対応

---

## サポートされているバージョン

以下のバージョンに対してセキュリティアップデートを提供しています：

| バージョン | サポート状況 |
|----------|----------|
| 1.x      | ✅ サポート中 |
| < 1.0    | ❌ サポート終了 |

---

## セキュリティ対策

### 認証・認可

#### 実装済みの対策

- **OAuth 2.0 / OpenID Connect**: 標準的な認証プロトコル
- **JWT (JSON Web Token)**: ステートレスな認証
- **パスワードハッシュ化**: Argon2idによる安全なハッシュ化
- **二段階認証 (2FA)**: TOTP based 2FA
- **セッション管理**: 安全なセッション管理
- **RBAC (Role-Based Access Control)**: ロールベースのアクセス制御

#### ベストプラクティス

```typescript
// パスワードのハッシュ化
import argon2 from 'argon2';

async function hashPassword(password: string): Promise<string> {
  return await argon2.hash(password, {
    type: argon2.argon2id,
    memoryCost: 2 ** 16,  // 64MB
    timeCost: 3,
    parallelism: 1
  });
}

// パスワードの検証
async function verifyPassword(hash: string, password: string): Promise<boolean> {
  try {
    return await argon2.verify(hash, password);
  } catch (error) {
    return false;
  }
}
```

### データ保護

#### 実装済みの対策

- **保存データの暗号化**: AES-256-GCMによる暗号化
- **通信の暗号化**: TLS 1.3
- **データベース暗号化**: PostgreSQL透過的データ暗号化 (TDE)
- **バックアップ暗号化**: 暗号化されたバックアップ
- **キー管理**: KMS (Key Management Service)による鍵管理

#### 暗号化の実装

```typescript
import crypto from 'crypto';

const ALGORITHM = 'aes-256-gcm';
const IV_LENGTH = 16;
const SALT_LENGTH = 64;
const TAG_LENGTH = 16;
const KEY_LENGTH = 32;

// データの暗号化
export function encrypt(text: string, masterKey: Buffer): string {
  // ランダムなIVとソルトを生成
  const iv = crypto.randomBytes(IV_LENGTH);
  const salt = crypto.randomBytes(SALT_LENGTH);

  // PBKDF2で鍵を導出
  const key = crypto.pbkdf2Sync(masterKey, salt, 100000, KEY_LENGTH, 'sha512');

  // 暗号化
  const cipher = crypto.createCipheriv(ALGORITHM, key, iv);
  const encrypted = Buffer.concat([
    cipher.update(text, 'utf8'),
    cipher.final()
  ]);

  // 認証タグを取得
  const tag = cipher.getAuthTag();

  // すべてを結合: salt + iv + tag + encrypted
  return Buffer.concat([salt, iv, tag, encrypted]).toString('base64');
}

// データの復号化
export function decrypt(encryptedText: string, masterKey: Buffer): string {
  const buffer = Buffer.from(encryptedText, 'base64');

  // データを分解
  const salt = buffer.subarray(0, SALT_LENGTH);
  const iv = buffer.subarray(SALT_LENGTH, SALT_LENGTH + IV_LENGTH);
  const tag = buffer.subarray(SALT_LENGTH + IV_LENGTH, SALT_LENGTH + IV_LENGTH + TAG_LENGTH);
  const encrypted = buffer.subarray(SALT_LENGTH + IV_LENGTH + TAG_LENGTH);

  // PBKDF2で鍵を導出
  const key = crypto.pbkdf2Sync(masterKey, salt, 100000, KEY_LENGTH, 'sha512');

  // 復号化
  const decipher = crypto.createDecipheriv(ALGORITHM, key, iv);
  decipher.setAuthTag(tag);

  return decipher.update(encrypted) + decipher.final('utf8');
}
```

### 脆弱性対策

#### SQL インジェクション

```typescript
// ❌ 悪い例 - SQLインジェクション脆弱性
const userId = req.params.id;
const query = `SELECT * FROM users WHERE id = ${userId}`;
db.query(query);  // 危険！

// ✅ 良い例 - パラメータ化クエリ
const userId = req.params.id;
const query = 'SELECT * FROM users WHERE id = $1';
db.query(query, [userId]);

// ✅ 良い例 - ORM使用
const user = await User.findOne({ where: { id: userId } });
```

#### XSS (Cross-Site Scripting)

```typescript
// ❌ 悪い例 - XSS脆弱性
res.send(`<h1>Hello ${req.query.name}</h1>`);  // 危険！

// ✅ 良い例 - エスケープ処理
import escape from 'escape-html';
res.send(`<h1>Hello ${escape(req.query.name)}</h1>`);

// ✅ 良い例 - テンプレートエンジン使用（自動エスケープ）
res.render('hello', { name: req.query.name });
```

#### CSRF (Cross-Site Request Forgery)

```typescript
import csrf from 'csurf';

// CSRFミドルウェアの設定
const csrfProtection = csrf({ cookie: true });

app.use(csrfProtection);

// フォームにCSRFトークンを含める
app.get('/form', (req, res) => {
  res.render('form', { csrfToken: req.csrfToken() });
});

// POSTリクエストで検証
app.post('/submit', (req, res) => {
  // csrfProtectionミドルウェアが自動的に検証
  res.send('Data received');
});
```

#### コマンドインジェクション

```typescript
// ❌ 悪い例 - コマンドインジェクション脆弱性
const filename = req.query.file;
exec(`cat ${filename}`, callback);  // 危険！

// ✅ 良い例 - 安全な代替手段
const fs = require('fs/promises');
const path = require('path');

const filename = path.basename(req.query.file);  // パスを正規化
const filepath = path.join(__dirname, 'files', filename);

// ディレクトリトラバーサル対策
if (!filepath.startsWith(path.join(__dirname, 'files'))) {
  throw new Error('Invalid file path');
}

const content = await fs.readFile(filepath, 'utf8');
```

### レート制限

```typescript
import rateLimit from 'express-rate-limit';

// 一般的なレート制限
const generalLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,  // 15分
  max: 100,                   // 100リクエスト
  message: 'Too many requests, please try again later.'
});

// ログインのレート制限（より厳しく）
const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,  // 15分
  max: 5,                     // 5回まで
  skipSuccessfulRequests: true,  // 成功したリクエストはカウントしない
  message: 'Too many login attempts, please try again later.'
});

app.use('/api/', generalLimiter);
app.use('/api/auth/login', loginLimiter);
```

### セキュアヘッダー

```typescript
import helmet from 'helmet';

app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      scriptSrc: ["'self'"],
      imgSrc: ["'self'", "data:", "https:"],
      connectSrc: ["'self'", "https://api.skillprofile.com"],
      fontSrc: ["'self'"],
      objectSrc: ["'none'"],
      mediaSrc: ["'self'"],
      frameSrc: ["'none'"]
    }
  },
  hsts: {
    maxAge: 31536000,  // 1年
    includeSubDomains: true,
    preload: true
  },
  noSniff: true,
  referrerPolicy: { policy: 'strict-origin-when-cross-origin' },
  permissionsPolicy: {
    features: {
      geolocation: ["'none'"],
      microphone: ["'none'"],
      camera: ["'none'"],
      payment: ["'none'"]
    }
  }
}));
```

### 入力検証

```typescript
import { z } from 'zod';

// Zodスキーマで入力検証
const userSchema = z.object({
  email: z.string().email(),
  password: z.string()
    .min(12, 'パスワードは12文字以上である必要があります')
    .regex(/[A-Z]/, '大文字を含める必要があります')
    .regex(/[a-z]/, '小文字を含める必要があります')
    .regex(/[0-9]/, '数字を含める必要があります')
    .regex(/[^A-Za-z0-9]/, '特殊文字を含める必要があります'),
  name: z.string()
    .min(1)
    .max(100)
    .regex(/^[a-zA-Z0-9\s-_]+$/, '無効な文字が含まれています')
});

app.post('/api/users', async (req, res) => {
  try {
    const validatedData = userSchema.parse(req.body);
    // 検証済みデータを使用
    const user = await createUser(validatedData);
    res.json(user);
  } catch (error) {
    if (error instanceof z.ZodError) {
      res.status(400).json({ errors: error.errors });
    } else {
      res.status(500).json({ error: 'Internal server error' });
    }
  }
});
```

---

## プラグインのセキュリティ

### サンドボックス実行

プラグインは隔離された環境で実行されます：

- **WebAssembly**: セキュアなサンドボックス実行
- **リソース制限**: CPU、メモリ、ネットワークの制限
- **権限システム**: 明示的な権限要求
- **コードレビュー**: 公開前の手動レビュー

### プラグイン開発者向けガイドライン

```typescript
// ✅ 良い例 - 安全なAPI呼び出し
async collectLogs(config: PluginConfig): Promise<ActivityLog[]> {
  try {
    // タイムアウト設定
    const controller = new AbortController();
    const timeout = setTimeout(() => controller.abort(), 30000);

    const response = await fetch(config.apiUrl, {
      signal: controller.signal,
      headers: {
        'Authorization': `Bearer ${config.apiKey}`,
        'User-Agent': 'SkillProfile-Plugin/1.0'
      }
    });

    clearTimeout(timeout);

    if (!response.ok) {
      throw new Error(`API error: ${response.status}`);
    }

    return await response.json();
  } catch (error) {
    console.error('Failed to collect logs:', error);
    return [];  // エラー時は空の配列を返す
  }
}

// ❌ 悪い例 - センシティブ情報のログ出力
console.log('API Key:', config.apiKey);  // 危険！

// ✅ 良い例 - マスク処理
console.log('API Key:', config.apiKey.substring(0, 4) + '****');
```

---

## インシデント対応

### セキュリティインシデント発生時

1. **検知**: 異常なアクティビティの検知
2. **封じ込め**: 影響範囲の特定と隔離
3. **根絶**: 脆弱性の修正
4. **復旧**: サービスの復旧
5. **事後対応**: インシデントレポートの作成
6. **改善**: 再発防止策の実施

### 連絡先

- **セキュリティチーム**: security@skillprofile.example.com
- **緊急連絡先**: +81-XX-XXXX-XXXX（24時間対応）
- **PGP公開鍵**: https://skillprofile.example.com/pgp-key.asc

---

## セキュリティ監査

### 定期監査

- **コードレビュー**: プルリクエストごとのセキュリティレビュー
- **依存関係スキャン**: 週次の脆弱性スキャン
- **ペネトレーションテスト**: 年次のペンテスト
- **セキュリティ監査**: 外部機関による年次監査

### 脆弱性スキャン

```bash
# npm audit
npm audit

# Snyk
snyk test

# OWASP Dependency Check
dependency-check --project "Skill Profile" --scan .

# Trivy (コンテナスキャン)
trivy image skill-profile:latest
```

---

## セキュリティ関連リンク

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [CWE/SANS Top 25](https://cwe.mitre.org/top25/)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)

---

## 謝辞

セキュリティ脆弱性を責任を持って報告してくださった以下の方々に感謝します：

- (報告者のリストは脆弱性が修正され公開された後に追加されます)

---

**最終更新**: 2026年2月

このセキュリティポリシーは定期的に見直され、更新されます。
