# リーディングシステムに期待する動作

本ガイドでは、リーディングシステム（以下 RS）が、本項に挙げるような動作をするものと想定して電子書籍データを記述する。
なお、当ガイドに準じた電子書籍データであることを RS に解釈させるキーワードとして、パッケージ文書（OPF ファイル）には、以下のメタデータを記載する。

```
  <meta property="ebpaj:guide-version">1.1.3</meta>
  ※接頭辞の宣言として package 要素内の prefix 属性に「ebpaj: http://www.ebpaj.jp/」を記載
```

また、XHTML、CSS、SVG といった出版物リソース（Publication Resource）の解釈と表示については、特筆しないかぎり、Readium Foundation が配布する「Readium Custom Chromium binary for Mac OS X（Readium-Chromium）」上で動作する Readium に準拠するものとする。

本ガイドでは、単に「Readium」と書いたときは、上記の「Readium-Chromium 上で動作する Readium」のことを指すものとする。

以下、パッケージ文書やナビゲーション文書、コンテンツ文書、スタイルシートなど、書籍データを構成する記述ファイル全般を称して、便宜上「文書ファイル」と記す。


## ■文書ファイルの基本

#### 文字コード

文書ファイルの記述に用いる文字コードは「UTF-8」とする。

BOM の有無を問わず、同様に解釈されるものとする。

※ただし、本ガイドでは、BOM 無しで文書ファイルを保存することを推奨する。

#### 改行コード

文書ファイルにおける改行コードは「CR+LF」「CR」「LF」の、いずれも正しく解釈されるものとする。

ただし、同一ファイル内での混在がある場合はこの限りではない。

※本ガイドでは、文書ファイルの同一ファイル内での改行コードの混在は、避けることを推奨する。

#### ソース中の空白、改行、コメント等の扱い

文書ファイルの記述ルールとその解釈は、基本的に XHTML に準じる。

ソース中のコメント行は適切に無視、要素中の属性の順番は任意、属性間の１つ以上の空白・改行・タブ文字は適切に処理等、Web標準をサポートした代表的なモダンブラウザと同程度の厳密さと自由度を担保する。

#### META-INF 内の container.xml

本ガイドでは OPF ファイルが２つ以上ある例を記載しないが、２つ以上の OPF がある場合も、EPUB 3 の仕様に則り適切に処理されるものとする。


## ■パッケージ文書（Package Document / OPF ファイル）

#### ページ進行方向の遵守

コンテンツ文書やスタイルシートに記された「-epub-writing-mode」の指定にかかわらず、書籍データの「ページ進行方向」は、パッケージ文書の spine 要素に記された「page-progression-direction」の方向に従う。

XHTML ファイル内容の進行方向は、各 XHTML ファイルの body 要素に指定された「-epub-writing-mode」に従う。

html 要素に指定された「-epub-writing-mode」は、適切に body 要素に継承されるものとする。

たとえば「page-progression-direction」が「rtl（右から左）」で、XHTMLの「-epub-writing-mode」が「horizontal-tb（横組み）」の場合、テキストは画面内で「左から右へ」伸びるように表示されるが、新たなページは「右から左へ」と増えていくことが望まれる。

#### spine 要素における指定の遵守

spine 要素での指定順を正しく反映するものとする。

itemref要素における「linear」属性の「yes」「no」指定を反映するものとする。「linear」属性が「yes」のときは、たとえカバーページであれ、勝手に非表示としない。

itemref 要素における「properties」属性の「page-spread-right」「page-spread-left」指定を反映するものとする。

spine 要素に記載がないものは、書籍のページとして表示されないものとする。

#### 廃止要素の利用に依存しない

guide 要素は廃止されたので、本ガイドではこれに依存する機能に対応するための記述はしない。

#### メタデータ等の扱い

RS に <dc:title> の情報を表示する機能がある場合、必ずRS内のどこかで、記載内容のすべてが画面に表示されるものと想定する。

