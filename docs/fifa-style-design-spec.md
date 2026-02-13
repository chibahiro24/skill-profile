# FIFAスタイル スキルプロファイル デザイン仕様書

## 概要

このドキュメントでは、FIFAなどのスポーツゲームのUIデザインを参考にした、スキルプロファイル表示のデザイン仕様を定義します。

## デザインコンセプト

### 参考イメージ
- FIFAの選手カード（Ultimate Team）
- 直感的な数値バー表示
- ゲーミフィケーション要素
- 高級感のあるグラデーション

### 主な特徴
- **視覚的インパクト**: 一目でスキルレベルが分かる
- **数値の明確性**: バーと数値の両方で表示
- **階層的情報構造**: 重要な情報が目立つレイアウト
- **モダンな外観**: グラデーション、シャドウ、グロー効果

---

## レイアウト構成

### 全体構造

```
┌────────────────────────────────────────────────────────┐
│  [Profile Card]                                        │
│  ┌──────────────┬─────────────────────────────────┐   │
│  │              │                                 │   │
│  │   Avatar     │  Name                           │   │
│  │   (150x150)  │  Role/Title                     │   │
│  │              │  Overall Score: 85              │   │
│  │              │                                 │   │
│  ├──────────────┼─────────────────────────────────┤   │
│  │              │                                 │   │
│  │  Basic Info  │  Skills Section                 │   │
│  │              │  ┌─────────────────────────┐    │   │
│  │  Level: 42   │  │ JavaScript    [▓▓▓▓▓] 92 │   │
│  │  XP: 12,500  │  │ TypeScript    [▓▓▓▓░] 85 │   │
│  │  Rank: Gold  │  │ React         [▓▓▓▓▓] 90 │   │
│  │              │  │ Node.js       [▓▓▓▓░] 88 │   │
│  │              │  │ Python        [▓▓▓░░] 75 │   │
│  │              │  │ Leadership    [▓▓▓▓░] 82 │   │
│  │              │  └─────────────────────────┘    │   │
│  │              │                                 │   │
│  └──────────────┴─────────────────────────────────┘   │
└────────────────────────────────────────────────────────┘
```

---

## コンポーネント詳細設計

### 1. プロフィールカード (Profile Card)

#### サイズ・配置
- **カード全体**: 800px × 500px（デスクトップ）
- **背景**: グラデーション背景 + ノイズテクスチャ
- **ボーダー半径**: 16px
- **シャドウ**: 0 20px 60px rgba(0, 0, 0, 0.3)

#### 背景グラデーション
```css
background: linear-gradient(135deg, 
  #667eea 0%, 
  #764ba2 50%, 
  #f093fb 100%
);
```

オプションカラーバリエーション:
- **Gold Tier**: `#f6d365` → `#fda085`
- **Silver Tier**: `#a8edea` → `#fed6e3`
- **Bronze Tier**: `#ff9a56` → `#ff6a88`
- **Dark Mode**: `#141E30` → `#243B55`

---

### 2. アバターセクション (Avatar Section)

#### 配置
- **位置**: 左上、カード左側
- **サイズ**: 150px × 150px
- **形状**: 円形 (border-radius: 50%) または角丸四角 (border-radius: 12px)

#### スタイル
- **ボーダー**: 4px solid rgba(255, 255, 255, 0.3)
- **ボックスシャドウ**: 0 8px 32px rgba(0, 0, 0, 0.2)
- **背景**: 画像がない場合はイニシャルを表示

#### プレースホルダー
```
[A]  ← イニシャル (大きなフォント)
```

---

### 3. 基本情報セクション (Basic Info Section)

#### 配置
- **位置**: アバターの下
- **幅**: アバターと同じ幅（150px）

#### 表示項目
```
Name: 山田 太郎
Role: フルスタックエンジニア
Level: 42
XP: 12,500 / 15,000
Rank: Gold
```

#### スタイル
- **フォントサイズ**:
  - Name: 24px (bold)
  - Role: 16px (medium)
  - その他: 14px (regular)
- **カラー**: White / rgba(255, 255, 255, 0.9)
- **行間**: 1.5

#### レベルインジケーター
- プログレスバー表示
- 現在XP / 次のレベルまでのXP

