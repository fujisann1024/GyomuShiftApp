# TailWindCSSの導入

[参考サイト](https://tailwindcss.com/docs/installation/framework-guides/nuxt)
 - この手順で導入しないとVSCodeの拡張機能に反応しない

[Tailwindの活用](https://zenn.dev/var/articles/3cd463f2388a35?utm_source=chatgpt.com)

## ベストプラクティス

 - Tailwindのユーティリティクラスは一定のルールでグループ化すると可読性が向上
   - レイアウト関連 (flex, grid, block, hidden など)
   - サイズ (w-, h-, max-w-, max-h-)
   - マージン & パディング (m-, p-)
   - タイポグラフィ (text-, font-, leading-, tracking-)
   - カラー & 背景 (bg-, text-, border-, shadow-)
   - その他 (rounded-, overflow-, cursor-, opacity- など)
 - @apply を活用してコンポーネント化
   - ユーティリティクラスが長くなると可読性が下がるので、@apply を活用してスタイルを整理
   - ```css
      .btn-primary {
       @apply bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded;
      }
     ```
   - ```html
      <button class="btn-primary">ボタン</button>
     ```
 - theme() を使ってカスタムカラーを適用
 - container クラスを活用してレスポンシブ対応
 - group を活用して親要素のホバーを制御
 - aspect-ratio を使って画像の比率を固定

<details>
<summary>入門</summary>


## 入門

### バリアント

 - **バリアント**:条件付きでスタイル適用
   - 擬似クラス
   - 擬似要素
   - メディアと機能のクエリ 
   - 属性セレクター
   - 子セレクター

```html
<!--ホバーしたときに色を変更する-->
<button class="bg-sky-500 hover:bg-sky-700 ...">Save changes</button>
```

### レスポンシブ対応

 - **ブレイクポイント**:異なるデバイス（パソコン、タブレット、スマートフォンなど）の画面サイズに応じて、ウェブサイトのレイアウトやデザインを切り替えるための境界となるポイント

|ブレークポイントのプレフィックス|最小幅|
|:--|:--|
|sm|40rem(640px)|
|md|48rem(768px)|  タブレッドサイズ
|lg|64rem(1024px)| PCサイズ
|xl|80rem(1280px)|
|2xl|96rem(1536px)|	


|バリアント|メディアクエリ|
|:--|:--|
|max-sm|@media (width < 40rem) { ... }|
|max-md|@media (width < 48rem) { ... }|
|max-lg|@media (width < 64rem) { ... }|
|max-xl|@media (width < 80rem) { ... }|
|max-2xl|@media (width < 96rem) { ... }|


ブレイクポイントのカスタマイズ
```css
@import "tailwindcss";
@theme {
　--breakpoint-*: initial; #デフォルトのブレイクポイントを削除
  --breakpoint-xs: 30rem;
  --breakpoint-2xl: 100rem;
  --breakpoint-3xl: 120rem;
}

```

 - コンテナクエリ:素の親要素（コンテナ）のサイズに基づいてスタイルを適用する手法
 - メディアクエリ:ビューポート（画面全体）のサイズに応じてスタイルを変更する


```html
<div class="@container">
  <div class="flex flex-col @md:flex-row">
    <!-- ... -->
  </div>
</div>
```

### カスタムCSS

```html
<button class="btn-primary">Save changes</button>
```

```css
@import "tailwindcss";
@layer components {
  .btn-primary {
    border-radius: calc(infinity * 1px);
    background-color: var(--color-violet-500);
    padding-inline: --spacing(5);
    padding-block: --spacing(2);
    font-weight: var(--font-weight-semibold);
    color: var(--color-white);
    box-shadow: var(--shadow-md);
    &:hover {
      @media (hover: hover) {
        background-color: var(--color-violet-700);
      }
    }
  }
}
```

</details>



<details>
<summary>使用するパターン</summary>

 - 横幅
   - **w-{number}**:number に4をかけたピクセル数
   - **w-full**:横幅100％
   - **max-w-{size}**:横幅の最大値
 - ポジション
   - **absolute**:絶対位置
   - ```html
      <div class="absolute bottom-4 right-4">Text</div>
     ```
   - absolute を指定した要素の位置は、top-{size}（上からの位置）や left-{size}（左からの位置）などで指定
 - Flexbox
   - **flex**:横並び
   - **justify-center**:横方向の中央揃え
   - **items-center**:縦方向の中央揃え
   - **gap-{size}**:要素の間隔
 - グリッドレイアウト
   - **grid**:グリッド
   - **grid-cols-{n}**:列数
   - **gap-{size}**:要素の間隔
 - 余白
   - **マージン**:要素の外側の余白
     - **m-{size}**
   - **パディング**:要素の内側の余白
     - **p-{size}**
   - 子要素どうしの間隔
     - **space-y-{amount}**:縦方向
     - **space-x-{amount}**:横方向



</details>