RS に <dc:creator> の情報を表示する機能があり、複数の <dc:creator> がある場合、必ず RS 内のどこかで、記載内容のすべてが画面に表示されるものと想定する（複数著作者名の連結時の記号や、役割表記の表示等は、RS に一任するものとする）。

複数の著作者名を一人ずつ分けるか、ひとつの <dc:creator> に全員記載するかは版元の指示に従う。分けて入れる場合、版元は各著作者の「role」の値と、著作者の表示順序を必ず指示するものとする。

「file-as」で指定した整列用カナは、読者に対しては表示されないことを想定する。

ファイル id（「unique-identifier」）に用いるコード体系は定めない（版元の指示に従う。特に指示がない場合は uuid を挿入する）。

更新日は特に指示がない場合、後のファイル管理の便宜を考えて、納品予定日とする。

更新日は読者に対して表示されないことが望ましい。


## ■ナビゲーション文書（EPUB Navigation Document）

#### ナビゲーション文書の優先的解釈

ナビゲーション文書と ncx ファイルが同梱されていた場合、ncx ではなく、ナビゲーション文書を優先的に解釈し、ncx は適切に無視するものとする。

なお、本ガイドでは、廃止された ncx に依存する機能に対応するための記述はしない。

#### ナビゲーション文書の表示

ナビゲーション文書の表示のされ方については、RS に一任するものとする。

ナビゲーション文書中にリンク以外の項目を含められるかどうかは、本仕様では想定しない。


## ■スタイルシートの基本

#### CSS プロパティの制限と解釈の基準

現時点での各社 RS による CSS 対応状況を鑑みて、制作時の利用を想定する CSS プロパティを必要最低限と思われるものに絞った。また、日本語書籍の表現としてベーシックなものでありながら、RS ごとに解釈が異なり、同一ソースで表示可能な書籍データ制作を困難にしている各要素については、主として Readium の解釈を基準とし、足りない部分は既存 RS による解釈をある程度参考にしつつ、記述方法を定めることとした。想定しないプロパティを利用する際には、各版元が自己責任で、RS での表示やガイド内の他の指定等との調整を図るものとする。

#### デフォルトスタイルセットの利用

本ガイドでは、基本的なスタイルセットを用意し、主にそのセット内の class を用いて記述することを主眼とする。ただし、本ガイドでは CSS ファイルのカスタマイズを許容しているため、class の新規追加や、既存 class 名の変更、また既存 class が他のプロパティを含むなどの可能性がある。RS は class 名を固定ととらえて、それを頼りにするのではなく、そこで指定された CSS プロパティに適切に対応するものとする。

本ガイドに沿った書籍データで指定される可能性のあるプロパティの種類は、後述の「RS による対応を想定する HTML 要素と CSS プロパティ」を参照のこと。

#### 代替スタイルシートの非利用

ユーザーによる縦組みと横組みの切り替え処理については、RS に一任する。

もしこういった機能を RS が用意するのであれば、以下の２点への配慮を期待する。

* デフォルトでは、作品データ内で指定された組み方向で表示する。
* 作品データ内の指定と違う方向に切り替えた際の表示については、著者や制作者の意図にそぐわない場合があることを、RS 内に含まれる機能紹介部分やヘルプなどに明示する。

ユーザーによる組み方向の切り替え処理に対応しないのは、現状では、実際に切り替えて表示させる環境の不足や、制作・監修の指標となるものがないこと、また組み方向によらないデザインの一助となる、論理方向指定等の利用が可能な CSS プロパティがまだ整備されておらず、そのため組み方向の任意の切り替えに対処するのに非常に困難が伴い、場合によっては新規の書籍をデザインするような労力が必要となることなどが理由である。

#### 「-epub-」接頭辞付き CSS プロパティの優先的解釈

未勧告の CSS3 から先行採用された CSS プロパティでは、原則として「-epub-」接頭辞付きのものが優先的に解釈されるものとする。ただし、現状の RS の仕様や、Webブラウザでの簡易チェックなどへの配慮として、「-epub-」接頭辞が必要なプロパティには「-webkit-」接頭辞も併記する。

