# Ｂ．固定レイアウト型

──────────────────────────────────────────────────

## 設定系の必須ファイル

#### ■テンプレート中の色分けについて

灰色：すべての作品で共通の部分（原則、変更しない）
青色：すべての作品の共通部分中で、作品ごとに変更する部分
赤色：そのテンプレートを利用する作品に特有の、注意すべき部分（原則、変更しない）
黒色：定型ではない部分（作品、版元により異なる）


#### ■mimetype

[filename: mimetype]

[備考]

* リフロー型と同じ

```
---------------------------------[sample code]---------------------------------
application/epub+zip
-------------------------------------------------------------------------------
```


#### ■META-INF 内の container.xml

[filename: container.xml]

[備考]

* リフロー型と同じ

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

* 基本的にリフロー型と同じ
* ファイル名が連番の場合は、適時調整（下記の例では目次が p-001.xhtml）

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
<li><a href="xhtml/p-001.xhtml">目次</a></li>
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

* リフロー型との違いは以下の点
   <package> 要素に prefix の行を追加
   <!-- Fixed-Layout Documents指定 --> 部分に <meta> 要素を２つ追加
   スタイルシートは fixed-layout-jp.css のみ
   <spine> 要素の <itemref> で、カバーページに「properties="rendition:page-spread-center"」を追加
   <spine> 要素の <itemref> で、カバー画像以外は左右ページを必ず対になるよう指定
   ※その他はリフロー型と同じ
* <spine> 要素の <itemref> で、idref の値が重複していると何も表示しなかったり（Readium）、ページがループする （Firefox の EPUBReader）ものなどがあるので、同じ画像を２度以上表示したいときは、念のため画像を呼び出す xhtml ファイルを別に用意することを推奨する（２度目の白画像なら white2.xhtml など）

```
---------------------------------[sample code]---------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<package
 xmlns="http://www.idpf.org/2007/opf"
 version="3.0"
 xml:lang="ja"
 unique-identifier="unique-id"
 prefix="rendition: http://www.idpf.org/vocab/rendition/#
         ebpaj: http://www.ebpaj.jp/"
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
<dc:identifier id="unique-id">urn:uuid:860ddf31-55a4-449a-8cc9-3c1837657a15</dc:identifier>

<!-- 更新日 -->
<meta property="dcterms:modified">2014-01-01T00:00:00Z</meta>

<!-- Fixed-Layout Documents指定 -->
<meta property="rendition:layout">pre-paginated</meta>
<meta property="rendition:spread">landscape</meta>

<!-- etc. -->
<meta property="ebpaj:guide-version">1.1.3</meta>

</metadata>

<manifest>

<!-- navigation -->
<item media-type="application/xhtml+xml" id="toc" href="navigation-documents.xhtml" properties="nav"/>

<!-- style -->
<item media-type="text/css" id="fixed-layout-jp" href="style/fixed-layout-jp.css"/>

<!-- image -->
<item media-type="image/jpeg" id="cover"      href="image/cover.jpg" properties="cover-image"/>
<item media-type="image/jpeg" id="i-white"    href="image/i-white.jpg"/>
<item media-type="image/jpeg" id="i-001"      href="image/i-001.jpg"/>
<item media-type="image/jpeg" id="i-002"      href="image/i-002.jpg"/>
<item media-type="image/jpeg" id="i-003"      href="image/i-003.jpg"/>
<item media-type="image/jpeg" id="i-004"      href="image/i-004.jpg"/>
<item media-type="image/jpeg" id="i-005"      href="image/i-005.jpg"/>
<item media-type="image/jpeg" id="i-colophon" href="image/i-colophon.jpg"/>

<!-- xhtml -->
<item media-type="application/xhtml+xml" id="p-cover"    href="xhtml/p-cover.xhtml"    properties="svg"/>
<item media-type="application/xhtml+xml" id="p-white"    href="xhtml/p-white.xhtml"    properties="svg"/>
<item media-type="application/xhtml+xml" id="p-001"      href="xhtml/p-001.xhtml"      properties="svg"/>
<item media-type="application/xhtml+xml" id="p-002"      href="xhtml/p-002.xhtml"      properties="svg"/>
<item media-type="application/xhtml+xml" id="p-003"      href="xhtml/p-003.xhtml"      properties="svg"/>
<item media-type="application/xhtml+xml" id="p-004"      href="xhtml/p-004.xhtml"      properties="svg"/>
<item media-type="application/xhtml+xml" id="p-005"      href="xhtml/p-005.xhtml"      properties="svg"/>
<item media-type="application/xhtml+xml" id="p-colophon" href="xhtml/p-colophon.xhtml" properties="svg"/>
<item media-type="application/xhtml+xml" id="p-white2"   href="xhtml/p-white2.xhtml"   properties="svg"/>

</manifest>

<spine page-progression-direction="rtl">

<itemref linear="yes" idref="p-cover"    properties="rendition:page-spread-center"/>
<itemref linear="yes" idref="p-white"    properties="page-spread-right"/>
<itemref linear="yes" idref="p-001"      properties="page-spread-left"/>
<itemref linear="yes" idref="p-002"      properties="page-spread-right"/>
<itemref linear="yes" idref="p-003"      properties="page-spread-left"/>
<itemref linear="yes" idref="p-004"      properties="page-spread-right"/>
<itemref linear="yes" idref="p-005"      properties="page-spread-left"/>
<itemref linear="yes" idref="p-colophon" properties="page-spread-right"/>
<itemref linear="yes" idref="p-white2"   properties="page-spread-left"/>

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


#### ■カバーページ

[filename: p-cover.xhtml]

[備考]

* 下記の青字の３箇所に画像の原寸サイズを記載
* 画像サイズは作品内ですべて統一する。

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
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/fixed-layout-jp.css"/>
<meta name="viewport" content="width=848, height=1200"/>
</head>
<body epub:type="cover">
<div class="main">

<svg xmlns="http://www.w3.org/2000/svg" version="1.1"
 xmlns:xlink="http://www.w3.org/1999/xlink"
 width="100%" height="100%" viewBox="0 0 848 1200">
<image width="848" height="1200" xlink:href="../image/cover.jpg"/>
</svg>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```


