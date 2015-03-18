# Ａ．リフロー型

## 設定系の必須ファイル


#### ■テンプレート中の色分けについて

灰色：すべての作品で共通の部分（原則、変更しない）
青色：すべての作品の共通部分中で、作品ごとに変更する部分
赤色：そのテンプレートを利用する作品に特有の、注意すべき部分（原則、変更しない）
黒色：定型ではない部分（作品、版元により異なる）


#### ■mimetype

[filename: mimetype]

```
---------------------------------[sample code]---------------------------------
application/epub+zip
-------------------------------------------------------------------------------
```


## ■META-INF 内の container.xml

[filename: container.xml]

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0"?>
<container
 version="1.0"
 xmlns="urn:oasis:names:tc:opendocument:xmlns:container"
>
<rootfiles>
<rootfile
 full-path="item/standard.opf"
 media-type="application/oebps-package+xml"
/>
</rootfiles>
</container>
-------------------------------------------------------------------------------
```



#### ■ナビゲーション文書

[filename: navigation-documents.xhtml]

[備考]

* リンク項目やリストの階層構造は作品内容により変更
* 版元から特に指示がないかぎり、カバーページ、目次ページ、奥付ページへのリンクのみとする
* ナビゲーション文書中にリンク以外の項目を含められるかどうかは、本仕様ではサポートしない
* ナビゲーション文書の表示のされ方については、RS に一任するものとする
* ナビゲーション文書を本文内の目次ページとしても表示させる場合は、後述する本文ページなどの例を参考に、スタイルシートの指定等を挿入すること

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<html
 xmlns="http://www.w3.org/1999/xhtml"
 xmlns:epub="http://www.idpf.org/2007/ops"
 xml:lang="ja"
>
<head>
<meta charset="UTF-8"/>
<title>Navigation</title>
</head>
<body>
<nav epub:type="toc" id="toc">
<h1>Navigation</h1>
<ol>
<li><a href="xhtml/p-cover.xhtml">表紙</a></li>
<li><a href="xhtml/p-toc.xhtml">目次</a></li>
<li><a href="xhtml/p-colophon.xhtml">奥付</a></li>
</ol>
</nav>
</body>
</html>
-------------------------------------------------------------------------------
```


#### ■OPF ファイル

[filename: standard.opf]

[備考]

* RS に <dc:title> の情報を表示する機能がある場合、必ず RS 内のどこかで、記載内容のすべてが画面に表示されるものと想定する
* RS に <dc:creator> の情報を表示する機能があり、複数の <dc:creator> がある場合、必ず RS 内のどこかで、記載内容のすべてが画面に表示されるものと想定する
  （複数著作者名の連結時の記号や、役割表記の表示等は、RS に一任するものとする）
* 複数の著作者名を一人ずつ分けるか、ひとつの <dc:creator> に全員記載するかは版元の指示に従う
  分けて入れる場合、版元は各著作者の「role」の値と、著作者の表示順序を必ず指示すること
* 「file-as」で指定した整列用カナは、読者に対しては表示されないことを想定する
* ファイル id（「unique-identifier」）に用いるコード体系は定めない
  （版元の指示に従うこと。特に指示がない場合は uuid を挿入する）
* 更新日は特に指示がない場合、後のファイル管理の便宜を考えて、納品予定日とする
* 更新日は読者に対して表示されないことが望ましい
* カバー画像のファイル名は、特に指示がない場合、RS 側のサムネイル表示の速度向上に配慮する目的で、すべて同じ名前（cover.jpg）とする
* 横組み作品の場合、<spine> の「page-progression-direction」は「rtl」から「ltr」に変更する

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<package
 xmlns="http://www.idpf.org/2007/opf"
 version="3.0"
 xml:lang="ja"
 unique-identifier="unique-id"
 prefix="ebpaj: http://www.ebpaj.jp/"
>

<metadata xmlns:dc="http://purl.org/dc/elements/1.1/">

<!-- 作品名 -->
<dc:title id="title">作品名１</dc:title>
<meta refines="#title" property="file-as">セイレツヨウサクヒンメイカナ01</meta>

