#mruby4VS

mruby for Visual Studio

##mruby4VSとは？

mruby ( [https://github.com/mruby/mruby](https://github.com/mruby/mruby "mruby") )をVisual Studioで使うためのラッパーライブラリです。
静的(lib)と動的(dll)を用意する予定ですが、DLLは一部うまく動かないので残念仕様です。

mruby4VSはmruby v1.0.0 ( [http://forum.mruby.org/](http://forum.mruby.org/) )を利用しています。
最新版を使いたい場合はmrubyフォルダを最新のmrubyにして若干修正すればいいと思います。

##利用方法

###準備

####共通

+ mruby4vsを適当なところにコピー
+ VC++ディレクトリのインクルードディレクトリにmruby4vsのパスを追加する。
 + mruby4vs
+ 構成プロパティ > 全般 > 文字セットをマルチバイトにする。

####静的ライブラリ(lib)

+ VC++ディレクトリのライブラリディレクトリにlibファイルのある場所を追加する。
+ MRubyクラスのインスタンスを取得する。
 + `class MRuby *mruby = CreateMRuby();`

####動的ライブラリ(DLL)

+ includeでDLL用ヘッダを読み込む。
 + `#include "MRubyDLL.h"`
+ DLLを読み込む。
 + `HMODULE hmruby = LoadLibrary( "mrubydll.dll" );`
+ MRubyクラスのインスタンスを取得する。
 + `class MRuby* mruby = CreateMRuby( hmruby );`

###利用

生成したmrubyを使ってください。
大体メソッド名はmrubyの関数名に似ています。

##ビルド方法

###lib

MRubyLib.slnを開いて適当にビルドすればいい。

###DLL

MRubyDLL.slnを開いて適当にビルドすればいい。

###テスト

testmruby.slnを開いて適当にビルドすればいい。

testmruby.cppの先頭付近にある`//#define USEDLL`のコメントアウトを外すとDLLを利用するサンプルになります。

また、DLLでは`loadFile(FILE*)`が失敗しますが、`load(const char *)`にファイル名を与えればファイルから読み込むことができます。
ファイルからの読み込みにはmruby4VSの`load()`を使ってください。

##その他

###開発環境

+ Visual Studio 2013

###今後について

mrubyの一部関数しかラップできてないので、このまま作業を続けてmruby 1.0.0を網羅できるまでは最低限頑張る予定。

また、こちらのバージョンは今のところmrubyの最新版は利用せず、新しい安定版が出たら追いかけたい。

###Spinelについて

Spinelは基本的にmrubyの関数をラップしただけのクラスです。
基本的に関数は命名規則を変更し、スネークケースからキャメルケースにしています。
`mrb_state`はクラス内で保持し、常に指定しないで使うことができます。

また、DLLにした際にヘッダファイルに更新がない限りはDLLの上書きのみで対応できるよう、インタフェースとしてMRubyクラスを用意し、継承しています。

###DLLのloadFile(FILE *)について

謎のバグにより落ちます。
`load(const char *)`や`loadString(const char *)`は動きます。

###可変長引数を用いたfuncallについて

実装見れば分かりますが、相当な力技です。
mrubyに手を入れないことを重視したため、ものすごい力技です。
`MRB_FUNCALL_ARGC_MAX`の値を変更した際は気をつけてください。