#### ランクバッジ
- アイコン + テキスト
- カラーコード:
  - Bronze: `#CD7F32`
  - Silver: `#C0C0C0`
  - Gold: `#FFD700`
  - Platinum: `#E5E4E2`
  - Diamond: `#B9F2FF`

---

### 4. 総合スコアセクション (Overall Score)

#### 配置
- **位置**: カード右上、名前の横
- **サイズ**: 80px × 80px

#### デザイン
- 大きな数値（例: 85）
- 円形または六角形の背景
- グロー効果

```
  ┌────┐
  │ 85 │  ← 大きく目立つ
  └────┘
  Overall
```

#### スコア色分け
- 90-100: Gold (#FFD700)
- 80-89: Silver (#C0C0C0)
- 70-79: Bronze (#CD7F32)
- 60-69: Green (#4CAF50)
- 0-59: Gray (#9E9E9E)

---

### 5. スキルバーセクション (Skills Section)

#### 配置
- **位置**: カード右側、メインエリア
- **幅**: カード幅の60-65%

#### スキルバー構成
各スキル項目:
```
┌─────────────────────────────────────┐
│ JavaScript          [▓▓▓▓▓] 92     │
└─────────────────────────────────────┘
```

#### スキルバー詳細仕様

**構造**:
```
[スキル名(140px)] [バー(200px)] [数値(40px)]
```

**バーデザイン**:
- **高さ**: 28px
- **背景色**: rgba(255, 255, 255, 0.1)
- **フィル色**: グラデーション
  - 90-100: `#11998e` → `#38ef7d` (Green)
  - 80-89: `#4facfe` → `#00f2fe` (Blue)
  - 70-79: `#fa709a` → `#fee140` (Pink-Yellow)
  - 60-69: `#fccb90` → `#d57eeb` (Orange-Purple)
  - 0-59: `#e0e0e0` → `#9e9e9e` (Gray)
- **ボーダー半径**: 14px
- **アニメーション**: ページロード時に左から右へフィル

**数値表示**:
- **フォントサイズ**: 20px (bold)
- **カラー**: White
- **テキストシャドウ**: 0 2px 4px rgba(0, 0, 0, 0.3)

**スキル名**:
- **フォントサイズ**: 16px (medium)
- **カラー**: White / rgba(255, 255, 255, 0.95)
- **配置**: 左揃え

#### スキルカテゴリー

スキルを以下のカテゴリーに分類:

1. **技術スキル** (Technical Skills)
   - プログラミング言語
   - フレームワーク/ライブラリ
   - ツール/プラットフォーム

2. **ソフトスキル** (Soft Skills)
   - リーダーシップ
   - コミュニケーション
   - 問題解決能力

3. **業務スキル** (Professional Skills)
   - プロジェクト管理
   - ドキュメンテーション
   - テスト/QA

#### 表示優先順位
- 上位6-8スキルを表示
- 「もっと見る」ボタンで全スキルを展開

---

### 6. ホバー・インタラクション効果

#### カード全体
- **ホバー時**:
  - 軽く浮き上がる (translateY: -8px)
  - シャドウ強化: 0 30px 80px rgba(0, 0, 0, 0.4)
  - トランジション: 0.3s ease-out

#### スキルバー
- **ホバー時**:
  - バーが微妙に拡大 (scaleX: 1.02)
  - グロー効果追加
  - ツールチップ表示（詳細情報）

#### ツールチップ内容
```
┌─────────────────────────┐
│ JavaScript              │
│ レベル: エキスパート     │
│ 経験年数: 5年           │
│ 最終更新: 2026-02-10    │
└─────────────────────────┘
```

---

### 7. レスポンシブデザイン

#### デスクトップ (1024px以上)
- カードサイズ: 800px × 500px
- 横並びレイアウト

#### タブレット (768px - 1023px)
- カードサイズ: 100% × auto
- 横並び維持、パディング調整

#### モバイル (767px以下)
- カードサイズ: 100% × auto
- **縦並びレイアウトに変更**:
  ```
  ┌─────────────────┐
  │   Avatar        │
  │   Name          │
  │   Overall: 85   │
  ├─────────────────┤
  │   Basic Info    │
  ├─────────────────┤
  │   Skills        │
  │   [bars...]     │
  └─────────────────┘
  ```
- アバターサイズ: 120px × 120px
- スキルバー幅: 100%

---

## デザイントークンマッピング

既存の`design-tokens.json`を活用:

### カラー
```json
{
  "card-background-primary": "linear-gradient(135deg, #667eea, #764ba2)",
  "card-background-gold": "linear-gradient(135deg, #f6d365, #fda085)",
  "skill-bar-bg": "rgba(255, 255, 255, 0.1)",
  "skill-bar-high": "linear-gradient(90deg, #11998e, #38ef7d)",
  "skill-bar-medium": "linear-gradient(90deg, #4facfe, #00f2fe)",
  "text-primary": "#FFFFFF",
  "text-secondary": "rgba(255, 255, 255, 0.8)"
}
```

### タイポグラフィ
- **Name**: `fontSize.2xl` (24px) + `fontWeight.bold` (700)
- **Role**: `fontSize.base` (16px) + `fontWeight.medium` (500)
- **Skill Label**: `fontSize.base` (16px) + `fontWeight.medium` (500)
- **Skill Value**: `fontSize.xl` (20px) + `fontWeight.bold` (700)

### スペーシング
- **カードパディング**: `spacing.2xl` (48px)
- **セクション間**: `spacing.lg` (24px)
- **スキルバー間**: `spacing.md` (16px)
- **要素間**: `spacing.sm` (8px)

### シャドウ
- **カード**: `shadows.xl`
- **アバター**: `shadows.lg`
- **ホバー**: カスタム `0 30px 80px rgba(0, 0, 0, 0.4)`

---

## Figma作成手順

### ステップ1: 新規ファイル作成
1. Figmaで新規ファイル作成: `Skill Profile - FIFA Style Design`
2. ページ構成:
   - Page 1: Components (コンポーネント定義)
   - Page 2: Desktop (デスクトップデザイン)
   - Page 3: Tablet (タブレットデザイン)
   - Page 4: Mobile (モバイルデザイン)
   - Page 5: Variations (カラーバリエーション)

### ステップ2: デザイントークンのセットアップ
1. カラースタイルの作成:
   - `design-tokens.json`から各カラーを登録
   - グラデーションも登録
2. テキストスタイルの作成:
   - Heading/L (24px, Bold)
   - Body/M (16px, Medium)
   - Body/S (14px, Regular)
   - Number/XL (20px, Bold)

### ステップ3: コンポーネント作成

#### 3.1 Avatar Component
- サイズバリアント: Large (150px), Medium (120px), Small (80px)
- 形状バリアント: Circle, Rounded Square
- 状態: Default, Placeholder

#### 3.2 Skill Bar Component
- プロパティ:
  - スキル名 (Text)
  - スキル値 (Number: 0-100)
  - カテゴリー (Dropdown: Technical, Soft, Professional)
- 自動幅調整: Auto Layout
- バリアント: スコア範囲別のカラー

#### 3.3 Rank Badge Component
- バリアント: Bronze, Silver, Gold, Platinum, Diamond
- アイコン + テキスト
- サイズ: 24px × 24px

#### 3.4 Overall Score Component
- 大きな数値表示
- 円形背景
- グロー効果（エフェクト）

#### 3.5 Profile Card Component
- メインコンポーネント
- すべてのサブコンポーネントを含む
- Auto Layout使用
- レスポンシブ対応

### ステップ4: デスクトップデザイン
1. フレーム作成: 1440px × 1024px
2. プロフィールカード配置
3. レイアウトグリッドの設定
4. スキルバーの配置（6-8本）

### ステップ5: タブレット・モバイル対応
1. タブレットフレーム: 768px × 1024px
2. モバイルフレーム: 375px × 812px
3. レスポンシブレイアウトの調整
4. Auto Layoutで自動リサイズ

### ステップ6: インタラクション・プロトタイプ
1. ホバー状態の作成
2. スキルバーのアニメーション
3. ツールチップの表示
4. 「もっと見る」展開アニメーション

### ステップ7: カラーバリエーション
1. Gold Tierカード
2. Silver Tierカード
3. Bronze Tierカード
4. Dark Modeバージョン

---

## エクスポート設定

### 開発用アセット
1. **アイコン**: SVG
2. **アバター画像**: PNG 2x, 3x
3. **背景グラデーション**: CSS/SVG
4. **コンポーネントスペック**: JSON

### エクスポート先
```
public/assets/
├── images/
│   ├── avatars/
│   └── badges/
└── icons/
```

---

## 実装時の注意点

### CSS/Styled Components
```jsx
const ProfileCard = styled.div`
  width: 800px;
  height: 500px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%);
  border-radius: 16px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
  display: flex;
  padding: 48px;
  transition: transform 0.3s ease-out, box-shadow 0.3s ease-out;
  
  &:hover {
    transform: translateY(-8px);
    box-shadow: 0 30px 80px rgba(0, 0, 0, 0.4);
  }
`;

const SkillBar = styled.div`
  display: flex;
  align-items: center;
  margin-bottom: 16px;
`;

const SkillBarFill = styled.div`
  width: ${props => props.value}%;
  height: 28px;
  background: ${props => getGradientByScore(props.value)};
  border-radius: 14px;
  transition: transform 0.3s ease-out;
  
  &:hover {
    transform: scaleX(1.02);
  }
`;
```

### アニメーション
```jsx
// スキルバーのフィルアニメーション
import { motion } from 'framer-motion';

<motion.div
  initial={{ width: 0 }}
  animate={{ width: `${value}%` }}
  transition={{ duration: 1, ease: "easeOut" }}
/>
```

---

## 拡張アイデア

### 将来的な機能追加
1. **3Dカード効果**: カーソル位置に応じた3D回転
2. **パーティクル効果**: 背景にキラキラエフェクト
3. **スキル比較モード**: 複数カードの並列表示
4. **テーマカスタマイズ**: ユーザーが背景色を選択可能
5. **実績バッジ表示**: 獲得した実績をカードに表示
6. **スキルレーダーチャート**: 別ビューでレーダーチャート表示
7. **経験値アニメーション**: レベルアップ時のエフェクト
8. **シェア機能**: SNSシェア用の画像生成

---

## 参考リンク

### デザインインスピレーション
- [FIFA 23 Ultimate Team Card Design](https://www.ea.com/games/fifa/fifa-23)
- [Dribbble - Player Card Designs](https://dribbble.com/search/player-card)
- [Behance - Sports UI](https://www.behance.net/search/projects?search=sports%20ui)

### Figmaチュートリアル
- [Figma Auto Layout Guide](https://help.figma.com/hc/en-us/articles/360040451373)
- [Figma Component Variants](https://help.figma.com/hc/en-us/articles/360056440594)
- [Figma Prototyping](https://help.figma.com/hc/en-us/articles/360040314193)

### 実装参考
- [Framer Motion - Animations](https://www.framer.com/motion/)
- [React Spring - Physics-based animations](https://www.react-spring.dev/)
- [CSS Gradient Generator](https://cssgradient.io/)

---

## チェックリスト

### デザインフェーズ
- [ ] Figmaファイル作成
- [ ] カラースタイル登録
- [ ] テキストスタイル登録
- [ ] Avatarコンポーネント作成
- [ ] Skill Barコンポーネント作成
- [ ] Profile Cardコンポーネント作成
- [ ] デスクトップデザイン完成
- [ ] タブレットデザイン完成
- [ ] モバイルデザイン完成
- [ ] インタラクション設定
- [ ] プロトタイプ作成
- [ ] デザインレビュー

### 開発フェーズ
- [ ] アセットエクスポート
- [ ] コンポーネント実装
- [ ] スタイリング
- [ ] アニメーション実装
- [ ] レスポンシブ対応
- [ ] ホバー効果実装
- [ ] アクセシビリティ対応
- [ ] パフォーマンス最適化
- [ ] ブラウザテスト
- [ ] デザインQA

---

## 更新履歴

| バージョン | 日付 | 変更内容 | 作成者 |
|-----------|------|---------|--------|
| 1.0 | 2026-02-13 | 初版作成 | @hiroyuki |

---

**このドキュメントは、FIFAスタイルのスキルプロファイルデザインの完全な仕様書です。Figmaでの作成から実装まで、この仕様に従って進めてください。**