<!-- 著者名 -->
<dc:creator id="creator01">著作者名１</dc:creator>
<meta refines="#creator01" property="role" scheme="marc:relators">aut</meta>
<meta refines="#creator01" property="file-as">セイレツヨウチョサクシャメイカナ01</meta>
<meta refines="#creator01" property="display-seq">1</meta>

<dc:creator id="creator02">著作者名２</dc:creator>
<meta refines="#creator02" property="role" scheme="marc:relators">aut</meta>
<meta refines="#creator02" property="file-as">セイレツヨウチョサクシャメイカナ02</meta>
<meta refines="#creator02" property="display-seq">2</meta>

<!-- 出版社名 -->
<dc:publisher id="publisher">出版社名</dc:publisher>
<meta refines="#publisher" property="file-as">セイレツヨウシュッパンシャメイカナ</meta>

<!-- 言語 -->
<dc:language>ja</dc:language>

<!-- ファイルid -->
<dc:identifier id="unique-id">urn:uuid:d7a8d311-7cd0-40df-9443-65847561decf</dc:identifier>

<!-- 更新日 -->
<meta property="dcterms:modified">2014-01-01T00:00:00Z</meta>

<!-- etc. -->
<meta property="ebpaj:guide-version">1.1.3</meta>

</metadata>

<manifest>

<!-- navigation -->
<item media-type="application/xhtml+xml" id="toc" href="navigation-documents.xhtml" properties="nav"/>

<!-- style -->
<item media-type="text/css" id="book-style"     href="style/book-style.css"/>
<item media-type="text/css" id="style-reset"    href="style/style-reset.css"/>
<item media-type="text/css" id="style-standard" href="style/style-standard.css"/>
<item media-type="text/css" id="style-advance"  href="style/style-advance.css"/>
<item media-type="text/css" id="style-check"    href="style/style-check.css"/>

<!-- image -->
<item media-type="image/jpeg" id="cover"      href="image/cover.jpg" properties="cover-image"/>
<item media-type="image/png"  id="logo-bunko" href="image/logo-bunko.png"/>
<item media-type="image/jpeg" id="kuchie-001" href="image/kuchie-001.jpg"/>
<item media-type="image/jpeg" id="img-001"    href="image/img-001.jpg"/>
<item media-type="image/jpeg" id="ad-001"     href="image/ad-001.jpg"/>

<!-- xhtml -->
<item media-type="application/xhtml+xml" id="p-cover"       href="xhtml/p-cover.xhtml"/>
<item media-type="application/xhtml+xml" id="p-fmatter-001" href="xhtml/p-fmatter-001.xhtml"/>
<item media-type="application/xhtml+xml" id="p-titlepage"   href="xhtml/p-titlepage.xhtml"/>
<item media-type="application/xhtml+xml" id="p-caution"     href="xhtml/p-caution.xhtml"/>
<item media-type="application/xhtml+xml" id="p-toc"         href="xhtml/p-toc.xhtml"/>
<item media-type="application/xhtml+xml" id="p-001"         href="xhtml/p-001.xhtml"/>
<item media-type="application/xhtml+xml" id="p-002"         href="xhtml/p-002.xhtml"/>
<item media-type="application/xhtml+xml" id="p-003"         href="xhtml/p-003.xhtml"/>
<item media-type="application/xhtml+xml" id="p-004"         href="xhtml/p-004.xhtml"/>
<item media-type="application/xhtml+xml" id="p-005"         href="xhtml/p-005.xhtml"/>
<item media-type="application/xhtml+xml" id="p-colophon"    href="xhtml/p-colophon.xhtml"/>
<item media-type="application/xhtml+xml" id="p-ad-001"      href="xhtml/p-ad-001.xhtml"/>

</manifest>

<spine page-progression-direction="rtl">

