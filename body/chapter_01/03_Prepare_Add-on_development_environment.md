# 3. アドオン開発環境を整えよう

ここではアドオンを効率良く開発するための環境を紹介します。

## Blenderの日本語化

Blenderで日本語化が可能なところは日本語化することで、英語が原因で開発が遅くなることを避けることができます。
[2節](02_Use_Blender_Add-on.md)を参考にBlenderを日本語化しましょう。

以降の説明ではBlender本体が日本語化されていることを前提に解説します。

## Blenderの画面設定

Blenderをダウンロードした直後のBlenderの起動画面は、以下のような画面になっていると思います。

![Blender 初期状態](https://dl.dropboxusercontent.com/s/jj7knj6wpu29mrd/blender_initial.png "Blender 初期状態")

このままでもアドオン開発は可能ですが、以下のようにアドオンを開発しやすい環境にすることをお勧めします。

![Blender アドオン開発向け環境](https://dl.dropboxusercontent.com/s/9ws6g0tr3xhpc94/blender_change_window_4.png "Blender アドオン開発向け環境")

なおここで示している設定は、あくまで筆者の好みがそのまま反映されているため、
各自作業しやすい環境にしていただいて構いません。

### コンソール・ウィンドウの表示

Blenderにはデバッグ用に **コンソール・ウィンドウ** と呼ばれる機能が備わっています。
コンソール・ウィンドウとは、アドオンの実行結果やエラーを表示するためのもので、起動直後は非表示になっています。
コンソール・ウィンドウを表示させるためには、メニューバーの下を下に向かってドラッグ＆ドロップする必要があります。

![コンソール・ウィンドウの表示 手順1](https://dl.dropboxusercontent.com/s/ho9x3vdwrfp1bqr/blender_show_console_window_1.png "コンソール・ウィンドウの表示 手順1")

アドオンを開発する際に大変お世話になる機能ですので、アドオン開発中は常にコンソール・ウィンドウを
表示させておきましょう。

![コンソール・ウィンドウの表示 手順2](https://dl.dropboxusercontent.com/s/49km722w99jxygf/blender_show_console_window_2.png "コンソール・ウィンドウの表示 手順2")

### ウィンドウの分割

過去にBlenderを使ったことのある人であればご存知かと思いますが、Blenderはアプリケーション内で
複数のウィンドウに分割することができます。
この機能を用いて、以下のようにウィンドウを分割してみましょう。

![ウィンドウの分割 手順1](https://dl.dropboxusercontent.com/s/hnc8c8qfonfnnyp/blender_divide_window_1.png "ウィンドウの分割 手順1")

![ウィンドウの分割 手順2](https://dl.dropboxusercontent.com/s/g6ifc1mn5wu120e/blender_divide_window_2.png "ウィンドウの分割 手順2")

![ウィンドウの分割 手順3](https://dl.dropboxusercontent.com/s/i3bbl8f5vbmazhk/blender_divide_window_3.png "ウィンドウの分割 手順3")

分割したウィンドウの表示は全て **3Dビュー** になっていると思います。

### ウィンドウ表示の変更

ウィンドウの分割が完了したら、各ウィンドウの表示を変更しましょう。
ウィンドウの表示の変更は、各ウィンドウのメニューバーの一番左のボタンから行うことができます。
最初に左下のウィンドウ表示を3Dビューから **テキストエディター** に変更しましょう。

![ウィンドウ表示の変更 手順1](https://dl.dropboxusercontent.com/s/v56yihqny5qy83q/blender_change_window_1.png "ウィンドウ表示の変更 手順1")

![ウィンドウ表示の変更 手順2](https://dl.dropboxusercontent.com/s/9edhgrh27ulak4p/blender_change_window_2.png "ウィンドウ表示の変更 手順2")

続いて、左上のウィンドウ表示を3Dビューから **Pythonコンソール** に変更しましょう。

![ウィンドウ表示の変更 手順3](https://dl.dropboxusercontent.com/s/owvn6git978ja7i/blender_change_window_3.png "ウィンドウ表示の変更 手順3")

![ウィンドウ表示の変更 手順4](https://dl.dropboxusercontent.com/s/9ws6g0tr3xhpc94/blender_change_window_4.png "ウィンドウ表示の変更 手順4")

### Blenderの初期状態にする

他にも不要なウィンドウを削除したり、ウィンドウの配置を変更したりして各自使いやすいように改造していきましょう。
これでBlenderのウィンドウ設定は完了しましたが、このままBlenderを閉じてしまうと次にBlenderを起動した時に初期状態に戻ってしまいます。
そこで、Blenderが起動した時は常にこの状態になるようにしましょう。

Blenderが起動した時に設定した状態にするためには、 **情報** ウィンドウの **ファイル** > **スタートアップファイルを保存** を実行します。

![Blenderの初期状態にする 手順1](https://dl.dropboxusercontent.com/s/kbro7t4evkim2au/blender_save_startup_file_1.png "Blenderの初期状態にする 手順1")

確認メッセージが出るので、 **スタートアップファイルを保存** をクリックします。

![Blenderの初期状態にする 手順2](https://dl.dropboxusercontent.com/s/pm74e5k1atjgu0a/blender_save_startup_file_2.png "Blenderの初期状態にする 手順2")

Blenderの起動直後の状態を設定したい場合は、いつでもこの方法を使えるので覚えておきましょう。

Blenderをダウンロードした直後の初期状態に戻したい場合は、 **情報** ウィンドウの **ファイル** > **初期設定を読み込む** を実行してください。

![初期設定を読み込む 手順1](https://dl.dropboxusercontent.com/s/fzhbvpp60xf76a6/blender_read_factory_setting_1.png "初期状態を読み込む 手順1")

確認メッセージが出るので、 **初期設定を読み込む** をクリックします。

![初期設定を読み込む 手順2](https://dl.dropboxusercontent.com/s/sc2dvqqw19twg12/blender_read_factory_setting_2.png "初期状態を読み込む 手順2")

なお初期設定を読み込んだ後も **スタートアップファイルを保存** しない限り、Blenderの起動直後の状態は更新されないことに注意が必要です。

## テキストエディタ

アドオンの開発は基本的にテキスト（ソースコード）を編集する作業になるため、テキストを編集する環境が整っていれば良いです。
そのため、各OSに標準で付随しているテキスト編集ソフトを使って開発を始めることができます。
しかし、各OSに標準で付随しているテキスト編集ソフトは機能が少ないため、アドオンの開発には不向きです。
このためアドオン開発向けのエディタとして、Pythonで書かれたソースコードの編集が楽になるエディタを導入した方が良いでしょう。
ちなみに筆者は、アドオンの開発等のソースコード編集はVim、READMEなど文章を編集する時はAtomを利用しています。

### Blender付随のテキストエディタ

Blenderはアドオンの開発者や簡単なPythonスクリプトを実行したい人のために、Blender本体にテキストエディタが付随しています。
テキストエディタの起動の仕方は非常にシンプルで、各ウィンドウのメニューバーの一番左のボタンから、 **テキストエディター** を選ぶだけです。
前述したBlenderの画面設定を行った方は、左下のウィンドウが **テキストエディター** に対応します。

Blender付随のテキストエディタには、テキストエディタで開いているソースコードをそのまま実行できるという、
他のテキストエディタにはない利点があります。

### Vim

詳しく書くとEmacsとのエディタ論争に巻き込まれそうなので、簡単に紹介します。
VimはEmacsと共に、Unix/Linuxでよく利用されるエディタの1つで、*コマンドモード* と *入力モード* と呼ばれる2つのモード
を備えていることが特徴であるエディタです。
**gVim** と呼ばれるWindows版のVimも存在するため、一度Vimの使い方に慣れてしまえば、OSを変えてもエディタの使い方を再度勉強する必要がありません。
なお、有志によってVimを使いやすくするためのプラグインや記事がインターネット上にたくさんあるため、
使いにくい部分があったら検索してみると良いかもしれません。

### Emacs

Vimと共に、Unix/Linuxでよく利用されるエディタです。
こちらもWindows版が用意されているため、一度使い方に慣れてしまえば、OSを変えてもエディタの使い方を再度勉強する必要がありません。
Vimと同様ユーザが多いエディタのため、Emacsを使いやすくするためのプラグインや記事がインターネットにたくさんあります。

### その他のエディタ

上にあげたエディタ以外にも様々なエディタがありますので、自分にあったエディタを選ぶと良いでしょう。
他のエディタとしては例えば以下のようなものがあります。

* Atom *(Windows/Mac/Linux)*
* 秀丸 *(Windows)*
* Terapad *(Windows)*
* nano *(Mac/Linux)*
* SublimeText *(Windows/Mac/Linux)*

## アドオン開発時のBlenderの起動方法

Blenderを起動させる時は、アプリケーションのアイコン（Windowsなら ```blender.exe``` ）をダブルクリックして
起動することが多いと思います。
しかしアドオン開発時は、ソースコードが正常に動作しているか確認（**デバッグ**）しやすくするために、 **コンソールからBlenderを起動** することをお勧めします。
ここでは、コンソールからBlenderを起動する方法を紹介します。
本書では、コンソールからBlenderを起動することを前提として説明しますので、ここで紹介する手順を覚えてしまいましょう。

### Windowsの場合

1. **コマンドプロンプト** を起動します。
2. 以下のコマンドを実行します。（blender.exeが置かれているパスが ```C:\path\blender.exe``` であると仮定します。）
```
$ C:\path\blender.exe
```
3. Blenderが起動します。

### Macの場合

1. **ターミナル** を起動します。
2. 以下のコマンドを実行します。（Blender.appが置かれているパスが ```/path/Blender.app``` であると仮定します。）
```
$ /path/blender.app/Contents/MacOS/blender
```
3. Blenderが起動します。

### Linuxの場合

1. **ターミナル** を起動します。
2. 以下のコマンドを実行します。（実行ファイルblenderが置かれているパスが ```/path/blender``` であると仮定します。）
```
$ /path/blender
```
3. Blenderが起動します。

## まとめ

### ポイント