※今後増えるであろうベンダーごとに接頭辞を追加しつづけるのは、大変困難な作業となる。また、WebKit の CSS 記述方法や解釈が後日変更になったときに、「-webkit-」を優先的に解釈してしまう RS で不都合が起こらないともかぎらない。CSS ファイル内でどのような順で記載されていたとしても、RS は「-epub-」接頭辞付きプロパティを最優先で解釈することが望ましい。

なお、「-epub-」接頭辞が利用可能なプロパティは、EPUB 3.0.1 仕様中に記載されたものだけである。
ブラウザなどで「-webkit-」接頭辞で利用可能なプロパティを、単純に「-epub-」接頭辞に置き換えても意味がないので、利用者は注意すること。


#### @import ルールの採用

XHTML 側での CSS ファイル指定の記述簡易化・統一化と、CSS の柔軟なカスタマイズ性確保のため、基本的な XHTML テンプレートでは、link 要素で メイン CSS のみを読み込み、@import ルールを用いて、メイン CSS ファイル内部から必要となる各 CSS ファイルを読み込むこととする。ただし、制作者がページによって読み込む CSS ファイルを変更したい場合はこの限りではない（特定ページでのみ、版元別スタイルセットを読み込みたい場合等）。RS は XHTML から複数の CSS ファイルを呼び出すことも可能であるものとする。
@import で読み込まれたファイル内で、さらに @import を用いることは推奨しない。

#### 非搭載フォント指定の適切な無視

フォント指定において、RS が搭載しないフォントの指定は、適切に無視されるものとする。
@font-face で指定したフォントセットについても、RS に搭載されないフォント指定は、同様に無視されるものとする。

#### html 要素への指定

html 要素には、原則として組み方向と書体の指定しか行わない。
いずれも、適切に body 要素へ継承されるものとする。

#### RS によるデフォルトスタイルシート指定の上書き

RS の設定したデフォルトスタイルシートは、書籍データ側で上書きできるものとする。
デフォルトスタイルシートの情報は、版元に対して公開されることが望ましい。


## ■文字・テキスト

#### 文字集合

少なくとも、以下の文字集合をサポートするものとする。

> JIS X 0213:2004 （Unicode ではサロゲートペア領域に含まれる文字も含む）

#### 書体

少なくとも、以下の２書体を利用可能とする。

```
等幅の明朝系フォント
等幅のゴシック系フォント
```

また、上記書体は以下のように「Generic font families」に割り当てる。

```
serif      ： 明朝
sans-serif ： ゴシック
```

フォントサイズが同じ場合、両書体の em サイズは等しいものとする。

全角、半角の違いに関わらず、em サイズは等しいものとする。

スタイルシートで -epub-text-orientation を用い、文字の向きを変更した場合でも、em サイズは不変とする。

両書体の各文字の表示上の幅は、極端に違わないものとする。

※インデントでの位置あわせ等、縦組みにおける基礎的な組版表現を再現する上で、フォントはプロポーショナルではなく、等幅であることが望ましい。ただし本ガイドでは、横組み時の字下げ等による位置合わせでは、期待するような効果が得られない可能性について注意を喚起する。



#### 縦組み時の文字の向き

Unicode Consortium の提示する、以下の文書に準拠した表示を想定する。

```
「Unicode Technical Report #50 Unicode Vertical Text Layout (UTR#50) Revision 13」
http://www.unicode.org/reports/tr50/
```

縦組み時の文字の向きは、以下の CSS プロパティで変更されるものとして指定する。
向きの変更が適用される文字は、上記文書の記述に従うものとする。

```
  正立               : -epub-text-orientation: upright;
  右90度回転（横転） : -epub-text-orientation: sideways;
```

EPUB 3.0 時点の仕様であった値「rotate-right」が横転指定のために用いられている、または「sideways」と併記されている場合は、sideways と同等の動作をするものとする。

※ただし現状では、半角文字に upright を指定したとき、複数の RS で文字のセンター位置が揃わないことが確認されている。そのため本ガイドでは、やむを得ず、-epub-text-combine で一部を代用する。