<itemref linear="yes" idref="p-cover"       properties="page-spread-left"/>
<itemref linear="yes" idref="p-fmatter-001" properties="page-spread-left"/>
<itemref linear="yes" idref="p-titlepage"   properties="page-spread-left"/>
<itemref linear="yes" idref="p-caution"     properties="page-spread-left"/>
<itemref linear="yes" idref="p-toc"         properties="page-spread-left"/>
<itemref linear="yes" idref="p-001"         properties="page-spread-left"/>
<itemref linear="yes" idref="p-002"         properties="page-spread-left"/>
<itemref linear="yes" idref="p-003"         properties="page-spread-left"/>
<itemref linear="yes" idref="p-004"/>
<itemref linear="yes" idref="p-005"/>
<itemref linear="yes" idref="p-colophon"    properties="page-spread-left"/>
<itemref linear="yes" idref="p-ad-001"/>

</spine>

</package>
-------------------------------------------------------------------------------
```

## XHTML文書ファイル


#### ■テンプレート中の色分けについて

灰色：すべてのページまたは作品で共通の部分（原則、変更しない）
青色：すべてのページの共通部分中で、作品ごと、ページごとに変更する部分
赤色：そのテンプレートを利用するページに特有の、注意すべき部分（原則、変更しない）
黒色：定型ではない部分（ページ内容や作品、版元により異なる）
緑色の□：全角空白を示す


#### ■組み方向について

各ページの <html> には、次に記す組み方向が指定されている

```
class="hltr"：横組み 組み方向 h（Horizontal） 進行方向 ltr（Left To Right）
class="vrtl"：縦組み 組み方向 v（Vertical）  進行方向 rtl（Right To Left）
```

※CSS3 にある「縦組みの ltr」は、現時点ではサポートを想定しない

※画像のみのページでは、画像の左右中央を実現するため、横組みを用いている



#### ■カバーページ

[filename: p-cover.xhtml]

[備考]

* デフォルトは天ツキ左右中央揃え
* 画像表示の指定以外は記載しないこと
* カバー画像が存在するとはかぎらない（権利処理上の事情等による）ため、
  RS はカバー画像が存在しないときは代替画像を用意するなどして、配信可能とすることが望ましい

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<html
 xmlns="http://www.w3.org/1999/xhtml"
 xmlns:epub="http://www.idpf.org/2007/ops"
 xml:lang="ja"
 class="hltr"
>
<head>
<meta charset="UTF-8"/>
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/book-style.css"/>
</head>
<body epub:type="cover" class="p-cover">
<div class="main">

<p><img class="fit" src="../image/cover.jpg" alt=""/></p>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```


#### ■前付（この例では口絵）

[filename: p-fmatter-***.xhtml]  ※例では「p-fmatter-001.xhtml」

[備考]

* カバーページから本扉までの間にあるページを、便宜上すべて前付とする
* 画像ページとはかぎらない

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<html
 xmlns="http://www.w3.org/1999/xhtml"
 xmlns:epub="http://www.idpf.org/2007/ops"
 xml:lang="ja"
 class="hltr"
>
<head>
<meta charset="UTF-8"/>
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/book-style.css"/>
</head>
<body class="p-image">
<div class="main">

<p><img class="fit" src="../image/kuchie-001.jpg" alt=""/></p>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```


#### ■本扉ページ

[filename: p-titlepage.xhtml]

[備考]

* 内容や組み方向は、各版元および作品により異なる
  （下記内容は class 名等を含め、すべてあくまでも参考用）

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<html
 xmlns="http://www.w3.org/1999/xhtml"
 xmlns:epub="http://www.idpf.org/2007/ops"
 xml:lang="ja"
 class="hltr"
>
<head>
<meta charset="UTF-8"/>
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/book-style.css"/>
</head>
<body class="p-titlepage">
<div class="main">

<div class="book-title">
<div class="book-title-before">
<p>サブタイトル・前</p>
</div>
<div class="book-title-main">
<p>メインタイトル</p>
</div>
<div class="book-title-after">
<p>サブタイトル・後</p>
</div>
</div>

<div class="author">
<p>著者名１</p>
<p>著者名２</p>
</div>

<div class="label">
<p class="label-logo"><img src="../image/logo-bunko.png" alt=""/></p>
<p class="label-name">●●文庫</p>
</div>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```


#### ■電子版用の注意書きページ

[filename: p-caution.xhtml]

[備考]

