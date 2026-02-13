# GitHub リポジトリ セットアップガイド

## 1. リポジトリの作成

### 方法A: GitHub Web UIを使用（推奨）

1. [GitHub](https://github.com/new) にアクセス
2. 以下を入力：
   - **Repository name**: `skill-profile`
   - **Description**: `スキルプロファイル管理システム - 個人のスキル情報を管理・公開するプラットフォーム`
   - **Public** を選択
   - ⚠️ "Initialize this repository" のオプションは**全てチェックを外す**
3. "Create repository" をクリック

### 方法B: GitHub CLI を使用

```bash
gh repo create skill-profile --public --source=. --remote=origin --push --description "スキルプロファイル管理システム - 個人のスキル情報を管理・公開するプラットフォーム"
```

---

## 2. ローカルリポジトリとの接続

GitHubでリポジトリを作成したら、以下のコマンドを実行：

```bash
# リモートリポジトリを追加
git remote add origin https://github.com/<YOUR_USERNAME>/skill-profile.git

# またはSSHを使用する場合：
# git remote add origin git@github.com:<YOUR_USERNAME>/skill-profile.git

# プッシュ
git push -u origin main
```

---

## 3. GitHub Secrets の設定（Figma連携用）

Figma自動エクスポート機能を使用する場合：

### 3.1 Figma Personal Access Token を取得

1. [Figma Settings](https://www.figma.com/settings) にアクセス
2. "Personal access tokens" セクションに移動
3. "Create a new personal access token" をクリック
4. トークンをコピー

### 3.2 GitHub Secrets に追加

1. GitHubリポジトリの **Settings** → **Secrets and variables** → **Actions**
2. "New repository secret" をクリック
3. 以下のSecretsを追加：

   - **Name**: `FIGMA_TOKEN`
   - **Value**: 先ほどコピーしたFigmaトークン

4. もう一つ追加：
   - **Name**: `FIGMA_FILE_KEY`
   - **Value**: FigmaファイルのURL内の `/file/` 後の文字列
     - 例: `https://www.figma.com/file/ABC123xyz/Design-System` → `ABC123xyz`

---

## 4. Figmaファイルリンクの更新

### 4.1 Figmaファイルを作成

1. [Figma](https://www.figma.com/) で以下のファイルを作成：
   - `Skill Profile - Design System`
   - `Skill Profile - Wireframes`
   - `Skill Profile - UI Design`
   - `Skill Profile - Prototype`

### 4.2 README.md を更新

1. 各Figmaファイルの「Share」→「Copy link」でリンクを取得
2. `README.md` の以下の部分を更新：

```markdown
### Figmaファイル

- **デザインシステム**: [Skill Profile - Design System](https://www.figma.com/file/YOUR_FILE_KEY/Design-System)
- **ワイヤーフレーム**: [Skill Profile - Wireframes](https://www.figma.com/file/YOUR_FILE_KEY/Wireframes)
- **UIデザイン**: [Skill Profile - UI Design](https://www.figma.com/file/YOUR_FILE_KEY/UI-Design)
- **プロトタイプ**: [Skill Profile - Prototype](https://www.figma.com/file/YOUR_FILE_KEY/Prototype)
```

3. 変更をコミット＆プッシュ：

```bash
git add README.md
git commit -m "docs: Update Figma file links"
git push
```

---

## 5. Figma Tokens プラグインの設定（オプション）

デザイントークンを自動同期する場合：

### 5.1 GitHub Personal Access Token を作成

1. GitHub **Settings** → **Developer settings** → **Personal access tokens** → **Tokens (classic)**
2. "Generate new token (classic)" をクリック
3. スコープで `repo` を選択
4. トークンをコピー

### 5.2 Figma Tokens プラグインで設定

1. Figmaで「Figma Tokens」プラグインをインストール
2. プラグインを起動
3. **Settings** → **Sync** → **GitHub**
4. 以下を入力：
   - **Name**: `skill-profile`
   - **Personal Access Token**: 先ほど作成したトークン
   - **Repository**: `<YOUR_USERNAME>/skill-profile`
   - **Branch**: `main`
   - **File Path**: `design-tokens.json`
5. 「Test」で接続確認
6. 「Save」で保存

---

## 6. GitHub Actions の動作確認

### 手動実行

1. GitHubリポジトリの **Actions** タブに移動
2. 「Export Figma Assets」ワークフローを選択
3. "Run workflow" をクリック
4. 実行結果を確認

---

## 7. ブランチ保護ルールの設定（オプション）

mainブランチを保護する場合：

1. リポジトリの **Settings** → **Branches**
2. "Add branch protection rule" をクリック
3. 以下を設定：
   - **Branch name pattern**: `main`
   - ✅ Require a pull request before merging
   - ✅ Require status checks to pass before merging
4. "Create" をクリック

---

## 8. コラボレーターの追加（チーム開発の場合）

1. リポジトリの **Settings** → **Collaborators**
2. "Add people" をクリック
3. GitHubユーザー名を入力
4. 権限レベルを選択（Read/Write/Admin）
5. 招待を送信

---

## チェックリスト

セットアップ完了の確認：

- [ ] GitHubリポジトリを作成
- [ ] ローカルとリモートを接続
- [ ] 初回プッシュ完了
- [ ] Figmaファイルを作成
- [ ] README.mdのFigmaリンクを更新
- [ ] GitHub Secretsを設定（Figma連携する場合）
- [ ] Figma Tokensプラグインを設定（使用する場合）
- [ ] GitHub Actionsが正常に動作することを確認

---

## トラブルシューティング

### プッシュできない

**エラー**: `Permission denied (publickey)`

**解決策**:
- SSH鍵が正しく設定されているか確認
- または、HTTPSを使用：`https://github.com/<USERNAME>/skill-profile.git`

### GitHub Actions が失敗する

**解決策**:
- Secretsが正しく設定されているか確認
- FIGMA_TOKEN の権限を確認
- ワークフローファイルの構文エラーをチェック

---

## 参考リンク

- [GitHub Docs](https://docs.github.com/)
- [Figma API Documentation](https://www.figma.com/developers/api)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