-epub-text-combine はその名が示すように「combine（結合）」のためのプロパティなので、文字の向きを変更するために用いるのは避けることが望ましい。

また、CSS の upright 指定では、「Tr」「Tu」「R」「U」といった既定の向きに関わらず、縦書き用グリフがあればそれを表示することになっているため、組み方向により形が変わりそうな文字には注意が必要である。

#### 縦組み時の vertical-align

画像及び正立すべき文字は「CSS Writing Modes Level 3 W3C Candidate Recommendation, 20 March 2014」の「4.2 Text Baselines」で「The central baseline」として示された表示（中央揃え）になるものと想定する。

http://www.w3.org/TR/2014/CR-css-writing-modes-3-20140320/#central-baseline

※なお、小書き文字のように、サイズを小さくした文字を、縦組み行内の左右いずれかに揃えたい場合がある。そのため「text-top」「text-bottom」の指定が、行の高さなどではなく、親要素の文字のインラインボックスの右辺、左辺（縦組み時）への揃えとして反映されることが望ましい（同一行内に拡大された文字があり、拡張インラインボックスが押し広げられても、指定した文字の位置がずれないことが望まれる）。本ガイドでは、安全のため「super」「sub」を用いることを推奨するが、これらは行から少し外側にはみ出してしまうため、期待とは多少異なる表示がなされる場合がある。

#### 行間の自動伸張

行間は原則として、書籍データでの指定が変わらないかぎり均等であるものとする。ただし、最終行がページ内に収まらない場合、その行が次ページに送られて後に空白ができるのは、やむを得ず許容する。

同一行内でフォントサイズが大きくなったり、縦中横などで文字が行から溢れ広がる場合、本来は、サイズや幅の変更による影響をある程度見越してデータを制作すべきであるが、リフロー型の書籍では行末折り返し位置を事前に想定できないため、手動での回避が難しい場合がある。そのため行間は極力維持されるのが望ましいが、どうしても隣行の文字やルビ・傍点等と重なってしまうような場合は、行間が自動的に広がるものとする。

#### 禁則

禁則処理については、各社 RS の現状を鑑みて、現時点では RS 次第であるものと想定する。

## ■画像

#### 画像の種類

JPEG、PNG、GIF が利用可能であるものとする。

また、PNG と GIF は透過画像が利用可能であることが望ましい。

#### 外字画像
本ガイドの画像縮小指定で、本文テキスト１文字分のサイズで表示できるものとする。

画像サイズの限界は特に定めない。

推奨値

```
画像サイズ       : 128px × 128px
画像形式         : 8bit の透過 PNG
アンチエイリアス : なし
```

※背景色をユーザーが自由に変更できる RS への配慮として、背景色を透明にして保存した 8bit PNG を推奨する。ただし、必ずしも RS が透過画像に対応しているとはかぎらないため、本文の背景に色を敷く場合には、同じ背景色を用いた外字画像を利用するほうが安全である。

#### 画像やブロック要素のサイズ指定における、サイズの最大値指定

max-height、max-width の挙動は、現在ブラウザ間でも解釈の差異がある項目のひとつだが、本ガイドに沿った書籍データにおいては、RS は Readium に準拠した表示を行うものとする。

これらは主に、画像のページフィット、および外字画像の挿入に用いられる。

CSS3 以降でページフィットに適した指定が採用され、それが EPUB 仕様に取り入れられるまでは、本ガイドでは Readium での動作に基づいた最大サイズ指定の挙動を前提とする。

#### 画像等の置換要素やブロック要素およびインラインブロック要素がページをまたぐか否かの判断

以下で述べる「要素の表示用サイズ」とは、スタイルシート等で指定されたサイズがあればそれを示すものとする。要素に指定された { max-width: 100%; } 等のサイズの最大値指定は、先に述べたように、Readium 準拠のサイズ解釈を行うものとする。

テキスト行内の要素がテキストの幅より大きく、かつ RS のページ内で「要素が表示される予定のスペース」に、表示すべき要素がおさまりきらないときは、その該当行ごと次ページに送ることとする。

