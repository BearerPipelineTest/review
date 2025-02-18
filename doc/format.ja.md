# Re:VIEW フォーマットガイド

Re:VIEW フォーマットの文法について解説します。Re:VIEW フォーマットはアスキー社（現カドカワ）の EWB を基本としながら、一部に RD や各種 Wiki の文法を取り入れて簡素化しています。

このドキュメントは、Re:VIEW 5.5 に基づいています。

## 段落

段落（本文）の間は1行空けて表現します。空けずに次の行を記述した場合は、1つの段落として扱われます。

例:

```
だんらくだんらく〜〜〜
この行も同じ段落

次の段落〜〜〜
```

* 2行以上空けても、1行空きと同じ意味になります。
* 空行せずに改行して段落の記述を続ける際、英文の単語間スペースについては考慮されないことに注意してください。Re:VIEW は各行を単純に連結するだけであり、TeX のように前後の単語を判断してスペースを入れるようなことはしません。

## 章・節・項・目・段（見出し）

章・節・項・目といった見出しは「`=`」「`==`」「`===`」「`====`」「`=====`」で表します。7 レベル以上は使えません。`=`のあとにはスペースを入れます。

例:

```review
= 章のキャプション

== 節のキャプション

=== 項のキャプション

==== 目のキャプション

===== 段のキャプション

====== 小段のキャプション
```

見出しは行の先頭から始める必要があります。行頭に空白を入れると、ただの本文と見なされます。

また、見出しの前に文があると、段落としてつながってしまうことがあります。このようなときには空行を見出しの前に入れてください。

## コラムなど

節や項の見出しに `[column]` を追加すると以降がコラムとして扱われ、見出しはコラムのキャプションになります。

例:

```review
===[column] コンパイラコンパイラ
```

このとき、「=」と「[column]」は間を空けず、必ず続けて書かなければなりません。

コラムは、そのコラムのキャプションの見出しと同等か上位のレベル（コラムキャプションが `===` であれば、`=`〜`===`）の見出しが登場するかファイル末尾に達した時点で終了します。これを待たずにコラムを終了する場合は、「`===[/column]`」と記述します（`=`の数はコラムキャプションと対応）。

例:

```review
===[column] コンパイラコンパイラ

コラムの内容

===[/column]
```

より下位の見出しを使うことでコラム内に副見出しを入れることができますが、それが必要になるほど長いコラムはそもそも文章構造に問題がある可能性があります。

このほか、次のような見出しオプションがあります。

* `[nonum]` : これを指定している章・節・項・段には連番を振りません。見出しは目次に含まれます。
* `[nodisp]` : これを指定している章・節・項・段の見出しは変換結果には表示されず、連番も振りません。ただし、見出しは目次に含まれます。
* `[notoc]` : これを指定している章・節・項・段には連番を振りません。見出しは目次に含まれません。

## 箇条書き

箇条書き（HTML で言う ul、ビュレット箇条書きともいう）は「` *`」で表現します。ネストは「` **`」のように深さに応じて数を増やします。

例:

```
 * 第1の項目
 ** 第1の項目のネスト
 * 第2の項目
 ** 第2の項目のネスト
 * 第3の項目
```

箇条書きには行頭に1つ以上の空白が必要です。行頭に空白を入れず「*」と書くと、ただのテキストと見なされるので注意してください。

通常の段落との誤った結合を防ぐため、箇条書きの前後には空行を入れることをお勧めします（他の箇条書きも同様）。

## 番号付き箇条書き

番号付きの箇条書き（HTML で言う ol）は「` 1. 〜`」「` 2. 〜`」「` 3. 〜`」のように示します。ネストはサポートしていません。

例:

```
 1. 第1の条件
 2. 第2の条件
 3. 第3の条件
```

番号付き箇条書きも、ただの箇条書きと同様、行頭に1つ以上の空白が必要です。

## 用語リスト

用語リスト（HTML で言う dl）は空白→「:」→空白、で始まる行を使って示します。

例:

```review
 : Alpha
    DEC の作っていた RISC CPU。
    浮動小数点数演算が速い。
 : POWER
    IBM とモトローラが共同製作した RISC CPU。
    派生として POWER PC がある。
 : SPARC
    Sun が作っている RISC CPU。
    CPU 数を増やすのが得意。
```

「`:`」それ自体はテキストではないので注意してください。その後に続く文字列が用語名（HTML での dt 要素）になります。

そして、その行以降、空白で始まる行が用語内容（HTML では dd 要素）になります。次の用語名か空行になるまで、1つの段落として結合されます。

## ブロック命令とインライン命令

見出しと箇条書きは特別な記法でしたが、それ以外の Re:VIEW の命令はほぼ一貫した記法を採用しています。

ブロック命令は1〜複数行の段落に対して何らかのアクション（たいていは装飾）を行います。ブロック命令の記法は次のとおりです。

```
//命令[オプション1][オプション2]…{
対象の内容。本文と同じような段落の場合は空行区切り
 …
//}
```

オプションを取らなければ単に `//命令{` という開始行になります。いずれにせよ、開始と終了は明確です。オプション内で「]」という文字が必要であれば、`\]` でリテラルを表現できます。

亜種として、一切段落を取らないブロック命令もごく少数あります。

```
//命令[オプション1][オプション2]…
```

インライン命令は段落、見出し、ブロック内容、一部のブロックのオプション内で利用でき、文字列内の一部に対してアクション（装飾）を行います。

```
@<命令>{対象の内容}
```

