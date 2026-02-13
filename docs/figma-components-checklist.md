# Figmaコンポーネントチェックリスト

このチェックリストは、デザインシステムで作成すべきコンポーネントの一覧です。

## ✅ Atoms（原子）- 最小単位の要素

### ボタン
- [ ] Primary Button
  - [ ] Small
  - [ ] Medium
  - [ ] Large
  - [ ] Default状態
  - [ ] Hover状態
  - [ ] Active状態
  - [ ] Disabled状態
  - [ ] Loading状態
  - [ ] アイコン付きバリアント
- [ ] Secondary Button（全状態）
- [ ] Outline Button（全状態）
- [ ] Text Button（全状態）
- [ ] Icon Button（全状態）
- [ ] Floating Action Button

### 入力フィールド
- [ ] Text Input
  - [ ] Default
  - [ ] Focus
  - [ ] Error
  - [ ] Disabled
  - [ ] With Icon (left/right)
- [ ] Password Input（全状態）
- [ ] Text Area
  - [ ] Default
  - [ ] Focus
  - [ ] Error
  - [ ] Disabled
- [ ] Select / Dropdown
  - [ ] Closed
  - [ ] Open
  - [ ] Disabled
- [ ] Checkbox
  - [ ] Unchecked
  - [ ] Checked
  - [ ] Indeterminate
  - [ ] Disabled
- [ ] Radio Button
  - [ ] Unselected
  - [ ] Selected
  - [ ] Disabled
- [ ] Toggle Switch
  - [ ] Off
  - [ ] On
  - [ ] Disabled
- [ ] Slider
  - [ ] Default
  - [ ] Disabled
  - [ ] Range Slider

### タイポグラフィ
- [ ] Heading 1 (H1)
- [ ] Heading 2 (H2)
- [ ] Heading 3 (H3)
- [ ] Heading 4 (H4)
- [ ] Heading 5 (H5)
- [ ] Heading 6 (H6)
- [ ] Body Large
- [ ] Body Regular
- [ ] Body Small
- [ ] Caption
- [ ] Overline
- [ ] Link
  - [ ] Default
  - [ ] Hover
  - [ ] Visited

### アイコン
- [ ] ナビゲーションアイコン（ホーム、設定、プロフィール等）
- [ ] アクションアイコン（編集、削除、追加、検索等）
- [ ] ステータスアイコン（成功、警告、エラー、情報）
- [ ] ソーシャルアイコン（GitHub, LinkedIn, Twitter等）
- [ ] スキルカテゴリアイコン
- [ ] 矢印・方向アイコン
- [ ] メニューアイコン（ハンバーガー、ケバブ、ミートボール）

### バッジ・チップ
- [ ] Badge
  - [ ] Default
  - [ ] Primary
  - [ ] Success
  - [ ] Warning
  - [ ] Error
  - [ ] Info
  - [ ] Small/Medium/Large
- [ ] Chip/Tag
  - [ ] Default
  - [ ] Clickable
  - [ ] Removable（×ボタン付き）
  - [ ] With Icon

### アバター
- [ ] Avatar Small (32x32)
- [ ] Avatar Medium (48x48)
- [ ] Avatar Large (64x64)
- [ ] Avatar XLarge (96x96)
- [ ] 画像あり
- [ ] 画像なし（イニシャル表示）
- [ ] ステータスインジケータ付き

### ローディング・プログレス
- [ ] Spinner
  - [ ] Small
  - [ ] Medium
  - [ ] Large
- [ ] Progress Bar
  - [ ] Determinate（進捗率表示）
  - [ ] Indeterminate（不定）
- [ ] Skeleton Loader
  - [ ] Text
  - [ ] Avatar
  - [ ] Card
  - [ ] Image

### その他
- [ ] Divider（水平/垂直）
- [ ] Tooltip
- [ ] Link（Internal/External）

---

## ✅ Molecules（分子）- Atoms の組み合わせ