また、インラインかブロックかにかかわらず、RS のページ内で、画像等の「要素が表示される予定のスペース」に、表示すべきその要素がおさまりきらないときは、「要素の表示用サイズ」のうち、ページ進行方向の幅（縦組みであれば横幅）が RS のページ表示領域のページ進行方向の幅と比べて大きいか小さいかで、ページをまたぐか否かの判断がなされるものとする。

##### ページ進行方向の幅と等しいか、その幅より小さい場合

スペースにおさまりきらなかった要素がページをまたがないよう、次ページに要素を表示させる。

※ページフィット用の指定がされている場合は、次ページに送られたあと、ページフィットして表示されるものとする。

このとき、要素のページ進行方向でない側（縦組みなら高さ）には、要素にページフィット用の指定をしていないかぎり、画面の残りスペースより大きな要素が行末方向にはみだしてしまい、表示されずともやむなしとする。

##### ページ進行方向の幅より大きい場合

そのままページをまたいで表示させる。

#### 縮小画像のユーザー操作による拡大

サイズ指定やページフィット指定など、原寸より縮小されて表示されている画像は、ピンチイン等のユーザー操作により原寸サイズにまで拡大可能であるものとする。

※小さな画面であっても、画像内のテキストなどを読ませたいケースがあるため


## ■カバー画像

#### 書棚等における代替画像の用意

カバー画像が存在するとはかぎらない（権利処理上の事情等による）ため、RS はカバー画像が存在しないときは、書棚表示用の代替画像を用意するなどして、支障なく動作可能とすることが望ましい。

#### ファイル名

カバー画像のファイル名は、版元より特に指示がない場合、RS 側のサムネイル表示の速度向上に配慮する目的で、すべて同じ名前（cover.jpg）とする。


## ■ページメディアの余白

#### body 要素の余白指定

現状では多くの場合、RS は書籍データ側から制御できない余白を画面内に追加する。
そのため、本ガイドでは、body 要素の margin と padding は 0 をデフォルトとする。

#### 本文表示領域内への、RS による勝手な余白の追加・削除

RS が body 要素内部の利用可能な画面サイズに影響するような、独自の余白を追加することはないものとする。また、書籍データに指定された margin や padding、空白行を、RS が勝手に詰めて表示することはないものとする。

※たとえばファイルの先頭にだけ自動的に消せない余白を追加されたり、指定した余白を恣意的に詰められたりすると、著者や制作者の意図を読者に正しく伝えられなくなる恐れがあるので、絶対にそのようなことが起こらないことが望ましい。


## ■その他 HTML 要素

#### 空白行のための <br/>

日本語書籍における空白行の位置づけや、印刷用データへの再利用、ハンドリングの容易さ等を鑑み、<br/> だけの行を、空白の１行として扱う。
また margin、padding をゼロにした p 要素や div 要素を用いた <p><br/></p>、<div><br/></div> といった記述も、同様に空白の１行として扱う。

#### ルビ

「<ruby>漢<rt>かん</rt>字<rt>じ</rt></ruby>」の指定（熟語ルビ風指定）が利用可能であるものとする。
ルビと圏点・傍点が同時指定されたときは、ルビが優先されるものとする。
外字画像にもルビの指定が可能であるものとする。
「<rt>ルビ文字列</rt>」で示されるルビ文字列には、以下の表現を含むことができるものとする。

* 通常のテキストすべて（欧文や数字も含む）
* 数値参照、文字参照
* 画像外字
* 文字の向き指定（-epub-text-orientation）
* 縦中横指定（-epub-text-combine 及び -epub-text-combine-horizontal）

※文字の向き指定にも利用するため

ルビ文字列中の色やサイズなど、装飾関連は今回想定しない。
vertical-align の変更（上付・下付など）も、今回は考慮しない。

#### ページ内リンク（アンカーリンク）