内容に「}」という文字が必要であれば、`\}` でリテラルを表現できます。なお、内容の末尾を「\」としたい場合は、`\\` と記述する必要があります（たとえば `@<tt>{\\}`）。

記法および処理の都合で、次のような制約があります。

* ブロック命令内には別のブロック命令をネストできません。
* ブロック命令内には見出しや箇条書きを格納できません。
* インライン命令には別のインライン命令をネストできません。

### インライン命令のフェンス記法

インライン命令において `}` や 末尾 `\` を多用したい場合、それぞれ `\}` や `\\` のようにエスケープするのはわずらわしいことがあります。そのようなときには、インライン命令の囲みの `{ }` の代わりに `$ $` あるいは `| |` を使って内容を囲むことで、エスケープ表記せずに記述できます。

```
@<命令>$対象の内容$
@<命令>|対象の内容|
```

例:

```review
@<m>$\Delta = \frac{\partial^2}{\partial x_1^2}+\frac{\partial^2}{\partial x_2^2} + \cdots + \frac{\partial^2}{\partial x_n^2}$
@<tt>|if (exp) then { ... } else { ... }|
@<b>|\|
```

あくまでも代替であり、推奨する記法ではありません。濫用は避けてください。

## ソースコードなどのリスト

ソースコードなどのリストには`//list`を使います。連番を付けたくない場合は先頭に `em`（embedded の略）、行番号を付ける場合は末尾に `num` を付加します。まとめると、以下の4種類になります。

* `//list[識別子][キャプション][言語指定]{ 〜 //}`
  * 通常のリスト。言語指定は省略できます。
* `//listnum[識別子][キャプション][言語指定]{ 〜 //}`
  * 通常のリストに行番号をつけたもの。言語指定は省略できます。
* `//emlist[キャプション][言語指定]{ 〜 //}`
  * 連番がないリスト。キャプションと言語指定は省略できます。
* `//emlistnum[キャプション][言語指定]{ 〜 //}`
  * 連番がないリストに行番号を付けたもの。キャプションと言語指定は省略できます。

例:

```review
//list[main][main()][c]{    ←「main」が識別子で「main()」がキャプション
int
main(int argc, char **argv)
{
    puts("OK");
    return 0;
}
//}
```

例:

```review
//listnum[hello][ハローワールド][ruby]{
puts "hello world!"
//}
```

例:

```review
//emlist[][c]{
printf("hello");
//}
```

例:

```review
//emlistnum[][ruby]{
puts "hello world!"
//}
```

言語指定は、ハイライトを有効にしたときに利用されます。

コードブロック内でもインライン命令は有効です。

また本文中で「リスト X.X を見てください」のようにリストを指定する場合は、インライン命令 `@<list>` を使います。`//list` あるいは `//listnum` で指定した識別子を指定し、たとえば「`@<list>{main}`」と表記します。

他章のリストを参照するには、後述の「章ID」を指定し、たとえば `@<list>{advanced|main}`（`advanced.re` ファイルの章にあるリスト `main` を参照する）と記述します。

### 行番号の指定

行番号を指定した番号から始めるには、`//firstlinenum`を使います。

例:

```review
//firstlinenum[100]
//listnum[hello][ハローワールド][ruby]{
puts "hello world!"
//}
```

### ソースコード専用の引用

ソースコードを引用するには次のように記述します。

例:

```review
//source[/hello/world.rb]{
puts "hello world!" # キャプションあり
//}

//source{
puts "hello world!" # キャプションなし
//}

//source[/hello/world.rb][ruby]{
puts "hello world!" # キャプションあり、ハイライトあり
//}

//source[][ruby]{
puts "hello world!" # キャプションなし、ハイライトあり
//}
```

ソースコードの引用は、`//emlist` とほぼ同じです。HTML の CSS などでは区別した表現ができます。

## 本文中でのソースコード引用

本文中でソースコードを引用して記述するには、`code` を使います。

例:

```review
@<code>{p = obj.ref_cnt}
```

## コマンドラインのキャプチャ

コマンドラインの操作を示すときは `//cmd{ 〜 //}` を使います。インライン命令を使って入力箇所を強調するのもよいでしょう。

例:

```
//cmd{
$ @<b>{ls /}
//}
```

## 図

図は `//image{ 〜 //}` で指定します。後述するように、識別子に基づいて画像ファイルが探索されます。

ブロック内の内容は単に無視されますが、アスキーアートや、図中の訳語などを入れておくといった使い道があります。

例:

```
//image[unixhistory][UNIX系OSの簡単な系譜]{
          System V 系列
            +----------- SVr4 --> 各種商用UNIX（Solaris, AIX, HP-UX, ...）
V1 --> V6 --|
            +--------- 4.4BSD --> FreeBSD, NetBSD, OpenBSD, ...
          BSD 系列

                --------------> Linux
//}
```

3番目の引数として、画像の倍率・大きさを指定することができます。今のところ「scale=X」で倍率（X 倍）を指定でき、HTML、TeX ともに紙面（画面）幅に対しての倍率となります（0.5 なら半分の幅になります）。3番目の引数をたとえば HTML と TeX で分けたい場合は、`html::style="transform: scale(0.5);",latex::scale=0.5` のように `::` でビルダを明示し、`,` でオプションを区切って指定できます。

※TeX において原寸からの倍率にしたいときには、`config.yml` に `image_scale2width: false` を指定してください。

また、本文中で「図 X.X を見てください」のように図を指定する場合は、インライン命令 `@<img>` を使います。`//image` で指定した識別子を用いて「`@<img>{unixhistory}`」のように記述します（`//image` と `@<img>` でつづりが違うので注意してください）。

