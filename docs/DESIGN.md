# デザインガイドライン

## Figma連携

### Figmaファイル構成

プロジェクトのデザインは以下のFigmaファイルで管理します：

#### 1. デザインシステム
- **ファイル名**: `Skill Profile - Design System`
- **内容**: 
  - カラーパレット
  - タイポグラフィ
  - スペーシングシステム
  - アイコンセット
  - 基本コンポーネント（ボタン、入力フィールド、カード等）

#### 2. ワイヤーフレーム
- **ファイル名**: `Skill Profile - Wireframes`
- **内容**:
  - ユーザーフロー
  - 画面遷移図
  - 低詳細度のワイヤーフレーム

#### 3. UIデザイン
- **ファイル名**: `Skill Profile - UI Design`
- **内容**:
  - 各画面の高詳細度デザイン
  - レスポンシブデザイン（デスクトップ/タブレット/モバイル）
  - インタラクションデザイン

#### 4. プロトタイプ
- **ファイル名**: `Skill Profile - Prototype`
- **内容**:
  - インタラクティブプロトタイプ
  - ユーザーテスト用モックアップ

## デザイントークン

デザイントークンは `design-tokens.json` で管理し、コードとデザインの一貫性を保ちます。

### カラー

```json
{
  "colors": {
    "primary": {
      "50": "#E3F2FD",
      "100": "#BBDEFB",
      "500": "#2196F3",
      "700": "#1976D2",
      "900": "#0D47A1"
    },
    "secondary": {
      "50": "#F3E5F5",
      "100": "#E1BEE7",
      "500": "#9C27B0",
      "700": "#7B1FA2",
      "900": "#4A148C"
    },
    "neutral": {
      "0": "#FFFFFF",
      "50": "#FAFAFA",
      "100": "#F5F5F5",
      "200": "#EEEEEE",
      "500": "#9E9E9E",
      "700": "#616161",
      "900": "#212121"
    },
    "success": "#4CAF50",
    "warning": "#FF9800",
    "error": "#F44336",
    "info": "#2196F3"
  }
}
```

### タイポグラフィ

```json
{
  "typography": {
    "fontFamily": {
      "primary": "Inter, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif",
      "monospace": "'Fira Code', 'Courier New', monospace"
    },
    "fontSize": {
      "xs": "12px",
      "sm": "14px",
      "base": "16px",
      "lg": "18px",
      "xl": "20px",
      "2xl": "24px",
      "3xl": "30px",
      "4xl": "36px",
      "5xl": "48px"
    },
    "fontWeight": {
      "regular": 400,
      "medium": 500,
      "semibold": 600,
      "bold": 700
    },
    "lineHeight": {
      "tight": 1.25,
      "normal": 1.5,
      "relaxed": 1.75
    }
  }
}
```

### スペーシング

```json
{
  "spacing": {
    "xs": "4px",
    "sm": "8px",
    "md": "16px",
    "lg": "24px",
    "xl": "32px",
    "2xl": "48px",
    "3xl": "64px"
  }
}
```

### ブレークポイント

```json
{
  "breakpoints": {
    "mobile": "320px",
    "tablet": "768px",
    "desktop": "1024px",
    "wide": "1440px"
  }
}
```

## デザイン→開発フロー

### 1. デザインフェーズ
1. Figmaで画面デザインを作成
2. コンポーネントをFigmaのコンポーネント機能で管理
3. デザインレビュー実施
4. デザイン承認

### 2. 開発準備
1. Figma Dev Modeでインスペクト
2. 必要なアセット（画像、アイコン等）をエクスポート
3. デザイントークンを更新

### 3. 実装フェーズ
1. Figmaのスペックを参照しながらコーディング
2. Figma Inspectでスタイル値を確認
3. コンポーネントの実装

### 4. レビュー
1. 実装とデザインの照合
2. レスポンシブ対応の確認
3. アクセシビリティチェック

## Figma Dev Mode 活用

### インスペクト機能
- CSS/Tailwind CSS/React Native等のコード取得
- スペーシング、カラー、フォントの値を正確に取得
- アセットの直接ダウンロード

### ステータス管理
デザインの実装状態を管理：
- `Ready for dev` - 開発可能
- `In progress` - 実装中
- `Completed` - 実装完了

## コンポーネント管理

### Figmaコンポーネント構造

```
Components/
├── Atoms/
│   ├── Button
│   ├── Input
│   ├── Label
│   ├── Icon
│   └── Badge
├── Molecules/
│   ├── FormField
│   ├── Card
│   ├── SearchBar
│   └── Navigation Item
├── Organisms/
│   ├── Header
│   ├── Footer
│   ├── Sidebar
│   ├── SkillCard
│   └── ProfileCard
└── Templates/
    ├── Dashboard Layout
    ├── Profile Layout
    └── Settings Layout
```

### コンポーネント命名規則

- **プレフィックス**: コンポーネントタイプを明示
  - `Button/Primary`
  - `Card/Skill`
  - `Form/Input-Text`
- **バリアント**: 状態やスタイルを明記
  - `default`, `hover`, `active`, `disabled`
  - `small`, `medium`, `large`

## アセット管理

### エクスポート設定

Figmaからのアセットエクスポート先：
```
public/assets/
├── images/
│   ├── logos/
│   ├── icons/
│   └── illustrations/
└── fonts/
```

### エクスポート形式
- **アイコン**: SVG (最適化済み)
- **イラスト**: SVG または PNG (2x, 3x)
- **写真**: WebP, JPEG (最適化済み)

## プラグイン推奨

Figma連携を効率化するプラグイン：

1. **Figma to Code**
   - React/Vue/Svelte等のコード生成
   
2. **Contrast**
   - アクセシビリティチェック（色のコントラスト比）

3. **Iconify**
   - アイコンライブラリの統合

4. **Design Tokens**
   - デザイントークンのエクスポート

5. **Content Reel**
   - ダミーコンテンツの自動生成

## ベストプラクティス

### 命名規則
- **わかりやすい名前**: 用途が明確な命名
- **一貫性**: プロジェクト全体で統一された命名
- **階層構造**: フォルダで論理的に整理

### コンポーネント化
- **再利用性**: 汎用的なコンポーネントを作成
- **バリアント活用**: 状態やサイズの違いをバリアントで管理
- **Auto Layout**: レスポンシブなコンポーネント設計

### コラボレーション
- **コメント機能**: デザインレビューでのフィードバック
- **バージョン管理**: 重要な変更時にバージョンを保存
- **ブランチ機能**: 大きな変更は別ブランチで作業

## アクセシビリティ

### デザイン時の考慮事項
- **色のコントラスト**: WCAG AA基準（4.5:1以上）
- **フォントサイズ**: 最小16px推奨
- **タッチターゲット**: 最小44x44px
- **フォーカス状態**: キーボード操作時の視覚的フィードバック

## リソース

### Figmaファイルリンク
- [デザインシステム](#) - 準備中
- [ワイヤーフレーム](#) - 準備中
- [UIデザイン](#) - 準備中
- [プロトタイプ](#) - 準備中

### 参考リンク
- [Figma公式ドキュメント](https://help.figma.com/)
- [Figma Dev Mode](https://help.figma.com/hc/en-us/articles/15023124644247-Guide-to-Dev-Mode)
- [Material Design](https://m3.material.io/)
- [Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)