### フォーム要素
- [ ] Form Field
  - [ ] Label
  - [ ] Input
  - [ ] Helper Text
  - [ ] Error Message
  - [ ] Required Indicator
- [ ] Search Bar
  - [ ] Search Icon
  - [ ] Input Field
  - [ ] Clear Button
  - [ ] Filter Button（オプション）

### カード
- [ ] Basic Card
  - [ ] Header
  - [ ] Content
  - [ ] Footer
- [ ] Skill Card
  - [ ] Icon
  - [ ] Skill Name
  - [ ] Level Indicator
  - [ ] Experience Years
  - [ ] Action Buttons
- [ ] Project Card
  - [ ] Thumbnail（オプション）
  - [ ] Title
  - [ ] Description
  - [ ] Tags
  - [ ] Date
- [ ] Achievement Card
  - [ ] Badge/Icon
  - [ ] Title
  - [ ] Date
  - [ ] Description
- [ ] User Card
  - [ ] Avatar
  - [ ] Name
  - [ ] Title/Role
  - [ ] Stats
  - [ ] Action Button

### リストアイテム
- [ ] List Item
  - [ ] Icon（オプション）
  - [ ] Primary Text
  - [ ] Secondary Text（オプション）
  - [ ] Action Icons（オプション）
- [ ] Notification Item
  - [ ] Avatar/Icon
  - [ ] Message
  - [ ] Time
  - [ ] Read/Unread状態

### ナビゲーション
- [ ] Navigation Item
  - [ ] Icon
  - [ ] Label
  - [ ] Badge（オプション）
  - [ ] Default状態
  - [ ] Active状態
  - [ ] Hover状態
- [ ] Breadcrumb Item
- [ ] Tab Item
  - [ ] Label
  - [ ] Icon（オプション）
  - [ ] Default状態
  - [ ] Active状態
  - [ ] Hover状態
  - [ ] Disabled状態

### メディア
- [ ] Image with Caption
- [ ] Video Player Controls
- [ ] File Upload Area
  - [ ] ドロップゾーン
  - [ ] アップロードボタン
  - [ ] ファイルリスト

---

## ✅ Organisms（有機体）- 複雑なコンポーネント

### ヘッダー・ナビゲーション
- [ ] Header (Desktop)
  - [ ] Logo
  - [ ] Navigation Menu
  - [ ] Search Bar
  - [ ] User Menu
  - [ ] Notifications
- [ ] Header (Mobile)
  - [ ] Logo
  - [ ] Hamburger Menu
  - [ ] User Avatar
- [ ] Sidebar Navigation
  - [ ] Logo
  - [ ] Navigation Items
  - [ ] User Info
  - [ ] Footer Actions
  - [ ] Collapsed状態
  - [ ] Expanded状態
- [ ] Mobile Navigation Drawer
- [ ] Footer
  - [ ] Links
  - [ ] Social Media Icons
  - [ ] Copyright

### プロフィール表示
- [ ] Profile Header
  - [ ] Cover Image
  - [ ] Avatar
  - [ ] Name & Title
  - [ ] Bio
  - [ ] Stats（フォロワー、スキル数等）
  - [ ] Action Buttons
- [ ] Profile Card Compact
  - [ ] Avatar
  - [ ] Name
  - [ ] Title
  - [ ] Skills Summary
  - [ ] Contact Button
- [ ] Skills Overview Section
  - [ ] Skill Cards Grid
  - [ ] Filter Options
  - [ ] Sort Options

### データ表示
- [ ] Data Table
  - [ ] Header Row
  - [ ] Data Rows
  - [ ] Pagination
  - [ ] Sort Icons
  - [ ] Selection Checkboxes（オプション）
  - [ ] Empty State
- [ ] Stats Dashboard Widget
  - [ ] Title
  - [ ] Main Value
  - [ ] Change Indicator
  - [ ] Mini Chart
- [ ] Chart Container
  - [ ] Title
  - [ ] Legend
  - [ ] Chart Area
  - [ ] Filters/Controls