「<a href="ファイル名#アンカー名">テキスト</a>」で指定されたリンク（ファイル名は同一ファイル内であれば省略可）をクリックまたはタップなどして辿った場合は、「<要素名 id="アンカー名">」で指定された要素の位置にジャンプするものとする。ファイル名、アンカー名は、実際に使用する際には全角文字や空白文字を使用しないこと。

なお、そのジャンプ先の要素が必ず画面の先頭行位置に表示されるのか、それとも事前に計算されたページ内の配置を変更することなく、単にその要素を含むページを表示するのかは、RS 次第とする。

※スクロールメディアでは前者のほうが自然であったが、ページメディアでは後者のほうが都合が良い場合も考えられる。

#### nav 要素とリスト系要素

本ガイドでは、ナビゲーション文書以外での nav 要素と、ol、li などのリスト系要素の利用を想定しない。

ナビゲーション文書中では、EPUB Content Documents 3.0.1 の記載にあるように、リスト要素にはリスト番号を表示しないこと。


## ■その他 CSS の解釈

#### 傍線

EPUB 3.0.1 より「-epub-text-underline-position」が採用されたことを踏まえ、body には { -epub-text-underline-position: under left; } を、縦組みではテキストの右線に { text-decoration: underline; } を、左線に { text-decoration: overline; } を指定しておくものとする。

現時点ではまだ「-epub-text-underline-position」が必ず反映されるとはかぎらないため、左線に { text-decoration: underline; -epub-text-underline-position: right; } とは指定しないでおく。

{ -epub-text-underline-position: auto; } は、現段階ではまだ利用を想定しない。

text-decoration による線は、ルビや傍点にまで引かれないものとする。

また、縦中横指定と併用したとき、おかしな位置に線が出てしまわないものとする。

今回はいずれも解消されるものと想定して text-decoration を利用する。

なお、CSS2.1 の仕様上、inline-block 要素には text-decoration は効かないことになっているので、CSS ファイル内で inline-block 指定している外字画像や注釈記号などには、傍線が引かれなくてもやむを得ないものとする。

#### 縦中横

{ -epub-text-combine: horizontal; }  及び { -epub-text-combine-horizontal: all; } は、半角３桁まで縦中横をするものと想定する。
縦中横された文字列は、まとめて１文字扱いされるものとする。

例）

縦中横に指定した傍線（text-decoration）は、縦組みのときはその外側の横にひとつだけつくものとする。

縦中横に指定した傍点は、縦中横された文字列を１文字として、その文字列のまとまりひとつにつき、ひとつの点を打つものとする。

縦中横に指定したルビは、縦中横された文字列を１文字として、ルビのルールどおり表示されるものとする。

なお、EPUB 3.0.1 の仕様では、[CSS3WritingModes-20121115] での変更されたプロパティ名を受けて
「-epub-text-combine-horizontal」が新たに採用されているが、勧告候補となった現在の[CSS3Writing
Modes] （20140320）ではさらにプロパティ名が変更され、「text-combine-upright」となっていることから、縦中横の指定には、当面これまでの「-epub-text-combine」の利用を想定するものとする。RS には、将来的にもこのプロパティへの対応が強く望まれる。さらに「-epub-text-combine-horizontal」が用いられた場合も、正しく縦中横指定と解釈されるものとする。また、これらを利用する際は、今後のブラウザ等での表示確認の便宜も考えて、 { text-combine-upright: all; } も追加で指定しておくことを推奨する

#### 非表示指定

{ display: none; } は正しく解釈されるものとする。

ただし現時点では、安全を考えて、読者に対して表示されるべきでない要素は初めから書籍データに含めないか、より安全性の高いコメントアウトを用いることを推奨する。

なお、{ visibility: hidden; } は正しくトルアキで解釈されることを期待する。ただし、JavaScript の利用や、マウスオーバーによる表示切り替えなどを想定しない本ガイドでは、どうしても visibility を利用せねばならぬというケースはおそらくないと思われるため、visibility を利用可能とする class を CSS ファイルに含めることは見送った。


## ■固定レイアウト

