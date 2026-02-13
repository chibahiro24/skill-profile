# FIFAスタイル プロトタイプ

このディレクトリには、FIFAスタイルのスキルプロファイルのHTMLプロトタイプが含まれています。

## ファイル

### メインファイル
- `skill-profile-full.html` - **📊 フルビュー（推奨）** - 全機能を網羅したダッシュボード
- `fifa-style-profile.html` - **🎯 シンプルビュー** - スキル表示に特化

### その他
- `fifa-style-profile-monotone.html` - テーマ切り替え機能付きバージョン

## 使い方

### 1. ブラウザで開く

```bash
# ファイルをダブルクリック、または
open fifa-style-profile.html

# ブラウザから開く場合
# ファイル > ファイルを開く > fifa-style-profile.html
```

### 2. フルビュー (`skill-profile-full.html`) の特徴

**📋 網羅的なプロフィール表示**
- ✅ **ヘッダーセクション**: アバター、基本情報、統計サマリー
- ✅ **プロフィール詳細**: 自己紹介、外部リンク（GitHub、LinkedIn等）
- ✅ **技術スキル**: プログラミング言語、フレームワーク等
- ✅ **ソフトスキル**: リーダーシップ、コミュニケーション等
- ✅ **プロジェクト経験**: 直近3件のプロジェクト詳細
- ✅ **資格・証明書**: AWS、Google Cloud、PMP等
- ✅ **アクティビティタイムライン**: 最近の活動履歴
- ✅ **グリッドレイアウト**: 見やすいカード形式
- ✅ **レスポンシブデザイン**: PC・タブレット・スマホ対応
- ✅ **統一されたモノトーンデザイン**

**🎯 ビジネス向け設計**
- プロフェッショナルな印象
- 情報が整理されて見やすい
- 企業採用担当者向けに最適化

### 3. シンプルビュー (`fifa-style-profile.html`) の特徴

**🎯 スキル表示に特化**
- ✅ シンプルでプロフェッショナルなカードデザイン
- ✅ モノトーン配色（黒・白・グレー）
- ✅ アバター表示（角丸、イニシャル）
- ✅ 基本情報（Level、Experience、Rank）
- ✅ 6つのスキルバー（グレースケールグラデーション）
- ✅ 総合スコア表示
- ✅ ホバーエフェクト
- ✅ ツールチップ表示
- ✅ 読み込み時のアニメーション効果
- ✅ レスポンシブデザイン

**💡 使い分け**
- スキル可視化に集中したい → シンプルビュー
- 全体的なプロフィールを表示 → フルビュー

### 4. デザインコンセプト

#### フルビュー: `skill-profile-full.html` ⭐推奨

**包括的プロフィールダッシュボード**

**レイアウト**：
- ヘッダーカード: プロフィール概要
- グリッドレイアウト: 2カラム（PC）/ 1カラム（モバイル）
- カード形式: セクションごとに独立したカード
- タイムライン: アクティビティ表示