* 内容や組み方向、挿入位置などは、各版元および作品により異なる
* 主に「全作品に必ず挿入する、文字サイズなどレイアウト含め定型の注意書き」に用いることを想定しているため、各作品ごとに異なる注意書きに関しては、特に指示がなければ通常の本文ページを利用する

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<html
 xmlns="http://www.w3.org/1999/xhtml"
 xmlns:epub="http://www.idpf.org/2007/ops"
 xml:lang="ja"
 class="vrtl"
>
<head>
<meta charset="UTF-8"/>
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/book-style.css"/>
</head>
<body class="p-caution">
<div class="main">

<p>無断転載を禁ず云々。</p>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```


#### ■目次ページ

[filename: p-toc.xhtml]

[備考]

* ナビゲーション文書を作品本体の目次としても表示させる場合には不要
* 内容や組み方向は、作品により異なる
* 特に指示がないかぎり、ジャンプ先には必ず id を入れておくこととする
* 特に指示がないかぎり、ジャンプ先から目次にもどるためのリンクは設定しない
* 見出し要素等に用いる class は固定ではない
  （<h1 class="gfont font-1em30"> のように、直接サイズや書体等の class を指定しても良い）

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<html
 xmlns="http://www.w3.org/1999/xhtml"
 xmlns:epub="http://www.idpf.org/2007/ops"
 xml:lang="ja"
 class="vrtl"
>
<head>
<meta charset="UTF-8"/>
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/book-style.css"/>
</head>
<body class="p-toc">
<div class="main">

<h1 class="mokuji-midashi">□目次見出し</h1>
<p><br/></p>
<p><br/></p>
<p><a href="p-001.xhtml#toc-001">目次項目１</a></p>
<p>□<a href="p-002.xhtml#toc-002"><span class="font-0em80">目次項目２</span></a></p>
<p>□<a href="p-002.xhtml#toc-003"><span class="font-0em80">目次項目３</span></a></p>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```


#### ■扉ページ

[filename: p-***.xhtml]  ※例では「p-001.xhtml」

[備考]

* 内容や組み方向は、作品により異なる
* 目次からのリンクを受ける id の指定位置について（本文ページも同様に）
  特に指示がないかぎり、目次項目と同じ文字列の見出し的な要素があれば、そこに id を指定する
  見出し的な要素が無い場合（画像のみなど）、もしくは見出しはあるがその直前にも表示すべき内容がある場合は、ジャンプ先の内容を含む直近の <p> や <div> など、CSS で { display: block; } 指定された要素（ブロックレベル要素）に id を添える
* ファイル名に「p-001」のような３桁連番を使う場合、３桁を超えたら適時ファイル名を調整すること
* 見出し要素等に用いる class は固定ではない
  （<p class="gfont font-1em50"> のように、直接サイズや書体等の class を指定しても良い）

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<html
 xmlns="http://www.w3.org/1999/xhtml"
 xmlns:epub="http://www.idpf.org/2007/ops"
 xml:lang="ja"
 class="vrtl"
>
<head>
<meta charset="UTF-8"/>
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/book-style.css"/>
</head>
<body class="p-tobira">
<div class="main">

<p class="tobira-midashi" id="toc-001">第一章□あいうえおかきくけこさしすせそたちつてと</p>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```


#### ■本文ページ（縦組み）

[filename: p-***.xhtml]  ※例では「p-002.xhtml」

[備考]

* 内容や組み方向は、作品により異なる
* 見出し要素等に用いる class は固定ではない
  （<h1 class="gfont font-1em30"> のように、直接サイズや書体等の class を指定しても良い）

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<html
 xmlns="http://www.w3.org/1999/xhtml"
 xmlns:epub="http://www.idpf.org/2007/ops"
 xml:lang="ja"
 class="vrtl"
>
<head>
<meta charset="UTF-8"/>
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/book-style.css"/>
</head>
<body class="p-text">
<div class="main">

<h1 class="oo-midashi" id="toc-002">第一節□あいうえおかきくけこさしすせそたちつてと</h1>
<p><br/></p>
<p><br/></p>
<p>□この文章はサンプルです。</p>
<p>「この文章はサンプルです」</p>
<p><br/></p>
<h2 class="ko-midashi" id="toc-003">□□□□第一項</h2>
<p><br/></p>
<p>□この文章はサンプルです。</p>
<p>□この文章はサンプルです。</p>
<p><img class="fit" src="../image/img-001.jpg" alt=""/></p>
<p>□この文章はサンプルです。</p>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```