固定レイアウトへの対応は、画像のみで構成される作品に限ることとする。

画像のみで構成される固定レイアウトには、SVG ラッピングの手法を採用する。

SVG は XHTML に直接記載する。含むことのできる画像の形式は、リフロー型と同様とする。

イメージマップ（クリッカブルマップ）も、SVG 用のものを用いて画像の伸縮に対応する。

カバーページ以外は、必ずページが対になるようにデータを作成する。

カバーページ（作品データ内の１枚目のページ）だけは、EPUB Fixed Layout の指定方法により spine itemref 要素にて、properties="rendition:page-spread-center" を指定するので、必ず単体で表示されるものとする。

カバーページには見開き画面の中央に表示される指定をするが、後につづく見開き配置が崩れないのであれば、カバーページが中央に配置されなくとも許容範囲とする。


## ■その他

#### スクリプティング

現状の各社 RS の現状を踏まえ、JavaScript の利用は想定しない。

## ■RS による対応を想定する HTML 要素と CSS プロパティ

### 【HTML】

#### ルート要素

html要素

#### ドキュメントのメタデータ

head要素 / title要素 / link要素 / meta要素 / style要素

#### セクション

body要素 / h1.h6要素 / nav要素（ナビゲーション文書用にしか想定しない）

#### コンテンツのグループ化

div要素 / p要素 / hr要素
ol要素（ナビゲーション文書用にしか想定しない）/ li要素（ナビゲーション文書用にしか想定しない）
テキストレベルの意味づけ
a要素 / br要素 / ruby要素 / rt要素 / span要素
組込コンテンツ
img要素 / SVG（固定レイアウトにおけるSVGラッピング手法とイメージマップ機能のみ）

### 【CSS】

#### 値

% / px / em / inherit / #RRGGBB / #RGB / rgb(R,G,B) / 色名（17色） / transparent

#### セレクタ

タイプセレクタ「ELEMENT」

ユニバーサルセレクタ「*」

クラスセレクタ「.class」

　複数クラスの同時指定「class="class class class ..."」

　複数クラスの組み合わせ指定「.class.class」

IDセレクタ「#id」

属性セレクタ

　[att] / [att="val"] / [att~="val"] / [att|="val"]

結合子

　子孫セレクタ「A B」 / 子セレクタ「A > B」 / 兄弟セレクタ（隣接セレクタ）「A + B」

グループ化「A, B」

疑似要素

:link / :visited / :active / :hover（※マウス操作可能な RS のみ）

!important 宣言

#### @ルール

@charset / @font-face / @import / @media

#### 色・背景

color / background（色のみ） / background-color

#### マージン

margin / margin-top / margin-right / margin-bottom / margin-left

#### パディング

padding / padding-top / padding-right / padding-bottom / padding-left

#### ボーダー

border / border-top / border-right / border-bottom / border-left
border-width / border-top-width / border-right-width / border-bottom-width / border-left-width
border-style / border-top-style / border-right-style / border-bottom-style / border-left-style
border-color / border-top-color / border-right-color / border-bottom-color / border-left-color

#### フォント

font / font-family / font-size / font-style / font-weight / line-height

#### テキスト

text-align / text-decoration / text-indent / letter-spacing / vertical-align / word-wrap

#### 幅・高さ

width / height / max-width / max-height

#### 表示

display (display: block; / display: inline-block; / display: inline; / display: none;)

#### ページメディア

page-break-before / page-break-after / page-break-inside

#### CSS Text Level 3

-epub-line-break / -epub-word-break / -epub-text-align-last

#### CSS Writing Modes Module Level 3

-epub-writing-mode / -epub-text-orientation / -epub-text-combine
-epub-text-combine-horizontal

#### CSS Fonts Level 3

@font-face (font-family / font-style / font-weight / src / unicode-range)

#### CSS Text Decoration Level 3

-epub-text-emphasis / -epub-text-emphasis-color / -epub-text-emphasis-style
-epub-text-underline-position

## ■本ガイドでは、RS による対応を想定しない HTML 要素と CSS プロパティ