### フォーム
- [ ] Login Form
  - [ ] Email Input
  - [ ] Password Input
  - [ ] Remember Me
  - [ ] Submit Button
  - [ ] Forgot Password Link
  - [ ] OAuth Buttons
- [ ] Sign Up Form
- [ ] Profile Edit Form
- [ ] Skill Add/Edit Form
  - [ ] Skill Name Input
  - [ ] Category Select
  - [ ] Level Slider
  - [ ] Experience Input
  - [ ] Certificate Upload

### モーダル・ダイアログ
- [ ] Modal
  - [ ] Header（Title + Close Button）
  - [ ] Content Area
  - [ ] Footer（Actions）
  - [ ] Backdrop/Overlay
- [ ] Confirmation Dialog
  - [ ] Icon（警告、情報等）
  - [ ] Title
  - [ ] Message
  - [ ] Cancel Button
  - [ ] Confirm Button
- [ ] Alert/Toast Notification
  - [ ] Icon
  - [ ] Message
  - [ ] Close Button
  - [ ] Success/Warning/Error/Info バリアント

### その他複雑なコンポーネント
- [ ] Timeline Component
  - [ ] Timeline Items
  - [ ] Dates
  - [ ] Content
- [ ] Accordion
  - [ ] Header
  - [ ] Content
  - [ ] Expand/Collapse Icon
- [ ] Tabs Container
  - [ ] Tab Headers
  - [ ] Tab Content
- [ ] Dropdown Menu
  - [ ] Trigger Button
  - [ ] Menu Items
  - [ ] Dividers
  - [ ] Icons
- [ ] User Menu Dropdown
  - [ ] User Info
  - [ ] Menu Items
  - [ ] Logout Button
- [ ] Stepper (ウィザード)
  - [ ] Step Indicators
  - [ ] Content Area
  - [ ] Navigation Buttons

---

## ✅ Templates（テンプレート）- ページレイアウト

### レイアウト
- [ ] Dashboard Layout
  - [ ] Header
  - [ ] Sidebar
  - [ ] Main Content Area
  - [ ] Responsive Behavior
- [ ] Profile Layout
  - [ ] Header
  - [ ] Profile Header
  - [ ] Content Tabs
  - [ ] Main Content
- [ ] Settings Layout
  - [ ] Header
  - [ ] Side Navigation
  - [ ] Content Area
- [ ] Full Width Layout（ランディング等）
- [ ] Centered Layout（認証画面等）

### 特殊画面
- [ ] Empty State
  - [ ] Illustration/Icon
  - [ ] Message
  - [ ] Call to Action
- [ ] Error Page (404, 500, etc.)
  - [ ] Error Code
  - [ ] Message
  - [ ] Home Button
- [ ] Loading Screen
- [ ] Maintenance Page

---

## ✅ デザインシステム基盤

### カラーパレット
- [ ] Primary Colors (50-900)
- [ ] Secondary Colors (50-900)
- [ ] Neutral Colors (0-1000)
- [ ] Success Color
- [ ] Warning Color
- [ ] Error Color
- [ ] Info Color
- [ ] Background Colors
- [ ] Text Colors
- [ ] Border Colors

### タイポグラフィスタイル
- [ ] Font Family (Primary, Monospace)
- [ ] Font Sizes (xs - 6xl)
- [ ] Font Weights (Light - ExtraBold)
- [ ] Line Heights
- [ ] Letter Spacing

### スペーシングシステム
- [ ] Spacing Scale (0, xs, sm, md, lg, xl, 2xl, 3xl, 4xl)
- [ ] Padding Utilities
- [ ] Margin Utilities
- [ ] Gap Utilities

### エフェクト
- [ ] Shadow Styles (sm, md, lg, xl)
- [ ] Border Radius (none, sm, md, lg, xl, 2xl, full)
- [ ] Opacity Values
- [ ] Blur Effects

### グリッド・レイアウト
- [ ] 12 Column Grid
- [ ] Responsive Breakpoints
- [ ] Container Max Widths
- [ ] Gutter Sizes

---