**配色**：
- 背景: 純粋な黒 (#0a0a0a)
- カード: ダークグレー (#1a1a1a)
- スキルバー: グレースケールグラデーション

**含まれる情報**：
1. **プロフィールヘッダー**
   - アバター、名前、役職、所在地
   - 統計サマリー（Projects、Skills、Certificates）
   - 総合スコア
   - 自己紹介
   - 外部リンク

2. **スキルセクション**
   - Technical Skills（技術スキル）
   - Soft Skills（ソフトスキル）
   - グラフィカルなスキルバー

3. **プロジェクト経験**
   - プロジェクト名、役割、期間
   - 使用技術タグ
   - 直近3件表示

4. **資格・証明書**
   - 資格名、取得日、有効期限
   - アイコン表示

5. **アクティビティタイムライン**
   - 最近の活動履歴
   - タイムスタンプ付き

**用途**：
- ✅ 企業採用サイトでの候補者プロフィール
- ✅ 個人ポートフォリオサイト
- ✅ 社内人材データベース
- ✅ フリーランスのプロフィール
- ✅ キャリア相談の資料

#### シンプルビュー: `fifa-style-profile.html`

**スキル表示特化型**

**配色**：
- 背景: 純粋な黒 (#0a0a0a)
- カード: ダークグレー (#1a1a1a)
- スキルバー: グレースケールグラデーション

**特徴**：
- ✨ ミニマルで洗練されたデザイン
- 🎨 落ち着いたグレースケール
- 🔲 角丸の上品なアバター (12px radius)
- ⬡ シンプルなアイコン
- 📏 統一感のあるレイアウト
- 🚫 テーマ切り替え機能なし（シンプル重視）

**用途**：
- ✅ スキル可視化ツール
- ✅ スキル評価レポート
- ✅ 簡易プロフィール
- ✅ プレゼンテーション資料

## 機能

### インタラクション

1. **カードホバー**
   - カード全体が浮き上がる
   - シャドウが強調される

2. **スキルバーホバー**
   - バーが微妙に拡大
   - 明るくなる
   - ツールチップが表示される

3. **ページ読み込みアニメーション**
   - カードがフェードインで登場
   - スキルバーが順次アニメーション

### レスポンシブ対応

以下のブレークポイントで最適化：

- **デスクトップ**: 768px以上 - 横並びレイアウト
- **モバイル**: 768px未満 - 縦並びレイアウト

## カスタマイズ方法（フルビュー）

### 1. 基本情報を変更

```html
<!-- 名前・役職 -->
<h1>山田 太郎</h1>
<div class="role">Full Stack Engineer / Tech Lead</div>
<div class="location">📍 Tokyo, Japan</div>

<!-- 統計サマリー -->
<div class="stat-value">24</div> <!-- Projects数 -->
<div class="stat-value">18</div> <!-- Skills数 -->
<div class="stat-value">8</div>  <!-- Certificates数 -->

<!-- 自己紹介 -->
<div class="bio">
    あなたの自己紹介文をここに入力...
</div>
```

### 2. スキルを追加・変更

```html
<!-- 技術スキル -->
<div class="skill-item skill-high">
    <div class="skill-name">新しいスキル</div>
    <div class="skill-bar">
        <div class="skill-bar-fill" style="width: 95%"></div>
    </div>
    <div class="skill-value">95</div>
</div>
```

**スキルレベルのクラス**：
- `skill-high`: 90-100点（白グラデーション）
- `skill-medium`: 80-89点（グレーグラデーション）
- `skill-low`: 70-79点（ダークグレーグラデーション）

### 3. プロジェクトを追加

```html
<div class="project-item">
    <div class="project-name">プロジェクト名</div>
    <div class="project-role">あなたの役割</div>
    <div class="project-period">2024.01 - 2024.12 (1 year)</div>
    <div class="project-tags">
        <span class="tag">React</span>
        <span class="tag">Node.js</span>
        <span class="tag">AWS</span>
    </div>
</div>
```

### 4. 資格を追加

```html
<div class="cert-item">
    <div class="cert-info">
        <div class="cert-name">資格名</div>
        <div class="cert-date">取得日: 2024.03 | 有効期限: 2027.03</div>
    </div>
    <div class="cert-badge">🎓</div>
</div>
```

### 5. アクティビティを追加

```html
<div class="timeline-item">
    <div class="timeline-dot"></div>
    <div class="timeline-content">
        <div class="timeline-title">タイトル</div>
        <div class="timeline-desc">詳細説明</div>
        <div class="timeline-date">2024.03.15</div>
    </div>
</div>
```

### 6. 外部リンクを追加

```html
<a href="https://github.com/username" class="link-btn">
    <span>⚡</span> GitHub
</a>
```

---

## カスタマイズ方法（シンプルビュー）

### 1. アバター画像を変更

```html
<!-- 現在（イニシャル表示） -->
<div class="avatar">YT</div>

<!-- 画像に変更 -->
<div class="avatar">
    <img src="path/to/avatar.jpg" alt="Avatar">
</div>
```

### 2. 名前・役職を変更

```html
<div class="name">山田 太郎</div>
<div class="role">フルスタックエンジニア</div>
```

### 3. スキルを追加・変更

```html
<div class="skill-item skill-high">
    <div class="skill-name">新しいスキル</div>
    <div class="skill-bar">
        <div class="skill-bar-fill" style="width: 95%"></div>
    </div>
    <div class="skill-value">95</div>
    <div class="tooltip">
        <strong>新しいスキル</strong><br>
        レベル: エキスパート<br>
        経験年数: 6年
    </div>
</div>
```

#### スキルレベルのクラス

- `skill-high` - 90-100点（緑グラデーション）
- `skill-medium` - 80-89点（青グラデーション）
- `skill-low` - 70-79点（ピンク・黄色グラデーション）

### 4. レベル・XPを変更

```html
<div class="basic-info-value">42</div> <!-- レベル -->
<div class="basic-info-value">12,500 / 15,000</div> <!-- XP -->
<div class="level-progress-fill" style="width: 83%"></div> <!-- プログレスバー -->
```

### 5. ランクを変更

```html
<div class="rank-badge">
    🥇 Gold
</div>

<!-- 他のオプション -->
🥉 Bronze
🥈 Silver
💎 Diamond
⭐ Platinum
```

### 6. 総合スコアを変更

```html
<div class="overall-number">85</div>
```

## 開発者向け情報

### 使用技術

- **HTML5**
- **CSS3**
  - Flexbox
  - CSS Grid
  - CSS Animations
  - CSS Gradients
- **JavaScript (Vanilla)**
  - アニメーション制御
  - テーマ切り替え

### デザイントークン

`design-tokens.json`に基づいて実装：

```css
--primary-gradient: linear-gradient(135deg, #667eea, #764ba2, #f093fb);
--font-family: 'Inter', sans-serif;
--border-radius-card: 16px;
--border-radius-bar: 14px;
--shadow-card: 0 20px 60px rgba(0, 0, 0, 0.3);
```

### パフォーマンス最適化

- ✅ Google Fonts使用（Inter）
- ✅ CSS Transitionsでスムーズなアニメーション
- ✅ Hardware accelerationを活用（transform）
- ✅ 最小限のJavaScript

## 次のステップ

### Figmaへの移行

1. このHTMLプロトタイプで確認
2. 問題があれば調整
3. [Figmaクイックスタートガイド](../docs/fifa-style-figma-quickstart.md)に従ってFigmaで作成

### React/Vueコンポーネント化

このHTMLを参考に、フレームワーク用コンポーネントを作成：

```jsx
// React例
<ProfileCard
  name="山田 太郎"
  role="フルスタックエンジニア"
  level={42}
  xp={12500}
  maxXp={15000}
  rank="Gold"
  overallScore={85}
  skills={[
    { name: 'JavaScript', value: 92, level: 'high' },
    { name: 'TypeScript', value: 85, level: 'medium' },
    // ...
  ]}
  theme="primary"
/>
```

## スクリーンショット

プロトタイプを開いて、以下を確認してください：

- [x] デスクトップ表示
- [x] モバイル表示
- [x] 各テーマの表示
- [x] ホバーエフェクト
- [x] アニメーション

## トラブルシューティング

### スタイルが適用されない

- ブラウザのキャッシュをクリア
- Chromeの開発者ツールで確認（F12）

### アニメーションが動かない

- JavaScriptが有効か確認
- コンソールエラーを確認（F12 > Console）

### フォントが表示されない

- インターネット接続を確認（Google Fonts使用）
- オフラインの場合はフォントをローカルにダウンロード

## フィードバック

このプロトタイプについて改善案がある場合：

1. 直接HTMLを編集して試す
2. [FIFA Style Design Spec](../docs/fifa-style-design-spec.md)を更新
3. チームに共有

---

**🎮 FIFAスタイルのプロフェッショナルなスキルプロファイルをお楽しみください！**