### 【HTML】

#### ドキュメントのメタデータ

base要素

#### スクリプティング

script要素 / noscript要素

#### セクション

section要素 / article要素 / aside要素 / header要素 / footer要素 / address要素

#### コンテンツのグループ化

blockquote要素 / ul要素 / dl要素 / dt要素 / dd要素 / figure要素 / figcaption要素

#### テキストレベルの意味づけ

em要素 / strong要素 / pre要素 / subとsup要素 / i要素 / b要素 / u要素 / s要素 / small要素
cite要素 / q要素 / dfn要素 / abbr要素 / time要素 / code要素 / var要素 / samp要素 / kbd要素
mark要素 / rp要素 / bdi要素 / bdo要素 / wbr要素 / rb要素 / rtc要素 / rp要素

#### 訂正

ins要素 / del要素

#### 組込コンテンツ

map要素 / area要素 / iframe要素 / embed要素 / object要素 / param要素 / video要素 / audio要素
source要素 / track要素 / メディア要素 / canvas要素 / MathML / SVG（固定レイアウト用途以外）

#### テーブルデータ

table要素 / caption要素 / colgroup要素 / col要素 / tbody要素 / thead要素 / tfoot要素 / tr要素
td要素 / th要素

#### フォーム

form要素 / fieldset要素 / legend要素 / label要素 / input要素 / button要素 / select要素
datalist要素 / optgroup要素 / option要素 / textarea要素 / keygen要素 / output要素
progress要素 / meter要素

#### インタラクティブな要素

details要素 / summary要素 / command要素 / menu要素

### 【CSS】

#### 値

ex / in / cm / mm / pt / pc

#### セレクタ

E:focus / E:lang(c) / E:first-child / E:first-line / E::first-line
E:first-letter / E::first-letter / E:before / E::before / E:after / E::after
E[foo^="bar"] / E[foo$="bar"] / E[foo*="bar"] / E:root / E:nth-child(n)
E:nth-last-child(n) / E:nth-of-type(n) / E:nth-last-of-type(n)
E:last-child / E:first-of-type / E:last-of-type / E:only-child / E:only-of-type
E:empty / E:target / E:enabled / E:disabled / E:checked / E:not(s) / E ~ F

#### @ルール

@page / @page:left / @page:right / @page:first

#### 色・背景

background-attachment / background-image / background-position / background-repeat

#### フォント

font-size-adjust / font-stretch / font-variant

#### テキスト

text-shadow / text-transform / white-space / word-spacing

#### 幅・高さ

min-width / min-height

#### 表示

direction / visibility / clip / overflow / unicode-bidi / z-index
display: list-item; / display: table; / display: inline-table / display: table-row-group
display: table-header-group / display: table-footer-group / display: table-row
display: table-column-group / display: table-column / display: table-cell
display: table-caption

#### ページメディア

page / size / marks / orphans / widows

#### リスト

list-style / list-style-type / list-style-position / list-style-image / marker-offset

#### 回り込み

float / clear

#### 位置

position / top / right / bottom / left

#### テーブル

border-collapse / border-spacing / caption-side / empty-cells / table-layout

#### 挿入

content / quotes / counter-reset / counter-increment

#### アウトライン

outline / outline-color / outline-style / outline-width

#### CSS 3.0 Speech

-epub-cue / -epub-pause / -epub-rest / -epub-speak / -epub-speak-as / -epub-voice-family

#### CSS Text Level 3

-epub-hyphens / text-transform: -epub-fullwidth / text-transform: -epub-fullsize-kana

#### CSS Writing Modes Module Level 3

caption-side: before / caption-side: after

#### CSS3 Multi Column

column-width / column-count / columns / column-gap / column-rule-color

#### CSS 2.0

list-style-type: cjk-ideographic / list-style-type: hebrew / list-style-type: hiragana
list-style-type: hiragana-iroha / list-style-type: katakana / list-style-type: katakana-iroha

#### EPUB 3

-epub-ruby-position / display: oeb-page-head / display: oeb-page-foot
