# FIFAスタイル Figma クイックスタートガイド

このガイドでは、FIFAスタイルのスキルプロファイルデザインをFigmaで作成する手順を簡潔に説明します。

> 📖 詳細な仕様は [FIFAスタイルデザイン仕様書](./fifa-style-design-spec.md) を参照してください

## 事前準備

### 必要なもの
- [ ] Figmaアカウント（無料版でOK）
- [ ] [FIFA Style Design Spec](./fifa-style-design-spec.md) を一読
- [ ] サンプル画像（アバター用）

### 推奨Figmaプラグイン
1. **Unsplash** - サンプル画像取得用
2. **Iconify** - アイコン取得用
3. **Content Reel** - ダミーテキスト生成用
4. **Stark** - アクセシビリティチェック用

---

## ステップ1: ファイルとページの作成 (5分)

### 1.1 新規ファイル作成
```
1. Figmaを開く
2. 「New design file」をクリック
3. ファイル名を「Skill Profile - FIFA Style Design」に変更
```

### 1.2 ページ構成
左サイドバーでページを作成:
```
📄 Components      ← コンポーネント定義
📄 Desktop         ← デスクトップデザイン (メイン作業)
📄 Tablet          ← タブレットデザイン
📄 Mobile          ← モバイルデザイン
📄 Variations      ← カラーバリエーション
📄 Prototype       ← インタラクティブプロトタイプ
```

---

## ステップ2: カラーとスタイルの設定 (10分)

### 2.1 カラースタイルの作成

右サイドバーの「Local styles」→「+」をクリック:

#### グラデーション作成
1. **Primary Gradient**
   ```
   Type: Linear
   Angle: 135°
   Stop 1: #667eea (0%)
   Stop 2: #764ba2 (50%)
   Stop 3: #f093fb (100%)
   ```

2. **Gold Gradient**
   ```
   Type: Linear
   Angle: 135°
   Stop 1: #f6d365 (0%)
   Stop 2: #fda085 (100%)
   ```

3. **Skill Bar - High**
   ```
   Type: Linear
   Angle: 90°
   Stop 1: #11998e (0%)
   Stop 2: #38ef7d (100%)
   ```

4. **Skill Bar - Medium**
   ```
   Type: Linear
   Angle: 90°
   Stop 1: #4facfe (0%)
   Stop 2: #00f2fe (100%)
   ```

#### 単色スタイル
```
White: #FFFFFF
White-80: #FFFFFF (80% opacity)
Black-30: #000000 (30% opacity)
Bar Background: #FFFFFF (10% opacity)
```

### 2.2 テキストスタイルの作成

「Text」→「Local text styles」→「+」:

| スタイル名 | フォント | サイズ | ウェイト | 行間 |
|-----------|---------|--------|---------|------|
| Heading/L | Inter | 24px | Bold (700) | 1.2 |
| Heading/M | Inter | 20px | SemiBold (600) | 1.3 |
| Body/L | Inter | 16px | Medium (500) | 1.5 |
| Body/M | Inter | 14px | Regular (400) | 1.5 |
| Number/XL | Inter | 20px | Bold (700) | 1.0 |

---

## ステップ3: コンポーネント作成 (30分)

### 3.1 Avatarコンポーネント

**Componentsページで作業**

1. **円形を作成**
   - `O`キーで円ツール選択
   - 150 × 150pxの円を描画
   - Fill: サンプル画像 or #E0E0E0

2. **ボーダー追加**
   - Stroke: 4px
   - Color: White-80
   - Position: Inside

3. **シャドウ追加**
   - Effect → Drop shadow
   - X: 0, Y: 8, Blur: 32
   - Color: Black-30

4. **コンポーネント化**
   - 選択して `Ctrl+Alt+K` (Mac: `⌘+⌥+K`)
   - 名前: `Avatar/Large`

### 3.2 Skill Barコンポーネント

1. **フレーム作成**
   - `F`キーでフレームツール
   - 380 × 40pxのフレーム作成
   - Auto Layout有効化 (`Shift+A`)

2. **スキル名テキスト**
   - `T`キーでテキストツール
   - 「JavaScript」と入力
   - スタイル: Body/L
   - Color: White
   - Width: 140px (固定)