他章の図を参照するには、リストと同様に「章ID」を指定します。たとえば `@<img>{advanced|unixhistory}`（`advanced.re` ファイルの章にある図 `unixhistory` を参照する）と記述します。

### 画像ファイルの探索

図として貼り込む画像ファイルは、次の順序で探索され、最初に発見されたものが利用されます。

```
1. <imgdir>/<builder>/<chapid>/<id>.<ext>
2. <imgdir>/<builder>/<chapid>-<id>.<ext>
3. <imgdir>/<builder>/<id>.<ext>
4. <imgdir>/<chapid>/<id>.<ext>
5. <imgdir>/<chapid>-<id>.<ext>
6. <imgdir>/<id>.<ext>
```

* `<imgdir>` はデフォルトでは images ディレクトリです。
* `<builder>` は利用しているビルダ名（ターゲット名）で、たとえば `--target=html` としているのであれば、images/html ディレクトリとなります。各 Maker におけるビルダ名は epubmaker および webmaker の場合は `html`、pdfmaker の場合は `latex`、textmaker の場合は `top` です。
* `<chapid>` は章 ID です。たとえば ch01.re という名前であれば「ch01」です。
* `<id>` は //image[〜] の最初に入れた「〜」のことです（つまり、ID に日本語や空白交じりの文字を使ってしまうと、後で画像ファイル名の名前付けに苦労することになります！）。
* `<ext>` は Re:VIEW が自動で判別する拡張子です。ビルダによってサポートおよび優先する拡張子は異なります。

各ビルダでは、以下の拡張子から最初に発見した画像ファイルが使われます。

* HTMLBuilder (EPUBMaker、WEBMaker)、MARKDOWNBuilder: .png、.jpg、.jpeg、.gif、.svg
* LATEXBuilder (PDFMaker): .ai、.eps、.pdf、.tif、.tiff、.png、.bmp、.jpg、.jpeg、.gif
* それ以外のビルダ・Maker: .ai、.psd、.eps、.pdf、.tif、.tiff、.png、.bmp、.jpg、.jpeg、.gif、.svg

### インラインの画像挿入

段落途中などに画像を貼り込むには、インライン命令の `@<icon>{識別子}` を使います。ファイルの探索ルールは同じです。

## 番号が振られていない図

`//indepimage[ファイル名][キャプション]` で番号が振られていない画像ファイルを生成します。キャプションは省略できます。

例:

```
//indepimage[unixhistory2]
```

同様のことは、`//numberlessimage`でも使えます。

例:

```
//numberlessimage[door_image_path][扉絵]
```

## グラフ表現ツールを使った図

`//graph[ファイル名][コマンド名][キャプション]` で各種グラフ表現ツールを使った画像ファイルの生成ができます。キャプションは省略できます。

例: gnuplotの使用

```
//graph[sin_x][gnuplot][Gnuplotの使用]{
plot sin(x)
//}
```

コマンド名には、「`graphviz`」「`gnuplot`」「`blockdiag`」「`aafigure`」「`plantuml`」のいずれかを指定できます。ツールはそれぞれ別途インストールし、インストール先のフォルダ名を指定することなく実行できる (パスを通す) 必要があります。

