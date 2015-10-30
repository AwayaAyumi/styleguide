# CSS Styleguide

## ファイル構成
CSSには詳細度があります。同じ詳細度であれば、あとで記述したスタイルが適応されますが、詳細度の強さが違う場合は必ずしも適応されるわけではありません。理想的にいえばスタイルシートの序盤は詳細度が弱く抽象的で一般的なスタイルを持ち、終盤になるにしたがって詳細度が強く具体的なスタイルが指定されるべきです。

### FLOCSS
ファイルの構成は[FLOCSS](https://github.com/hiloki/flocss)をベースにするのを推奨します。FLOCSSはFoundation, Layout, Objectの3つのレイヤーからなります。以下はそれぞれの特徴を示したものです。

* Foundation - Normalize.cssやタイポグラフィーなどプロジェクトのベースになるスタイルが該当します。詳細度は極力抑えるべきで、要素セレクタや属性セレクタなどに最低限のスタイルを指定します。
* Layout - ワイヤーフレームに定義されるような大きなコンテナブロックが該当します。このレイヤーでのみIDセレクタを指定することが許容されます。
* Object - OOCSSのコンセプトを元にしたピジュアルパターンが主に該当します。classセレクタのみで構成されます。

ObjectレイヤーはさらにComponent, Project, Utilityの3つのレイヤーに分けられます。

* Component - 多くのプロジェクトで横断的に再利用できるような、小さな単位のモジュールが該当します。OOCSSでの構造を担うため、固有の幅や装飾的なスタイルは極力指定しないようにします。
* Project - プロジェクト固有のビジュアルパターンでいわゆるUI（ユーザーインターフェイス）が該当します。ほとんどのスタイルの追加はこのレイヤーになります。
* Utility - いわゆる汎用クラスのことで、主に単一のスタイルを持っています。ComponentとProjectレイヤーで指定するのが適切でない例外的なスタイルや汎用的に使えるスタイルが定義されています。オプションとして`!important`を許容しています。

FLOCSS以外では[ITCSS](https://speakerdeck.com/dafed/managing-css-projects-with-itcss)と[SMACSS](https://smacss.com/ja)が候補としてあげられます。

Sassのパーシャルを使用するとファイルを一旦分割して管理をして、CSSにコンパイルする時に指定した順序でアウトプットすることができます。

## シンタックスとフォーマッティング
CSSの構文はセレクタとブレース、プロパティと値からなっています。このルールセットに読みやすさを確保したフォーマット（書式）を定義します。

### ルールセットのフォーマット
ルールセットの基本的なフォーマットは以下の通りです。

* 複数のセレクタは別の行に（関連性があれば同じ行に指定してもいい）
* 最後のセレクタとオープニングブレースの間にスペースを1つ
* それぞれの宣言は別々の行に
* ルールセット内に空行は入れない
* プロパティの前にスペースを4つ（2つにしてもいい）
* コロン（`:`）と値の間にスペースを1つ
* クロージングブレースは独立した行に
* 各ルールセットの間に空行を1つ

```css
/* OK */
.foo, .foo--bar,
.baz {
    display: block;
    margin-right: auto;
    margin-left: auto;
}

/* NG */
.foo,
.foo--bar, .baz{
  display: block;margin-right: auto;
  margin-left:auto;}
```

### Sass使用時の追加ルール
Sassを使用している場合は以下のルールを追加します。

* ローカル変数を最初に定義します
* @extendと@mixinをローカル変数の次に指定します
* @contentを使用している@mixinは最後に指定します

```scss
/* OK */
.foo, .foo--bar,
.baz {
    $padding: 1em;
    @extend %base-unit;
    @include clearfix;
    display: block;
    margin-right: auto;
    margin-left: auto;
    padding-right: $padding;
    padding-left: $padding;
}

/* NG */
.foo,
.foo--bar, .baz{
  display: block;margin-right: auto;
  margin-left:auto;
  @extend %base-unit;
  @include clearfix;
  $padding: 1em;}
```

### 単一行ルールセットのフォーマット
原則的には複数行でルールセットを記述しますが、ユーティリティクラスなどの単一のスタイルの場合は1行で記述することを許容します。この場合は複数行よりも可読性に優れていると考えるからです。

単一行のフォーマットは以下の通りです。

* 最後のセレクタとオープニングブレースの間にスペースを1つ
* オープニングブレースとプロパティの間にスペースを1つ
* コロン（`:`）と値の間にスペースを1つ
* セミコロン（`;`）とクロージングブレースの間にスペースを1つ
* 各ルールセットの間には空行を入れない

```css
.u-pos { position: static !important; }
.u-por { position: relative !important; }
.u-poa { position: absolute !important; }
.u-pof { position: fixed !important; }
```

## コメント
コードの機能やルールの意図、目次やTODOを示すために積極的にコメントを使用しましょう。誰もが同じ知識を持っているわけではありませんし、必要なコードを謝って削除してしまう可能性を下げることができます。

### 目次（Table of Contents）
コードを書く前に目次を書きましょう。そしてコードを書いたら目次に追加しましょう。スタイルシートの全体像を把握するのを助けてくれますし、目次から目的のコードを検索することもできるようになります。レイヤーとその中のセクション（モジュール）を簡単な説明も加えて書いていきましょう。

```css
/**
 * VARIABLE
 * Global...............プロジェクト全体を通して使用される変数です。
 *
 * MIXIN
 * Media-query..........メディアクエリ用のmixinです。
 * Clearfix.............`float`を解除する`clearfix`のmixinです。
 *
 * BASE
 * Base.................要素セレクタと属性セレクタのデフォルトスタイルです。
 *
 * LAYOUT
 * Header...............ヘッダーエリアのコンテナブロックのスタイルを指定します。
 * Footer...............フッターエリアのコンテナブロックのスタイルを指定します。
 * Main.................メインコンテンツエリアのコンテナブロックのスタイルを指定します。
 * Sidebar..............サイドバーエリアのコンテナブロックのスタイルを指定します。
 *
 * COMPONENT
 * Wrapper..............コンポーネントをラップするオブジェクトです。`max-width`が指定されています。
 * Grid.................グリッドレイアウトのベーススタイルです。
 *
 * PROJECT
 * Tag-cloud............タグクラウドのコンポーネントです。
 *
 * UTILITY
 * Width................レスポンシブに対応した`width`を指定するためのヘルパークラスです。
 */
```

### セクションタイトル
目次とセクションタイトルは対になります。セクションタイトルに`#`を接頭辞として付けることで素早く検索することができるようになります。

```css
/**
 * #Grid
 */
```

### コードの説明と参照リンク
そのコードが一部のひとにしか正しく理解されないと感じたらコメントを書きましょう。そして、詳しくコメントで説明できない場合は参照となるリンクを書きましょう。

```css
/**
 * ボックスモデルを`border-box`にリセットします。
 * https://css-tricks.com/inheriting-box-sizing-probably-slightly-better-best-practice/
 */
html {
    box-sizing: border-box;
}

*,
*:before,
*:after {
    box-sizing: inherit;
}
```

### マークアップ例
コンポーネントなど、頻繁に使用されるコードにはできるだけ簡単な説明とマークアップの例を書きましょう。

```css
/**
 * グリッドレイアウトオブジェクトです。
 * `width`はデフォルトで100%が指定されています。
 * ガターは左側にだけ指定されています。
 * `width`の変更は`Utility/_width.scss`のクラスを使用します。
 *
 * <div class="c-wrapper">
 *   <div class="c-grid c-grid--gutter-medium">
 *     <div class="c-grid__item u-8/12@md"></div>
 *     <div class="c-grid__item u-4/12@md"></div>
 *   </div>
 * </div>
 */
.c-grid {
    display: block;
    margin: 0;
    padding: 0;
    /* カラム間の余白を除去する */
    font-size: 0;
    list-style-type: none;
}

.c-grid__item {
    display: inline-block;
    width: 100%;
    /* フォントサイズを戻す */
    font-size: 1rem;
    vertical-align: top;
}
```

### Sassのコメント
Sassでは@functionや@mixinなど通常のCSSにはない機能があります。Sassを熟知していないひとのためにもコメントを書いて正しく理解してもらえるようにしましょう。以下は@mixinのコメント例です。

```scss
//
// #Media-query
//

// @desc - ブレイクポイントを挿入します。
// @type Mixin
// @param {String} $breakpoint - `$breakpoints`のkeyを渡します。
// @requires {Map} $breakpoints - ブレイクポイントを管理するためのmapです。
//
// @example scss - Usage
// .foo {
//     color: red;
//     @include media-query(sm) {
//         color: blue;
//     }
// }
//
// @example css - CSS output
// .foo {
//   color: red;
// }
// @media screen and (min-width: 400px) {
//   .foo {
//     color: blue;
//   }
// }
//
@mixin media-query($breakpoint) {
    @media #{map-get($breakpoints, $breakpoint)} {
        @content;
    }
}
```



## 命名規則
命名規則はBEMをベースにした[MindBEMding](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)を使用します。理由は役割や関係性が明確にできるからです。これはコードを書く時でも読む時でも同様です。

BEMについては以下のリンクを参考にしてください。

* <a href="https://github.com/juno/bem-methodology-ja/blob/master/definitions.md" target="_blank">bem-methodology-ja/definitions.md at master · juno/bem-methodology-ja</a>
* <a href="http://blog.ruedap.com/2013/10/29/block-element-modifier" target="_blank">BEMという命名規則とSass 3.3の新しい記法 - アインシュタインの電話番号</a>
* <a href="https://app.codegrid.net/entry/bem-basic-1" target="_blank">BEMによるフロントエンドの設計 - 基本概念とルール | CodeGrid</a>

### MindBEMding
BEMはBlock, Element, Modifierの3つから構成され、以下のようなclass名になります。

```css
.block-name {}
.block-name__element-name {}
.block-name--modifierkey-value {}
.block-name__element-name--modifierkey-value {}
```

以下のようなルールがあります。

* 小文字の英単語とハイフン2つ（`--`）とアンダースコア2つ（`__`）で構成されます
* BlockとElementとModifierの単語はハイフン1つ（`-`）でつなぎます
* Blockはできるだけ2つ以上の単語を合わせてユニークな名前にします
* ElementとModifierは1つの単語だけを使ってもかまいません
* ElementとModifierはBlockの名前を受け継ぎます
* BlockとElementはアンダースコア2つ（`__`）でつなぎます
* BlockとModiferもしくはElementとModiferはハイフン2つ（`--`）でつなぎます

よって以下のように短く書くこともできます。

```css
.block-name {}
.block-name__element {}
.block-name--modifier {}
.block-name__element--modifier {}
```

### BEMの注意点
ElementのElementを表すためにclass名を連結させてはいけません。`sub`や`child`といった名前によって関係性を表しましょう。

```css
/* OK */
.block__element {}
.block__sub-element {}

/* NG */
.block__element {}
.block__element__element {}
```

Sassを使用している場合、ネストによってBEMの記述が簡単になりますが、使用を禁止します。理由はBlockに関連するルールセットが際限なくつながり、長くなることで可読性やメンテナンス性を損なう可能性が高いからです。

```scss
// NG
.block {
    &__element {
        ...
    }
    &__element {
        ...
    }
    &__element {
        ...
    }
    &--modifier {
        ...
    }
    &__element {
        ...
    }
    &__element {
        &--modifier {
            ...
        }
    }
}
```

## セレクタ
### インライン記述を使用しない
HTMLの`style`属性に直接スタイルを記述するインライン記述は使用しません。理由はスタイルシートで一括管理できないこと、詳細度が高いこと、HTMLのファイルサイズを無闇に増やしてしまうこと、単一のスタイルごとしか指定できない（レスポンシブに対応できない）からです。

```html
<div style="color:red; background-color: black;"></div>
```

JavaScriptによるインラインでのスタイル追加もできるだけ避けて、classを追加するようにします。

### HTML（DOM）構造に依存させない
マークアップは変更される場合があります。例えば`article`を`section`にしたり`h2`を`h3`に変更するかもしれません。特定のHTML要素に依存したセレクタを指定している場合はその都度変更しなくてはいけません。

```css
article h2 {}
```

`class`を使ったセレクタを定義していればHTML構造が変更されたとしても、CSSを変更する必要がなくなり、可搬性が高まります。

```css
/* `article`要素の見出しに指定するクラス */
.article__title {}

/* 汎用的な見出しのクラスを用意 */
.u-h2 {}
```

### 不要な要素セレクタを連結させない
classやIDセレクタで指定する時に要素セレクタを連結させるのは詳細度を高めるだけで意味がありません。

```css
/* OK */
.foo {}

/* NG */
h2.foo {}
```

もし、特定の要素に指定してほしいのであれば、コメントによってそれを示しましょう。

```css
/* OK */
/* h2要素に指定してください。 */
.foo {}

/* h2 */.foo {}
```

### セレクタを短くする
セレクタは必ずしもHTML構造に沿う必要はありません。場合によってはセレクタを短くすることで詳細度を低く抑えることができます。

```css
/* OK */
ul a {}

/* NG */
ul li a {}
```

### セマンティックでないセレクタの指定を制限する
特定の意味を持たないセレクタ（`div`, `span`）は使用される範囲も広いため、セレクタに対する影響範囲も広くなります。できるだけ範囲を狭くして、少なくとも単体でのセレクタは禁止とします。

```css
/* NG */
div {}
span {}
.foo div {}
.foo span {}
```

また、`ul`や`li`といったセマンティックな要素セレクタであれば指定することは許容されますが、ネストされたセレクタに影響することを防ぐためにclassを親としたセレクタに直下セレクタや隣接セレクタを併用しましょう。

```css
/* OK */
.foo > li {}
```

### IDセレクタを多用しない
CSSはカスケーディングをコントロールしなければ、すぐに破綻してしまいます。IDセレクタは詳細度が圧倒的に高いことや、ページ内に1度しか使用できないため再利用性にも欠けます。classではなくIDで指定する理由が説明できるのであれば使用しましょう。

CSSのIDセレクタとHTMLのid属性とは区別して考えます。

## プロパティ
### !importantを多用している
`!important`は基本的に使用しませんが、意図を持って能動的に使用する場合は許容されます。例えばグリッドレイアウトなどに使用される`width`のヘルパークラスです。

```css
/* OK */
.u-col1of12 { width: 8.33333% !important; }
.u-col2of12 { width: 16.66667% !important; }
.u-col3of12 { width: 25% !important; }
```

その場合もスタイルシートの終盤に記述をして、影響範囲を抑えましょう。

### スタイルを取り消している
スタイルの取り消しとは、あるスタイルを`0`や`none`などで値をリセットすることです。スタイルの取り消しがあった場合は多くのスタイルを持ちすぎている、スタイルを早く指定していまっていることが考えられます（リセットCSSは除く）。

```css
.foo {
    border: 1px solid #ddd;
}

/* ある要素内ではボーダーが邪魔になるためリセット */
.bar .foo {
    border: none;
}
```

Modifierで定義しておくと必要な時にだけ指定することができます。

```css
.foo {}
.foo--bordered {
    border: 1px solid #ddd;
}
```

### 要素セレクタに装飾的なスタイルを指定しない
`h1`は`p`といったセマンティックな要素セレクタには装飾的なスタイルを指定してはいけません。必ずあとでスタイルを取り消すことになります。

```css
/* NG */
h2 {
    padding-bottom: 0.5em;
    border-bottom: 1px solid #888;
}
```

要素セレクタにはブラウザのデフォルトスタイルシートやリセットCSSで指定されるような最低限のスタイルにとどめておきましょう。

```css
/* OK */
h1 {
    font-size: 2rem;
    line-height: 1.2;
    margin-top: 0;
    margin-bottom: 24px;
}
```


### 高さを指定しない
閲覧されるデバイスによって同じコンテンツであっても横幅が変わるため高さも可変します。高さを指定しているとコンテンツが隠れてしまったり、余分な余白ができてしまいます。

```css
.foo {
    height: 1000px;
}
```

高さは指定せず、コンテンツによって決まるものと考えましょう。ロゴなどの特定のサイズを持ったものはこれに当たりません。

### 固定の幅を持たせない
コンポーネントは基本的に固有の幅を持つことを禁止します。`width`を指定していると、ある場所では幅が足りず、ある場所でははみ出てしまうなどの弊害が出ます。幅を指定したい時は、コンテナコンポーネントに`max-width`を指定するか、`width`のヘルパークラスを使用するようにしましょう。

### カラーコードは短縮させる
`color`プロパティなどで色を指定する時は可読性をあげるために、可能な場合は短縮しましょう。

```css
/* OK */
.foo  { color: #ddd; }

/* NG */
.foo  { color: #dddddd; }
```

### 文字列には引用符（ダブルクォート）をつける
`content`プロパティやURLの指定、属性セレクタの指定をする時にはダブルクウォートを使用しましょう。

```css
/* OK */
.foo {
    content: "this is content";
    background: url("logo.png");
}
input[type="submit"] {}

/* NG */
.foo {
    content: 'this is content';
    background: url(logo.png);
}
input[type=submit] {}
input[type='submit'] {}
```

### 0に単位をつけない
値が0の場合はpxや%といった単位は必要がないため使用しません。

```css
/* OK */
.foo { margin: 0; }

/* NG */
.foo { margin: 0px; }
```

### 小数点のあたまの0を省略しない
`0.5em`などの小数点の前の0は省略しません。ファイルサイズの削減は考えずに、明示的に指定しましょう。

```css
/* OK */
.foo { font-size: 0.5em; }

/* NG */
.foo { font-size: .5em; }
```

### すべて小文字で記述する
プロパティとプロパティ値は大文字でも小文字でも同様に扱われますが、小文字に統一しましょう。

```css
/* OK */
.foo { color: #fff;

/* NG */
.foo  { COLOR: #FFF;
```

### ショートハンドはなるべく避ける
`font-size`や`margin`などショートハンドが用意されているプロパティがありますが、ショートハンドに指定しなかったプロパティに初期値が渡されてしまうことから意図せずプロパティがリセットされてしまう可能性があります。明示的に何が目的なのかを示すという意味でも使用をなるべく避けましょう。

```css
/* OK */
.foo { 
    /* 中央配置にする。 */
    margin-right: auto;
    margin-left: auto;
}

/* NG */
/* 上下を0に指定する必要がない。 */
.foo { margin: 0 auto; }
```

### ベンダープレフィックスは手動で書かない
`-webkit-`や`-moz-`などのベンダープレフィックスは手動で書かず、[Autoprefixer](https://github.com/postcss/autoprefixer)を使用しましょう。Sassのmixinを使用する方法もありますが、mixinを管理する手間が発生します。Gulpなどの環境が使えない場合は[Prepros](https://prepros.io/)を推奨します。


## Sass
BootstrapやFoundaitonなどのCSSフレームワークでも使われているようにSassを利用してCSSを書くことがデファクトスタンダードになっています。Sass特有の機能についていくつかのルールを設けます。

### 機能ごとにコードを分割する
SassにはCSSとしてアウトプットさせないファイルを定義するパーシャルという機能があります。コードをファイルごとに分割してモジュール化して管理しましょう。scssファイルをアンダースコア(`_`)から始めることでパーシャルファイルとして認識されます。

```scss
// パーシャルファイルをインポートする
@import "_grid";

// インポートしない場合
// @import "_grid";
```

スタイルシートのアウトプットする場所を指定することでカスケーディングを管理しやすくなったり、使うファイルだけインポートすることができます。

### 変数は繰り返しや変更する場合にだけ使用する
Sassの変数は特定の値を一括で管理できる便利な機能ですが、使いすぎると可読性を損なったり変数名がバッティングする危険性が増します。同じ値を繰り返し指定する場合や四則演算などで値を変更・再計算する場合にだけ定義しましょう。

```scss
// OK
$unit-base: 1em !dafault;
.foo {
    margin-left: (-$unit-base);
}
.foo__bar {
    padding-left: $unit-base;
}

// NG
$foo-unit: 1em !dafault;;
.foo {
    padding: $foo-unit;
}
```

### モジュール内でだけ使用する変数は個別に定義する
モジュール内で、ある変数や値が繰り返し使用されている場合はモジュール専用の変数を定義しましょう。プロジェクト全体で使用される変数に変更があった場合でも各モジュールで対応できるようにしておきます。

```scss
$unit-base: 1em !default;

/**
 * #Media
 */
$media-gutter: $unit-base !default;
```

### 変数には!defaultフラグを必ず指定する
変数を定義する際には`!default`フラグを必ず指定するようにしましょう。その変数が定義されているファイルより先に同じ変数を定義し直すことで変数の値を確実に上書きすることができます。これによって変数をまとめて管理することが可能になります（必ずしもまとめて変更する必要はなく、あくまでオプションです）。

```scss
// OK
$unit-base: 1em !default;

// NG
$unit-base: 1em;
```

### ネストは擬似要素やメディアクエリだけに制限する
兄弟セレクタやBEMのElementやModifierを指定するためにSassのネストは便利です。ですが、ネストを制限なく使用すると際限なく縦に長くなってしまい、可読性を損ないます。ネストは擬似要素、擬似クラス、メディアクエリの使用にとどめます。

```scss
// OK
.foo {
    &:hover,
    &:focus {
        ...
    }
    @media (min-width: 768px) {
        ...
    }
}

// NG
.foo {
    &__bar {
        ...
        ...
        ...
    }
    &__baz {
        ...
        ...
        ...
    }
}
```

ネストするセレクタが明確に関係している場合はネストの使用を許容します。別々のルールセットにして記述するよりも理解しやすいと考えられるためです。

```scss
// OK
.foo--gutter-medium {
    margin-left: - $foo-gutter;
    > .foo__item {
        padding-left: $foo-gutter;
    }
}
```

### 数値の計算には括弧（）を常に使う
Sassでは四則演算を使うことができます。複数の数値を計算させる時に想定外の計算結果になってしまうことを防ぐために括弧`（）`で囲いましょう。

```scss
// OK
.foo {
    width: (100% / 3);
}

// NG
.foo {
    width: 100% / 3;
}
```

### extendはできるだけ使用しない
extendは継承を可能にするため、Sassファイル内を簡潔にできます。しかし、継承元のセレクタを記述した場所にまとめて出力されるため、カスケーディングを複雑にしてしまう可能性があります。

もし使用するのであれば、同じモジュール内で完結させるか、継承元のセレクタを1つのモジュールにまとめてカスケーディングを管理しやすいようにしましょう。