3. **バー背景作成**
   - `R`キーで長方形
   - 200 × 28px
   - Fill: Bar Background
   - Border radius: 14px

4. **バーフィル作成**
   - 長方形: 184 × 28px (92%)
   - Fill: Skill Bar - High gradient
   - Border radius: 14px
   - バー背景の中に配置

5. **数値テキスト**
   - `T`キーでテキスト
   - 「92」と入力
   - スタイル: Number/XL
   - Color: White
   - Width: 40px (右揃え)

6. **Auto Layout調整**
   - フレーム全体を選択
   - Horizontal padding: 0
   - Item spacing: 12px
   - Alignment: Center

7. **コンポーネント化**
   - `Ctrl+Alt+K`
   - 名前: `Skill Bar/Default`

8. **バリアント作成**
   - コンポーネントを右クリック
   - 「Add variant」
   - 名前: `Score=High`, `Score=Medium`, `Score=Low`
   - 各バリアントでグラデーション変更

### 3.3 Profile Cardコンポーネント

1. **カードフレーム作成**
   - `F`キーでフレーム
   - 800 × 500px
   - Fill: Primary Gradient
   - Border radius: 16px

2. **シャドウ追加**
   - Effect → Drop shadow
   - X: 0, Y: 20, Blur: 60
   - Color: Black (30% opacity)

3. **パディング設定**
   - Auto Layout有効化
   - Padding: 48px (全方向)

4. **左側セクション作成**
   - 縦のフレーム (150px幅)
   - Auto Layout: Vertical
   - Item spacing: 24px

5. **Avatar配置**
   - Componentsページから`Avatar/Large`をインスタンス化
   - ドラッグ&ドロップ

6. **基本情報追加**
   ```
   Name: 山田 太郎 (Heading/L, White)
   Role: フルスタックエンジニア (Body/L, White-80)
   Level: 42 (Body/M, White-80)
   XP: 12,500 / 15,000 (Body/M, White-80)
   Rank: Gold (Body/M + Badge)
   ```

7. **右側スキルセクション作成**
   - 縦のフレーム
   - Auto Layout: Vertical
   - Item spacing: 16px

8. **Skill Barインスタンス配置**
   - `Skill Bar/Default`を6-8個配置
   - 各バーのテキストと数値を変更

9. **Overall Score追加**
   - 円形: 80 × 80px
   - Fill: White (20% opacity)
   - テキスト: 「85」(Number/XL)
   - カード右上に配置

10. **コンポーネント化**
    - カード全体を選択
    - `Ctrl+Alt+K`
    - 名前: `Profile Card/Primary`

---

## ステップ4: デスクトップデザイン (15分)

### 4.1 Desktopページでの作業

1. **フレーム作成**
   - `F`キー → Desktop (1440 × 1024)

2. **背景設定**
   - Fill: `#F5F5F5` または暗めの背景

3. **Profile Cardインスタンス配置**
   - Componentsページから`Profile Card/Primary`をコピー
   - デスクトップフレームの中央に配置
   - センター揃え

4. **複数カード配置（オプション）**
   - カードを複製 (`Ctrl+D`)
   - グリッドレイアウトで配置
   - 各カードのデータを変更

---

## ステップ5: レスポンシブ対応 (20分)

### 5.1 Tabletページ

1. **フレーム作成**: Tablet (768 × 1024)
2. **Profile Card配置**: 幅を700pxに調整
3. **Auto Layout再調整**: パディング調整

### 5.2 Mobileページ

1. **フレーム作成**: Phone (375 × 812)

2. **Profile Card複製**: Desktopからコピー

3. **レイアウト変更**:
   ```
   - Auto Layout方向: Horizontal → Vertical
   - Avatar: 120 × 120pxに縮小
   - Skill Bar: 幅100%
   - パディング: 24px
   ```

4. **テキストサイズ調整**:
   - Name: 20px (Heading/M)
   - その他: 14px (Body/M)

---

## ステップ6: インタラクション追加 (15分)

### 6.1 ホバー状態の作成

1. **Profile Cardバリアント追加**
   - コンポーネントを右クリック
   - 「Add variant」
   - Property: `State=Default`, `State=Hover`