#### ■本文ページ（横組み）

[filename: p-***.xhtml]  ※例では「p-003.xhtml」

[備考]

* <html> の class が変わるだけで、基本は縦組みと同じ
* 内容や組み方向は、作品により異なる

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<html
 xmlns="http://www.w3.org/1999/xhtml"
 xmlns:epub="http://www.idpf.org/2007/ops"
 xml:lang="ja"
 class="hltr"
>
<head>
<meta charset="UTF-8"/>
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/book-style.css"/>
</head>
<body class="p-text">
<div class="main">

<p><br/></p>
<p><br/></p>
<p>□この文章はサンプルです。</p>
<p>「この文章はサンプルです」</p>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```


#### ■本文ページ（画像のみ）

[filename: p-***.xhtml]  ※例では「p-004.xhtml」

[備考]

* 内容や組み方向は、作品により異なる
* デフォルトは天ツキ左右中央揃え
* 右寄せで良ければ、<html> の class は縦組み指定でも構わない
* 画像表示の指定以外は記載しないこと

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<html
 xmlns="http://www.w3.org/1999/xhtml"
 xmlns:epub="http://www.idpf.org/2007/ops"
 xml:lang="ja"
 class="hltr"
>
<head>
<meta charset="UTF-8"/>
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/book-style.css"/>
</head>
<body class="p-image">
<div class="main">

<p><img class="fit" src="../image/img-001.jpg" alt=""/></p>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```


#### ■ページ全体の位置揃えの指定

[filename: p-***.xhtml]  ※例では「p-005.xhtml」

[備考]

* テキスト中心のページ、画像のみのページ、いずれも同様
* ページ全体の位置揃え …… <div class="main"> に「class="align-***"」を指定

```
align-justify ： 両端揃え（行末のみ行頭揃え、テキスト本文ではこれがデフォルト）
align-start   ： 行頭揃え（画像のみのページは横組みなので、start = 左揃えになることに注意）
align-left    ： 行頭揃え
align-center  ： 行中揃え（縦組み時は天地中央、横組み時は左右中央。画像ページのデフォルト）
align-end     ： 行末揃え
align-right   ： 行末揃え
```

* 以下は、画像ページ左寄せの例

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<html
 xmlns="http://www.w3.org/1999/xhtml"
 xmlns:epub="http://www.idpf.org/2007/ops"
 xml:lang="ja"
 class="hltr"
>
<head>
<meta charset="UTF-8"/>
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/book-style.css"/>
</head>
<body class="p-image">
<div class="main align-left">

<p><img class="fit" src="../image/img-001.jpg" alt=""/></p>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```


#### ■奥付ページ

[filename: p-colophon.xhtml]

[備考]

* 内容や組み方向は、各版元および作品により異なる
  （下記内容は class 名等を含め、すべてあくまでも参考用）

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<html
 xmlns="http://www.w3.org/1999/xhtml"
 xmlns:epub="http://www.idpf.org/2007/ops"
 xml:lang="ja"
 class="hltr"
>
<head>
<meta charset="UTF-8"/>
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/book-style.css"/>
</head>
<body class="p-colophon">
<div class="main">

<div class="book-title">
<div class="book-title-before">
<p>サブタイトル・前</p>
</div>
<div class="book-title-main">
<p>メインタイトル</p>
</div>
<div class="book-title-after">
<p>サブタイトル・後</p>
</div>
</div>

<div class="author">
<p>著者名１</p>
<p>著者名２</p>
</div>

<div class="label">
<p class="label-logo"><img src="../image/logo-bunko.png" alt=""/></p>
</div>

<div class="release-date">
<p>平成xx年xx月xx日□発行</p>
</div>

<div class="publisher-data">
<p class="publish-person">発行者□●●●●</p>
<p class="publish-company">発行所□株式会社●●出版</p>
<p class="publish-address">〒000-0000□東京都●●区●●1-2-3</p>
<p class="publish-url">http://www.***.co.jp/</p>
</div>

<div class="copyright">
<p>(C) author01 20xx</p>
<p>(C) author02 20xx</p>
</div>

<div class="kotowarigaki">
<p>（奥付中の断り書きがあればここに入る）</p>
</div>

<div class="original-books">
<p>本電子書籍は下記にもとづいて制作しました</p>
<p class="original-first-edition">●●文庫『底本名』平成xx年xx月xx日初版発行</p>
<p class="original-used-edition">平成xx年xx月xx日第xx版発行</p>
</div>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```