* Graphviz ( https://www.graphviz.org/ ) : `dot` コマンドへのパスを OS に設定すること
* Gnuplot ( http://www.gnuplot.info/ ) : `gnuplot` コマンドへのパスを OS に設定すること
* Blockdiag ( http://blockdiag.com/ ) : `blockdiag` コマンドへのパスを OS に設定すること。PDF を生成する場合は ReportLab もインストールすること
* aafigure ( https://launchpad.net/aafigure ) : `aafigure` コマンドへのパスを OS に設定すること
* PlantUML ( http://plantuml.com/ ) : `java` コマンドへのパスを OS に設定し、`plantuml.jar` が作業フォルダ、または `/usr/share/plantuml` あるいは `/usr/share/java` フォルダにあること

## 表

表は `//table[識別子][キャプション]{ 〜 //}` という記法です。ヘッダと内容を分ける罫線は「`------------`」（12個以上の連続する `-` または `=`）を使います。

表の各列のセル間は「1つ」のタブで区切ります。空白のセルには「`.`」と書きます。セルの先頭の「`.`」は削除されるので、先頭文字が「`.`」の場合は「`.`」をもう1つ余計に付けてください。たとえば「`.`」という内容のセルは「`..`」と書きます。

例:

```
//table[envvars][重要な環境変数]{
名前            意味
-------------------------------------------------------------
PATH            コマンドの存在するディレクトリ
TERM            使っている端末の種類。linux・kterm・vt100など
LANG            ユーザのデフォルトロケール。日本語ならja_JP.eucJPやja_JP.utf8
LOGNAME         ユーザのログイン名
TEMP            一時ファイルを置くディレクトリ。/tmpなど
PAGER           manなどで起動するテキスト閲覧プログラム。lessなど
EDITOR          デフォルトエディタ。viやemacsなど
MANPATH         manのソースを置いているディレクトリ
DISPLAY         X Window Systemのデフォルトディスプレイ
//}
```

本文中で「表 X.X を見てください」のように表を指定する場合はインライン命令 `@<table>` を使います。たとえば `@<table>{envvars}` となります。

表のセル内でもインライン命令は有効です。

「採番なし、キャプションなし」の表は、`//table` ブロック命令に引数を付けずに記述します。

```
//table{
〜
//}
```

「採番なし、キャプションあり」の表を作りたいときには、`//emtable` ブロック命令を利用します。

```
//emtable[キャプション]{
〜
//}
```

### 表の列幅

LaTeX および IDGXML のビルダを利用する場合、表の各列の幅を `//tsize` ブロック命令で指定できます。

```
//tsize[|ビルダ|1列目の幅,2列目の幅,……]
```

* 列の幅は mm 単位で指定します。
* IDGXML の場合、3列のうち1列目だけ指定したとすると、省略した残りの2列目・3列目は紙面版面の幅の残りを等分した幅になります。1列目と3列目だけを指定する、といった指定方法はできません。
* LaTeX の場合、すべての列について漏れなく幅を指定する必要があります。
* LaTeX の場合、「`//tsize[|latex||p{20mm}cr|]`」のように LaTeX の table マクロの列情報パラメータを直接指定することもできます。
* その他のビルダ (HTML など) においては、この命令は単に無視されます。

### 複雑な表

現時点では表のセルの結合や、中央寄せ・右寄せなどの表現はできません。

複雑な表については、画像を貼り込む `imgtable` ブロック命令を代わりに使用する方法もあります。`imgtable` の表は通常の表と同じく採番され、インライン命令 `@<table>` で参照できます。

例:

```
//imgtable[complexmatrix][複雑な表]{
complexmatrixという識別子に基づく画像ファイルが貼り込まれる。
探索ルールはimageと同じ
//}
```

## 引用

引用は「`//quote{ 〜 //}`」を使って記述します。

例:

```
//quote{
百聞は一見に如かず。
//}
```

複数の段落を入れる場合は、空行で区切ります。

## 囲み記事

技術書でよくある、コラムにするほどではないけれども本文から独立したちょっとした記事を入れるために、以下の命令があります。

* `//note[キャプション]{ 〜 //}` : ノート
* `//memo[キャプション]{ 〜 //}` : メモ
* `//tip[キャプション]{ 〜 //}` : Tips
* `//info[キャプション]{ 〜 //}` : 情報
* `//warning[キャプション]{ 〜 //}` : 注意
* `//important[キャプション]{ 〜 //}` : 重要
* `//caution[キャプション]{ 〜 //}` : 警告
* `//notice[キャプション]{ 〜 //}` : 注意

いずれも `[キャプション]` は省略できます。

内容には、空行で区切って複数の段落を記述可能です。

Re:VIEW 5.0 以降では、囲み記事に箇条書きや図表・リストを含めることもできます。

```
//note{

箇条書きを含むノートです。

 1. 箇条書き1
 2. 箇条書き2

//}
```

## 脚注

脚注は「`//footnote`」を使って記述します。

例:

```
パッケージは本書のサポートサイトから入手できます@<fn>{site}。
各自ダウンロードしてインストールしておいてください。

//footnote[site][本書のサポートサイト： http://i.loveruby.net/ja/stdcompiler ]
```

本文中のインライン命令「`@<fn>{site}`」は脚注番号に置換され、「本書のサポートサイト……」という文は実際の脚注に変換されます。

注意: TeX PDF において、コラムの中で脚注を利用する場合、`//footnote` 行はコラムの終わり（`==[/column]` など）の後ろに記述することをお勧めします。Re:VIEW の標準提供のコラム表現では問題ありませんが、サードパーティのコラムの実装によってはおかしな採番表現になることがあります。

### footnotetext オプション
TeX PDF において、コラム以外の `//note` などの囲み記事の中で「`@<fn>{～}`」を使うには、`footnotetext` オプションを使う必要があります。

`footnotetext` オプションを使うには、`config.yml` ファイルに`footnotetext: true` を追加します。

ただし、通常の脚注（footnote）ではなく、footnotemark と footnotetext を使うため、本文と脚注が別ページに分かれる可能性があるなど、いろいろな制約があります。また、採番が別々になるため、footnote と footnotemark/footnotetext を両立させることはできません。

## 後注

後注（最後にまとめて出力される注釈）は、「`//endnote`」を使って記述します。

```
パッケージは本書のサポートサイトから入手できます@<endnote>{site}。
各自ダウンロードしてインストールしておいてください。

//endnote[site][本書のサポートサイト： http://i.loveruby.net/ja/stdcompiler ]
```

本文中のインライン命令「`@<endnote>{site}`」は後注番号に置換され、「本書のサポートサイト……」という文は後注として内部に保存されます。

保存されている後注を書き出すには、書き出したい箇所（通常は章の末尾）に「`//printendnotes`」を置きます。

```
 …

==== 注釈

//printendnotes
```

後注の管理は章 (re ファイル) 単位であり、複数の章にまたがった後注を作ることはできません。

## 参考文献の定義

参考文献は同一ディレクトリ内の `bib.re` ファイルに定義します。

```
//bibpaper[cite][キャプション]{…コメント…}
```

コメントは省略できます。

```
//bibpaper[cite][キャプション]
```

例:

```
//bibpaper[lins][Lins, 1991]{
Refael D. Lins. A shared memory architecture for parallel study of
algorithums for cyclic reference_counting. Technical Report 92,
Computing Laboratory, The University of Kent at Canterbury , August
1991
//}
```

本文中で参考文献を参照したい場合は、インライン命令 `@<bib>` を使い、次のようにします。

例:

```
…という研究が知られています（@<bib>{lins}）。
```

## リード文

リード文は `//lead{ 〜 //}` で指定します。歴史的経緯により、`//read{ 〜 //}` も使用可能です。

例:

```
//lead{
本章ではまずこの本の概要について話し、
次にLinuxでプログラムを作る方法を説明していきます。
//}
```

空行区切りで複数の段落を記述することもできます。

## TeX 式

LaTeX の式を挿入するには、`//texequation{ 〜 //}` を使います。

例:

```
//texequation{
\sum_{i=1}^nf_n(x)
//}
```

「式1.1」のように連番を付けたいときには、識別子とキャプションを指定します。

```
//texequation[emc][質量とエネルギーの等価性]{
\sum_{i=1}^nf_n(x)
//}
```

参照するにはインライン命令 `@<eq>` を使います（たとえば `@<eq>{emc}`）。

インライン命令では `@<m>{〜}` を使います。インライン命令の式中に「}」を含む場合、`\}` とエスケープする必要があることに注意してください（`{` はエスケープ不要）。「インライン命令のフェンス記法」も参照してください。

LaTeX の数式が正常に整形されるかどうかは処理系に依存します。LaTeX を利用する PDFMaker では問題なく利用できます。

EPUBMaker および WEBMaker では、MathML に変換する方法、MathJax に変換する方法、画像化する方法から選べます。

### MathML の場合
MathML ライブラリをインストールしておきます（`gem install math_ml`）。

さらに config.yml に以下のように指定します。

```
math_format: mathml
```

なお、MathML で正常に表現されるかどうかは、ビューアやブラウザに依存します。

### MathJax の場合
config.yml に以下のように指定します。

```
math_format: mathjax
```

MathJax の JavaScript モジュールはインターネットから読み込まれます。現時点で EPUB の仕様では外部からの読み込みを禁止しているため、MathJax を有効にすると EPUB ファイルの検証を通りません。また、ほぼすべての EPUB リーダーで MathJax は動作しません。CSS 組版との組み合わせでは利用できる可能性があります。

### 画像化の場合

LaTeX を内部で呼び出し、外部ツールを使って画像化する方法です。画像化された数式は、`images/_review_math` フォルダに配置されます。

TeXLive などの LaTeX 環境が必要です。必要に応じて config.yml の `texcommand`、`texoptions`、`dvicommand`、`dvioptions` のパラメータを調整します。

さらに、画像化するための外部ツールも用意します。現在、以下の2つのやり方をサポートしています。

- `pdfcrop`：TeXLive に収録されている `pdfcrop` コマンドを使用して数式部分を切り出し、さらに PDF から画像化します。デフォルトでは画像化には Poppler ライブラリに収録されている `pdftocairo` コマンドを使用します（コマンドラインで利用可能であれば、別のツールに変更することもできます）。
- `dvipng`：[dvipng](https://ctan.org/pkg/dvipng) を使用します。OS のパッケージまたは `tlmgr install dvipng` でインストールできます。数式中に日本語は使えません。

config.yml で以下のように設定すると、

```
math_format: imgmath
```

デフォルト値として以下が使われます。

```
imgmath_options:
  # 使用する画像拡張子。通常は「png」か「svg」（svgの場合は、pdfcrop_pixelize_cmdの-pngも-svgにする）
  format: png
  # 変換手法。pdfcrop または dvipng
  converter: pdfcrop
  # プリアンブルの内容を上書きするファイルを指定する（デフォルトはupLaTeX+jsarticle.clsを前提とした、lib/review/makerhelper.rbのdefault_imgmath_preambleメソッドの内容）
  preamble_file: null
  # 基準のフォントサイズ
  fontsize: 10
  # 基準の行間
  lineheight: 12
  # pdfcropコマンドのコマンドライン。プレースホルダは
  # %i: 入力ファイル、%o: 出力ファイル
  pdfcrop_cmd: "pdfcrop --hires %i %o"
  # PDFから画像化するコマンドのコマンドライン。プレースホルダは
  # %i: 入力ファイル、%o: 出力ファイル、%O: 出力ファイルから拡張子を除いたもの
  # %p: 対象ページ番号、%t: フォーマット
  pdfcrop_pixelize_cmd: "pdftocairo -%t -r 90 -f %p -l %p -singlefile %i %O"
  # pdfcrop_pixelize_cmdが複数ページの処理に対応していない場合に単ページ化するか
  extract_singlepage: null
  # 単ページ化するコマンドのコマンドライン
  pdfextract_cmd: "pdfjam -q --outfile %o %i %p"
  # dvipngコマンドのコマンドライン
  dvipng_cmd: "dvipng -T tight -z 9 -p %p -l %p -o %o %i"
```

たとえば SVG を利用するには、次のようにします。

```
math_format: imgmath
imgmath_options:
  format: svg
  pdfcrop_pixelize_cmd: "pdftocairo -%t -r 90 -f %p -l %p %i %o"
```

デフォルトでは、pdfcrop_pixelize_cmd に指定するコマンドは、1ページあたり1数式からなる複数ページの PDF のファイル名を `%i` プレースホルダで受け取り、`%p` プレースホルダのページ数に基づいて `%o`（拡張子あり）または `%O`（拡張子なし）の画像ファイルに書き出す、という仕組みになっています。

単一のページの処理を前提とする `sips` コマンドや `magick` コマンドを使う場合、入力 PDF から指定のページを抽出するように `extract_singlepage: true` として挙動を変更します。単一ページの抽出はデフォルトで TeXLive の `pdfjam` コマンドが使われます。

```
math_format: imgmath
imgmath_options:
  extract_singlepage: true
  # pdfjamの代わりに外部ツールのpdftkを使う場合（Windowsなど）
  pdfextract_cmd: "pdftk A=%i cat A%p output %o"
  # ImageMagickを利用する例
  pdfcrop_pixelize_cmd: "magick -density 200x200 %i %o"
  # sipsを利用する例
  pdfcrop_pixelize_cmd: "sips -s format png --out %o %i"
```

textmaker 向けに PDF 形式の数式ファイルを作成したいときには、たとえば以下のように設定します（ページの抽出には pdftk を利用）。

```
math_format: imgmath
imgmath_options:
  format: pdf
  extract_singlepage: true
  pdfextract_cmd: "pdftk A=%i cat A%p output %o"
  pdfcrop_pixelize_cmd: "mv %i %o"
```

Re:VIEW 2 以前の dvipng の設定に合わせるには、次のようにします。

```
math_format: imgmath
imgmath_options:
  converter: dvipng
  fontsize: 12
  lineheight: 14.3
```

## 字下げの制御

段落の行頭字下げを制御するタグとして、`//noindent` があります。HTML では `noindent` が `class` 属性に設定されます。

## 空行

1行ぶんの空行を明示して入れるには、`//blankline` を使います。

例:

```
この下に1行の空行が入る

//blankline

この下に2行の空行が入る

//blankline
//blankline
```

## 見出し参照
章に対する参照は、次の3つのインライン命令を利用できます。章 ID は、各章のファイル名から拡張子を除いたものです。たとえば `advanced.re` であれば `advanced` が章 ID です。

* `@<chap>{章ID}` : 「第17章」のような、章番号を含むテキストに置換されます。
* `@<title>{章ID}` : その章の章題に置換されます。
* `@<chapref>{章ID}` : 『第17章「さらに進んだ話題」』のように、章番号とタイトルを含むテキストに置換されます。

節や項といったより下位の見出しを参照するには、`@<hd>` インライン命令を利用します。見出しの階層を「`|`」で区切って指定します。

例:

```
@<hd>{はじめに|まずは}
```

他の章を参照したい場合は、先頭に章 ID を指定してください。

例:

```
@<hd>{preface|はじめに|まずは}
```

参照先にラベルが設定されている場合は、見出しの代わりに、ラベルで参照します。複雑な階層をとるときにはラベルを使うことを推奨します。

```
=={hajimeni} はじめに
…
=== まずは
…
@<hd>{hajimeni|まずは}
```

* `@<hd>{見出しまたはラベル}` あるいは `@<secref>{見出しまたはラベル}` : 『「1.1 まずは」』のように、節項番号とタイトルを含むテキストに置換されます。
* `@<sec>{見出しまたはラベル}` : 「1.1」のような節項番号に置換されます。番号が付かない箇所の場合はエラーになります。
* `@<sectitle>{見出しまたはラベル}` : 「まずは」のように、節項のタイトル部に置換されます。

### コラム見出し参照

コラムの見出しの参照は、インライン命令 `@<column>` を使います。

例:

```
@<column>{Re:VIEWの用途いろいろ}
```

ラベルでの参照も可能です。

```
==[column]{review-application} Re:VIEWの応用
…
@<column>{review-application}
```

## リンク

Web ハイパーリンクを記述するには、リンクに `@<href>`、アンカーに `//label` を使います。リンクの書式は `@<href>{URL, 文字表現}` で、「`, 文字表現`」を省略すると URL がそのまま使われます。URL 中に `,` を使いたいときには、`\,` とエスケープしてください。

例:

```
@<href>{http://github.com/, GitHub}
@<href>{http://www.google.com/}
@<href>{#point1, ドキュメント内ポイント}
@<href>{chap1.html#point1, ドキュメント内ポイント}
//label[point1]
```

## 単語ファイルの展開

キーと値のペアを持つ単語ファイルを用意しておき、キーが指定されたら対応するその値を展開するようにできます。`@<w>{キー}` あるいは `@<wb>{キー}` インライン命令を使います。後者は太字にします。

現在、単語ファイルは CSV（コンマ区切り）形式で拡張子 .csv のものに限定されます。1列目にキー、2列目に値を指定して列挙します。

```
"LGPL","Lesser General Public License"
"i18n","""i""nternationalizatio""n"""
```

単語ファイルのファイルパスは、`config.yml` に `words_file: ファイルパス` で指定します。`word_file: ["common.csv", "mybook.csv"]` のように複数のファイルも指定可能です（同一のキーがあるときには後に指定したファイルの値が優先されます）。

例:

```review
@<w>{LGPL}, @<wb>{i18n}
```

テキストビルダを使用している場合:

```
Lesser General Public License, ★"i"nternationalizatio"n"☆
```

展開されるときには、ビルダごとに決められたエスケープが行われます。インライン命令を含めるといったことはできません。

## コメント

最終結果に出力されないコメントを記述するには、「`#@#`」を使います。行末までがコメントとして無視されます。

例:

```
#@# FIXME: あとで調べておくこと
```

最終結果に出力するコメントを記述したい場合は、`//comment` または `@<comment>` を使ったうえで、review-compile コマンドに `--draft` オプションを付けて実行します。

例:

```
@<comment>{あとで書く}
```

## 生データ行

Re:VIEW のタグ範囲を超えて何か特別な行を挿入したい場合、`//raw` ブロック命令や`//embed`ブロック命令、または `@<raw>` インライン命令や `@<embed>` インライン命令を使います。

### `//raw`行

例:

```
//raw[|html|<div class="special">\nここは特別な行です。\n</div>]
```

ブロック命令は1つだけオプションをとり、「|ビルダ名|そのまま出力させる内容」という書式です。`\n`は改行文字に変換されます。

ビルダ名には「`html`」「`latex`」「`idgxml`」「`markdown`」「`top`」のいずれかが入ります。ビルダ名は「,」で区切って複数指定することも可能です。該当のビルダを使用しているときのみ、内容が出力されます。

例:

```
（HTMLビルダの場合:）
<div class="special">
ここは特別な行です。
</div>

（ほかのビルダの場合は単に無視されて何も出力されない）
```

インライン命令は、`@<raw>{|ビルダ名|〜}` という書式で、記述はブロック命令に同じです。

### `//embed`ブロック

例:

```
//embed{
<div class="special">
ここは特別なブロックとして扱われます。
</div>
//}

//embed[html,markdown]{
<div class="special">
ここはHTMLとMarkdownでは特別なブロックとして扱われます。
</div>
//}
```

`//embed`ブロック命令はブロック内の文字列をそのまま文書中に埋め込みます。エスケープされる文字はありません。

オプションとして、ビルダ名を指定できます。ビルダ名には「`html`」「`latex`」「`idgxml`」「`markdown`」「`top`」のいずれかが入ります。ビルダ名は「,」で区切って複数指定することも可能です。該当のビルダを使用しているときのみ、内容が出力されます。異なるビルダを使用している場合は無視されます。

オプションを省略した場合、すべてのビルダで文字列が埋め込まれます。

例:

HTMLビルダを使用している場合:

```html
<div class="special">
ここは特別なブロックとして扱われます。
</div>

<div class="special">
ここはHTMLとMarkdownでは特別なブロックとして扱われます。
</div>
```

LaTeXビルダを使用している場合:

```tex
<div class="special">
ここは特別なブロックとして扱われます
</div>

```

`//raw`、`//embed`、`@<raw>` および `@<embed>` は、HTML、XML や TeX の文書構造を容易に壊す可能性があります。使用には十分に注意してください。

### 入れ子の箇条書き

Re:VIEW の箇条書きは `*` 型の箇条書きを除き、基本的に入れ子を表現できません。いずれの箇条書きも、別の箇条書き、あるいは図表・リストを箇条書きの途中に配置することを許容していません。

この対策として、Re:VIEW 4.2 では試験的に `//beginchild`、`//endchild` というブロック命令を追加しています。箇条書きの途中に何かを含めたいときには、それを `//beginchild` 〜 `//endchild` で囲んで配置します。多重に入れ子にすることも可能です。

```
 * UL1

//beginchild
#@# ここからUL1の子

 1. UL1-OL1

//beginchild
#@# ここからUL1-OL1の子

UL1-OL1-PARAGRAPH

 * UL1-OL1-UL1
 * UL1-OL1-UL2

//endchild
#@# ここまでUL1-OL1の子

 2. UL1-OL2

 : UL1-DL1
        UL1-DD1
 : UL1-DL2
        UL1-DD2

//endchild
#@# ここまでUL1の子

 * UL2
```

これをたとえば HTML に変換すると、次のようになります。

```
<ul>
<li>UL1
<ol>
<li>UL1-OL1
<p>UL1-OL1-PARAGRAPH</p>
<ul>
<li>UL1-OL1-UL1</li>
<li>UL1-OL1-UL2</li>
</ul>
</li>

<li>UL1-OL2</li>
</ol>
<dl>
<dt>UL1-DL1</dt>
<dd>UL1-DD1</dd>
<dt>UL1-DL2</dt>
<dd>UL1-DD2</dd>
</dl>
</li>

<li>UL2</li>
</ul>
```

（試験実装のため、命令名や挙動は今後のバージョンで変更になる可能性があります。）

## インライン命令
主なインライン命令を次に示します。

### 書体
書体については、適用するスタイルシートなどによって異なることがあります。

* `@<kw>{〜}`, `@<kw>{キーワード, 補足}` : キーワード。通常は太字になることを想定しています。2つめの記法では、たとえば `@<kw>{信任状, credential}` と表記したら「信任状（credential）」のようになります。
* `@<bou>{〜}` : 傍点が付きます。
* `@<ami>{〜}` : 文字に対して網がかかります。
* `@<u>{〜}` : 下線を引きます。
* `@<b>{〜}` : 太字にします。
* `@<i>{〜}` : イタリックにします。和文の場合、処理系によってはイタリックがかからないこともあります。
* `@<strong>{〜}` : 強調（太字）にします。
* `@<em>{〜}` : 強調にします。
* `@<tt>{〜}` : 等幅にします。
* `@<tti>{〜}` : 等幅＋イタリックにします。
* `@<ttb>{〜}` : 等幅＋太字にします。
* `@<code>{〜}` : 等幅にします（コードの引用という性質）。
* `@<tcy>{〜}` : 縦書きの文書において文字を縦中横にします。
* `@<ins>{〜}` : 挿入箇所を明示します（デフォルトでは下線が引かれます）。
* `@<del>{〜}` : 削除箇所を明示します（デフォルトでは打ち消し線が引かれます）。

### 参照
* `@<chap>{章ファイル名}` : 「第17章」のような、章番号を含むテキストに置換されます。
* `@<title>{章ファイル名}` : その章の章題に置換されます。
* `@<chapref>{章ファイル名}` : 『第17章「さらに進んだ話題」』のように、章番号とタイトルを含むテキストに置換されます。
* `@<list>{識別子}` : リストを参照します。
* `@<img>{識別子}` : 図を参照します。
* `@<table>{識別子}` : 表を参照します。
* `@<eq>{識別子}` : 式を参照します。
* `@<hd>{ラベルまたは見出し}` : 節や項を参照します。
* `@<column>{ラベルまたは見出し}` : コラムを参照します。

### その他
* `@<ruby>{親文字,ルビ}` : ルビを振ります。たとえば `@<ruby>{愕然,がくぜん}` のように表記します。
* `@<br>{}` : 段落途中で改行します。濫用は避けたいところですが、表のセル内や箇条書き内などで必要になることもあります。
* `@<uchar>{番号}` : Unicode文字を出力します。引数は16進数で指定します。
* `@<href>{URL}`, `@<href>{URL, 文字表現}` : ハイパーリンクを作成します（後述）。
* `@<icon>{識別子}` : インラインの画像を出力します。
* `@<m>{数式}` : インラインの数式を出力します。
* `@<w>{キー}` : キー文字列に対応する、単語ファイル内の値文字列を展開します。
* `@<wb>{キー}` : キー文字列に対応する、単語ファイル内の値文字列を展開し、太字にします。
* `@<raw>{|ビルダ|〜}` : 生の文字列を出力します。`\}`は`}`に、`\\`は`\`に、`\n`は改行に置き換えられます。
* `@<embed>{|ビルダ|〜}` : 生の文字列を埋め込みます。`\}`は`}`に、`\\`は`\`に置き換えられます（`\n`はそのままです）。
* `@<idx>{文字列}` : 文字列を出力するとともに、索引として登録します。索引の使い方については、makeindex.ja.md を参照してください。
* `@<hidx>{文字列}` : 索引として登録します (idx と異なり、紙面内に出力はしません)。`親索引文字列<<>>子索引文字列` のように親子関係にある索引も定義できます。
* `@<balloon>{〜}` : コードブロック (emlist など) 内などでのいわゆる吹き出しを作成します。たとえば「`@<balloon>{ABC}`」とすると、「`←ABC`」となります。デフォルトの挙動および表現は簡素なので、より装飾されたものにするにはスタイルシートを改変するか、`review-ext.rb` を使って挙動を書き換える必要があります。

## 著者用タグ（プリプロセッサ命令）

これまでに説明したタグはすべて最終段階まで残り、見た目に影響を与えます。それに対して以下のタグは著者が使うための専用タグであり、変換結果からは除去されます。

* `#@#` : コメント。この行には何を書いても無視されます。
* `#@warn(〜)` : 警告メッセージ。プリプロセス時にメッセージが出力されます。
* `#@require`, `#@provide` : キーワードの依存関係を宣言します。
* `#@mapfile(ファイル名) 〜 #@end` : ファイルの内容をその場に展開します。
* `#@maprange(ファイル名, 範囲名) 〜 #@end` : ファイル内の範囲をその場に展開します。
* `#@mapoutput(コマンド) 〜 #@end` : コマンドを実行して、その出力結果を展開します。

コメントを除き、プリプロセッサ `review-preproc` コマンドとの併用を前提とします。

## 国際化（i18n）

Re:VIEW が出力する文字列(「第◯章」「図」「表」など）を、指定した言語に合わせて出力することができます。デフォルトは日本語です。

ファイルが置かれているディレクトリに `locale.yml` というファイルを用意して、以下のように記述します（日本語の場合）。

例:

```
locale: ja
```

既存の表記を書き換えたい場合は、該当する項目を上書きします。既存の設定ファイルは Re:VIEW の `lib/review/i18n.yml` にあります。

例:

```
locale: ja
list: 実行例
```

### Re:VIEW カスタムフォーマット

`locale.yml` ファイルでは、章番号などに以下の Re:VIEW カスタムフォーマットを使用可能です。

* `%pA` : アルファベット（大文字）A, B, C, ...
* `%pa` : アルファベット（小文字）a, b, c, ...
* `%pAW` : アルファベット（大文字・いわゆる全角）Ａ, Ｂ, Ｃ, ...
* `%paW` : アルファベット（小文字・いわゆる全角）ａ, ｂ, ｃ, ...
* `%pR` : ローマ数字（大文字）I, II, III, ...
* `%pr` : ローマ数字（小文字）i, ii, iii, ...
* `%pRW` : ローマ数字（大文字・単一文字表記）Ⅰ, Ⅱ, Ⅲ, ...
* `%pJ` : 漢数字 一, 二, 三, ...
* `%pdW` : アラビア数字（0〜9まではいわゆる全角、10以降半角）１, ２, ... 10, ...
* `%pDW` : アラビア数字（すべて全角）１, ２, ... １０, ...

例:

```
locale: ja
  part: 第%pRW部
  appendix: 付録%pA
```

## その他の文法

拡張文法は `review-ext.rb` というファイルで指定できます。

たとえば、

```ruby
# review-ext.rb
ReVIEW::Compiler.defblock :foo, 0..1
class ReVIEW::HTMLBuilder
  def foo(lines, caption = nil)
    puts lines.join(",")
  end
end
```

のような内容のファイルを用意すると、以下のような文法を追加できます。

```
//foo{
A
B
C
//}
```

```
# 出力結果
A,B,C
```

詳しいことについては、ここでは触れません。

## HTML および LaTeX のレイアウト機能

ファイルが置かれているディレクトリに layouts/layout.html.erb を置くと、デフォルトの HTML テンプレートの代わりにその HTML が使われます（erb 記法で記述します）。

例:

```
<html>
  <head>
    <title><%= @config["booktitle"] %></title>
  </head>
  <body>
    <%= @body %>
    <hr/>
  </body>
</html>
```

同様に、layouts/layout.tex.erb で、デフォルトの LaTeX テンプレートを置き換えることができます。