2. **Hover状態のスタイル変更**:
   - Y位置: -8px
   - シャドウ: Y=30, Blur=80に変更

### 6.2 プロトタイプ設定

1. **Prototypeページに移動**

2. **Desktop画面を複製**
   - Default状態とHover状態を並べる

3. **インタラクション設定**:
   - Default → Hover: While hovering
   - Transition: Smart animate, 300ms

4. **プレビュー**:
   - 右上の再生ボタンでプレビュー

---

## ステップ7: カラーバリエーション (10分)

### 7.1 Variationsページ

Profile Cardを複製して各バリアント作成:

1. **Gold Tier**
   - Background: Gold Gradient
   - Overall Score: 90+

2. **Silver Tier**
   - Background: Silver Gradient (`#a8edea` → `#fed6e3`)
   - Overall Score: 80-89

3. **Bronze Tier**
   - Background: Bronze Gradient (`#ff9a56` → `#ff6a88`)
   - Overall Score: 70-79

4. **Dark Mode**
   - Background: Dark Gradient (`#141E30` → `#243B55`)
   - テキスト: White (100%)

---

## ステップ8: エクスポートと共有 (5分)

### 8.1 開発用エクスポート

1. **アセット選択**
   - Export settings設定
   - Avatar: 2x, 3x PNG
   - Icons: SVG

2. **エクスポート**
   - 右下「Export」ボタン
   - フォルダ選択

### 8.2 Figmaリンクの共有

1. **右上「Share」ボタン**
2. 「Copy link」
3. README.mdのリンクを更新:
   ```markdown
   - **FIFAスタイルデザイン**: [リンク](https://www.figma.com/file/YOUR_KEY/)
   ```

### 8.3 Dev Mode有効化

1. **右上の「Dev Mode」切り替え**
2. 開発者がインスペクト可能に
3. CSS/コードのコピーが可能

---

## よくある質問 (FAQ)

### Q1: Auto Layoutがうまく動かない
**A:** フレームを選択して`Shift+A`でAuto Layoutを有効化。アイテムの方向（Horizontal/Vertical）を確認。

### Q2: グラデーションがずれる
**A:** グラデーションの角度を135°に設定。Stop位置を正確に入力。

### Q3: コンポーネントが更新されない
**A:** メインコンポーネントを編集。インスタンスは自動更新されます。右クリック→「Reset instance」で初期状態に戻せます。

### Q4: テキストが見づらい
**A:** テキストにシャドウを追加 (X:0, Y:2, Blur:4, Black 30%)。または背景に半透明オーバーレイを追加。

### Q5: モバイルでレイアウトが崩れる
**A:** Auto Layoutの「Resizing」を確認。「Hug contents」または「Fill container」に設定。

---

## 次のステップ

### Figma作成完了後
- [ ] デザインレビューの実施
- [ ] プロトタイプでユーザーテスト
- [ ] README.mdのリンク更新
- [ ] 開発チームへの共有

### 実装フェーズへ
- [ ] アセットのエクスポート
- [ ] デザイントークンの確認
- [ ] コンポーネント実装開始
- [ ] [実装ガイド](./fifa-style-design-spec.md#実装時の注意点) を参照

---

## サポート

質問や問題がある場合:
1. [FIFA Style Design Spec](./fifa-style-design-spec.md) を確認
2. [Figma Help Center](https://help.figma.com/) で検索
3. チームメンバーに相談

---

## タイムライン目安

| フェーズ | 所要時間 | 累計 |
|---------|---------|------|
| ファイル・ページ作成 | 5分 | 5分 |
| カラー・スタイル設定 | 10分 | 15分 |
| コンポーネント作成 | 30分 | 45分 |
| デスクトップデザイン | 15分 | 60分 |
| レスポンシブ対応 | 20分 | 80分 |
| インタラクション追加 | 15分 | 95分 |
| カラーバリエーション | 10分 | 105分 |
| エクスポート・共有 | 5分 | **110分** |

**合計: 約2時間** で完成します！

---

**📝 作業チェックリスト**: [fifa-style-design-spec.md](./fifa-style-design-spec.md#チェックリスト) を参照