#### ■広告ページ

[filename: p-ad-***.xhtml]  ※例では「p-ad-001.xhtml」

[備考]

* 内容や組み方向は、各版元および作品により異なる
* 画像ページとはかぎらない

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<html
 xmlns="http://www.w3.org/1999/xhtml"
 xmlns:epub="http://www.idpf.org/2007/ops"
 xml:lang="ja"
 class="hltr"
>
<head>
<meta charset="UTF-8"/>
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/book-style.css"/>
</head>
<body class="p-image">
<div class="main">

<p><img class="fit" src="../image/ad-001.jpg" alt=""/></p>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```

### 【参考情報】※本ガイド非推奨項目


#### ■縦組み左右中央ページ

[filename: p-***.xhtml]

[備考]

* 組み方向の混在がサポートされているとはかぎらないので、対象とする RS の性能をよく確認すること
* 内容がページから溢れると一部が表示されなくなるので、対象とする画面サイズなどをよく確認すること
* 横組み中に縦組みブロックを入れ子とするため、組み方向が変わるときに上書きされない class の値（特に余白等）に注意すること
* 天地の margin がゼロになるので、必要に応じて <div class="main"> の内側にさらに <div> を用意して、margin か padding を指定すること（この手法では <body> 及び <div class="main"> に margin、padding の追記は不可。.p-text など <body> に指定する class に margin などが指定されているときも期待どおりの表示にはならないので要注意）
* テキスト系ページで「vrtl block-align-center」を <body> ではなく <div class="main"> に指定したいときは、<body> の margin と padding をゼロにしておくこと
* 左寄せにしたいときは下記の「block-align-center」を「block-align-left（or start）」に変更する
  ※横組みページなので「end = right」になることに注意
* 扉（.p-tobira）などでも利用方法は同じ
* 横組みの天地中央指定をしたいときは、下記の「hltr」と「vrtl」を入れ替えること
  その際、main に width-100per を指定しないと右寄せとなるので注意
  ※ベースは縦組みなので、右から左に要素が配置されます

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<html
 xmlns="http://www.w3.org/1999/xhtml"
 xmlns:epub="http://www.idpf.org/2007/ops"
 xml:lang="ja"
 class="hltr"
>
<head>
<meta charset="UTF-8"/>
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/book-style.css"/>
</head>
<body class="p-text">
<div class="main vrtl block-align-center">

<div class="start-2em">      // ←ページ全体の字下げには、さらに内側に <div> を用意
<p>あいうえおかきくけこさしすせそたちつてと</p>
</div>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```


※以下は、画像を左下隅に置く例

[備考]

* 地揃えにしたいとき、body 直下の <div class="main">  の高さを 100% に指定しないと WebKit 系では位置がずれるので注意
  ※横組みにするときは、高さではなく幅を 100% に指定すること
* 下の例では、効果を確認しやすいよう、画像を画面天地 50% サイズに縮小表示してある
  （「<img class="fit max-height-050per"」部分）

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<html
 xmlns="http://www.w3.org/1999/xhtml"
 xmlns:epub="http://www.idpf.org/2007/ops"
 xml:lang="ja"
 class="hltr"
>
<head>
<meta charset="UTF-8"/>
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/book-style.css"/>
</head>
<body class="p-text">
<div class="main vrtl block-align-left height-100per">

<div class="align-end">  // ←ページ全体の下寄せには、さらに内側に <div> を用意
<p><img class="fit max-height-050per" src="../image/img-001.jpg" alt=""/></p>
</div>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```
