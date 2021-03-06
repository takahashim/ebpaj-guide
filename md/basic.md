# 制作記述の基本項目

特に指示がないかぎり、以下のルールに則りデータ制作を行います。

なお、ファイル・フォルダ名やソースの整形ルールについては、あくまでも一定の基準でデータを制作する上での指針にすぎません。制作者ごとの癖が強く出てしまうと、後で他人が修正等をする際に困難が生じるため、なるべく統一のルールで運用することを推奨します。

特定のオーサリングツールの自動整形を用いるなど、版元から他の指示がある場合はそちらに従ってください。


## ■最新版の epubcheck でエラーの出ないデータを制作すること

IDPF/epubcheck . GitHub
https://github.com/IDPF/epubcheck/

iTunes のように、読み込んだ EPUB ファイル内に独自ファイルを埋め込む（そのデータは再度 epubcheck をかけるとエラーとなる）ような、ファイルの改変をしてしまう RS にご注意ください。監修時にはコピーを用いるなどして、一度他の RS に読み込ませたものを納品してしまわないことを推奨します。


## ■基本的なフォルダ構成とファイル名

```
root フォルダ
├ mimetype
├ META-INF フォルダ
│ └ container.xml
└ item フォルダ
   ├ standard.opf
   ├ navigation-documents.xhtml
   ├ image フォルダ
   ├ style フォルダ
   └ xhtml フォルダ
```

* root フォルダ名は版元の指示に従って設定
* ファイル・フォルダ名は原則小文字（META-INF および管理コードなど指示のあるものは除く）
* 素材格納フォルダの名前は、パッケージ文書での `<item>` 要素にあわせ「item」とする（仕様上は任意）
* 素材はすべて item フォルダ内の指定のフォルダに入れ、他のフォルダやサブフォルダを作らない

```
画像ファイル        ：「image」フォルダ
CSS ファイル        ：「style」フォルダ
xhtml ファイル      ：「xhtml」フォルダ
```

* 以下のファイルは改変しない（添付のサンプル同梱のものをそのまま利用）
    * root フォルダ直下にある「mimetype」
    * 「META-INF」フォルダ内の「container.xml」
* 最低限、どの作品にも共通するページと本文部分とで、XHTML 文書のファイル名を分離しておくことを推奨

※注意書き等の差し替え時におけるファイル判別の手間軽減、画像や解説などの追加・削除によるリンク指定の URL 変更回避といった管理面での事情に配慮

## ■ファイル仕様

* 底本で見開きの図版や写真を、左右ページつなぎあわせた１枚の画像として作成し、ページフィットさせて挿入
* 底本での改ページごとにファイルを分割して、XHTML 文書を作成
    * 改ページがない作品は 240KB 程度（〜 256KB 未満）で適度に分割
      （近くに見出しがある場合はその直前で、ない場合は空行の位置で分割）
* ファイルのタイトルはすべて作品名
    * XHTML 文書の「`<title>`〜`</title>`」部分には、そのファイルに含まれる内容のタイトルを挿入します。
      複数の章を含む場合など、内容が一様でない場合は、各自ルールを決めて指示を出してください。
    * 特に指示がない場合は、作品名を挿入します。
      後述するパッケージ文書（OPF ファイル）の作品名情報を利用してください。
      メインタイトルと、サブタイトルやシリーズ名との間は、全角アキでつなぎます。
    * ここに記されたタイトルがどのように利用されるかは、RS 側の機能や考え方次第です。
      Web ブラウザの場合と同じように、画面のどこかに表示される可能性があります。
      読者の目に触れても大丈夫なように、入力された情報に間違いがないか注意してください。
* epub:type は、カバーとナビゲーション文書にのみ挿入
    * EPUB では、ページの役割を示すために、epub:type という属性を指定することができます。
    * ただ、現状でこれらを利用する RS はなく、また epub:type を利用した CSSの指定が保証されているわけでもありません。そのため現時点では、将来システムから利用される可能性が高そうな以下の項目だけ、目印代わりに指定しておくこととします。
    * 「epub:type」は、その値により適用できる HTML 要素が異なります。下記以外の値を利用するときは、すべてが body や section に指定できるわけではないことに注意してください。
    * なお、これらはテンプレートに記した定型ページ用の class や、ナビゲーション文書の id の前に記述することとします。
        * カバー画像のページ  `<body epub:type="cover" class="p-cover">`
        * ナビゲーション文書  `<nav epub:type="toc" id="toc">`
    * ナビゲーション文書に epub:type="toc" が指定された `<nav>` が存在しないと epubcheck でエラーが出ますので、必ず指定するようにしてください。


## ■簡易コーディングルール

* 文字コードは UTF-8N（BOM 無し）を推奨
* 改行コードを同一ファイル内で混在させない
* 本ガイドで触れていない HTML 要素や CSS プロパティの利用は非推奨
* 版元指定外のコメントは挿入しない
* 論理方向の表記を class 名に採用

    * class や CSS では縦組みと横組みで上下左右が変わってしまうため、行内では主に以下の表記を用います。

      ```
      行頭     ：start （縦：top     横：left）
      行末     ：end   （縦：bottom  横：right）
      行の前方 ：before（縦：right   横：top）
      行の後方 ：after （縦：left    横：bottom）
      ```

    * ただし、ページ全体の設定では、画像のみのページがほぼ常に横組みとなってしまうことを踏まえ、top / right / bottom / left を用いても構いません。
    * なお今回の CSS 中では、行頭行末方向の中央を center と考え、class 名を設定しています。
    * また便宜的に、ページ進行方向（行前後方向）の中央を middle としています。

* 本文内での要素中の属性記載順は「epub:type → class → id → src / href → alt」
* 煩雑になるのを避けるため、`<p>` には極力 class を指定しない
* 本文用 XHTML 文書中の HTML 要素直後の改行

`<div>` などのブロックレベル的な要素では、必ず開始タグと閉じタグの直前直後にそれぞれ改行コードを入れるようにします。
ただし、`<p>` と見出しの `<h1>`〜`<h6>` については、開始タグの直後および閉じタグの直前には改行コードを入れないようにします。

例）

```
×  <h1>
    テキスト
    </h1>
    <div><p>テキスト</p></div>

○  <h1>テキスト</h1>
    <div>
    <p>テキスト</p>
    </div>
```

インライン的な要素（`<span>` など）では原則、改行しないようにします。

`<a>` の場合は、`<a>` がブロックレベル的要素（`<p>` も含む）か `<img>` を囲むのでないかぎり、改行コードは入れないようにします。

いずれも、もし要素の入れ子が増えすぎて対応関係がわからなくなるようなら、無理に既存の class を用いるのではなく、専用の class を用意することをご検討ください。


なお「特定キャラの台詞の書体や色を一括で変更したい」「手紙文として字下げした箇所を、すべて囲み罫に変更したい」など、後からスタイルを修正・変更する可能性がある部分では、同一要素内で複数 class を指定するより、専用の class を定義したほうが良い場合もあります。必要があれば、CSS ファイルのカスタマイズ領域を利用して、新規 class を作成してください。スタイルシートのカスタマイズについては、後述の「デフォルト CSS ファイルについて」の項を参照のこと。