#### ■本文ページ

[filename: p-***.xhtml] ※例では「p-002.xhtml」

[備考]

* 「epub:type="cover"」が無いこと以外は、カバーページと同じ

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
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/fixed-layout-jp.css"/>
<meta name="viewport" content="width=848, height=1200"/>
</head>
<body>
<div class="main">

<svg xmlns="http://www.w3.org/2000/svg" version="1.1"
 xmlns:xlink="http://www.w3.org/1999/xlink"
 width="100%" height="100%" viewBox="0 0 848 1200">
<image width="848" height="1200" xlink:href="../image/i-002.jpg"/>
</svg>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```


#### ■本文イメージマップ（クリッカブルマップ）ページ

[filename: p-***.xhtml] ※例では「p-001.xhtml」

[備考]

* a 要素の xlink:href 属性に、リンク先のファイル名を記載
* rect 要素の x と y 属性に、クリック範囲の開始位置（左上）の座標を記載
* rect 要素の width と height 属性に、クリック範囲のサイズを記載
* 現状では未対応の RS やリンクの個数に制限がある RS が存在するため、利用の際は対象とする RS の性能や挙動を事前によく確認すること

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
<title>作品名</title>
<link rel="stylesheet" type="text/css" href="../style/fixed-layout-jp.css"/>
<meta name="viewport" content="width=848, height=1200"/>
</head>
<body>
<div class="main">

<svg xmlns="http://www.w3.org/2000/svg" version="1.1"
 xmlns:xlink="http://www.w3.org/1999/xlink"
 width="100%" height="100%" viewBox="0 0 848 1200">
<image width="848" height="1200" xlink:href="../image/i-001.jpg"/>
<a xlink:href="p-002.xhtml" target="_top"><rect fill-opacity="0.0" x="476" y="1000" width="300" height="60"/></a>
<a xlink:href="p-colophon.xhtml" target="_top"><rect fill-opacity="0.0" x="476" y="1075" width="300" height="60"/></a>
</svg>

</div>
</body>
</html>
-------------------------------------------------------------------------------
```