## ✅ アイコンライブラリ

### 必須アイコン（最低限）
- [ ] Home
- [ ] Dashboard
- [ ] Profile / User
- [ ] Settings / Gear
- [ ] Search
- [ ] Notification / Bell
- [ ] Menu / Hamburger
- [ ] Close / X
- [ ] Chevron (Up/Down/Left/Right)
- [ ] Arrow (Up/Down/Left/Right)
- [ ] Plus / Add
- [ ] Edit / Pencil
- [ ] Delete / Trash
- [ ] Check / Checkmark
- [ ] Star (Filled/Outline)
- [ ] Heart (Filled/Outline)
- [ ] Share
- [ ] Download
- [ ] Upload
- [ ] Filter
- [ ] Sort
- [ ] More (Horizontal/Vertical dots)
- [ ] Info
- [ ] Warning / Alert
- [ ] Error / X Circle
- [ ] Success / Check Circle
- [ ] Help / Question
- [ ] Calendar
- [ ] Clock / Time
- [ ] Location / Pin
- [ ] Email / Envelope
- [ ] Phone
- [ ] Link / Chain
- [ ] External Link
- [ ] Lock / Security
- [ ] Unlock
- [ ] Eye (Show)
- [ ] Eye Slash (Hide)
- [ ] File / Document
- [ ] Folder
- [ ] Image / Photo
- [ ] Video
- [ ] Chart / Graph
- [ ] List
- [ ] Grid
- [ ] GitHub
- [ ] LinkedIn
- [ ] Twitter

---

## ✅ 状態バリエーション確認

すべてのインタラクティブコンポーネントについて以下を確認：

- [ ] Default（デフォルト）
- [ ] Hover（マウスオーバー）
- [ ] Focus（フォーカス時）
- [ ] Active（アクティブ/押下時）
- [ ] Disabled（無効化時）
- [ ] Loading（ローディング中）
- [ ] Error（エラー時）
- [ ] Success（成功時）

---

## ✅ レスポンシブ対応確認

各コンポーネント・レイアウトについて以下のブレークポイントで確認：

- [ ] Mobile (320px - 767px)
- [ ] Tablet (768px - 1023px)
- [ ] Desktop (1024px+)
- [ ] Wide (1440px+)

---

## ✅ アクセシビリティ確認

- [ ] 色のコントラスト比が WCAG AA 基準を満たしている（4.5:1以上）
- [ ] タッチターゲットが最低44x44px
- [ ] フォーカス状態が明確に視覚化されている
- [ ] 適切なフォントサイズ（本文16px以上）
- [ ] アイコンのみのボタンにはツールチップあり

---

## ✅ ドキュメント作成

- [ ] コンポーネントの使用例
- [ ] バリアントの説明
- [ ] 使用上の注意点
- [ ] コード例（開発者向け）
- [ ] プロパティ一覧

---

## 作業の進め方

### ステップ1: 基盤の構築（Week 1）
1. カラーパレットの作成
2. タイポグラフィスタイルの設定
3. スペーシングシステムの定義
4. エフェクトスタイルの作成

### ステップ2: Atoms の作成（Week 2）
1. ボタン
2. 入力フィールド
3. アイコン
4. バッジ・チップ
5. アバター

### ステップ3: Molecules の作成（Week 3）
1. フォームフィールド
2. カード類
3. リストアイテム
4. ナビゲーションアイテム

### ステップ4: Organisms の作成（Week 4-5）
1. ヘッダー・サイドバー
2. プロフィール表示
3. データテーブル
4. フォーム
5. モーダル

### ステップ5: Templates & Pages の作成（Week 6-8）
1. レイアウトテンプレート
2. 主要画面のデザイン
3. レスポンシブ対応
4. プロトタイプ作成

---

## 参考リンク

- [Atomic Design by Brad Frost](https://atomicdesign.bradfrost.com/)
- [Material Design Components](https://m3.material.io/components)
- [Apple Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)
- [Figma Best Practices](https://www.figma.com/best-practices/)
