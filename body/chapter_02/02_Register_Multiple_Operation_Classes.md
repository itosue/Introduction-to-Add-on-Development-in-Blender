<div id="sect_title_img_2_2"></div>

<div id="sect_title_text"></div>

# 複数のオペレータクラスを登録する

<div id="preface"></div>

###### 前節ではアドオンの基本的な部分について説明しました。本節では前回解説した内容を理解していることを前提に、より複雑なサンプルを用いて解説していきます。既に解説済みの部分は解説を省略しますので、不明な点がある場合は [2-1節](01_Basic_of_Add-on_Development.md) を再度確認することをお勧めします。

## 作成するアドオンの仕様

本節で作成するアドオンの仕様は、以下のようにします。

* *3Dビュー* エリアのメニューである *オブジェクト* に *選択オブジェクトの拡大* と *選択オブジェクトの縮小* を追加
* *オブジェクト* > *選択オブジェクトの拡大* を実行すると、選択中のオブジェクトの大きさが2倍になる
* *オブジェクト* > *選択オブジェクトの縮小* を実行すると、選択中のオブジェクトの大きさが1/2倍になる

## アドオンを作成する

以下のソースコードを、 [1-4節](../chapter_01/04_Install_own_Add-on.md)を参考にして *テキストエディター* エリアに入力し、 ```sample_2.py``` という名前で保存します。

[import](../../sample/src/chapter_02/sample_2.py)

## アドオンを使用する

作成したアドオンを使ってみます。

### アドオンを有効化する

[1-4節](../chapter_01/04_Install_own_Add-on.md) を参考に、作成したアドオンを有効化します。
アドオンが有効かされると、 *コンソール* に以下の文字列が出力されるはずです。

```sh
サンプル2: アドオン「サンプル2」が有効化されました。
```

<div id="sidebyside"></div>

|アドオン有効化後、右図のように *3Dビュー* エリアのメニューに *オブジェクト* > *選択オブジェクトの拡大* と、 *オブジェクト* > *選択オブジェクトの縮小* が追加されていることを確認します。|![メニューの追加確認](https://dl.dropboxusercontent.com/s/udxtkxqkbrbj4hz/blender_use_add-on_1.png "メニューの追加確認")|
|---|---|


### アドオンの機能を使用する

以下の手順に従い、作成したアドオンの機能を使ってみます。

<div id="process_title"></div>

##### Work

<div id="process"></div>

|<div id="box">1</div>|Blender起動直後に自動的に生成されている *Cube* を選択し、*3Dビュー* エリアのメニュー *オブジェクト* > *選択オブジェクトの拡大* を実行します。選択したオブジェクト *Cube* のサイズが2倍に拡大されます。|![選択オブジェクトの拡大](https://dl.dropboxusercontent.com/s/ll5jtxlmj5vek96/blender_use_add-on_2.png "選択オブジェクトの拡大")|
|---|---|---|

この時 *コンソール・ウィンドウ* を見ると、以下のメッセージが出力されます。

```sh
サンプル2: 「Cube」を2倍に拡大しました。
```

また、 *コンソール* にも以下のメッセージが出力されます。

```sh
サンプル2: オペレーション「OBJECT_OT_enlarge_object」が実行されました。
```

<div id="process_sep"></div>

---

<div id="process"></div>

|<div id="box">2</div>|*情報* エリアのメニュー *ファイル* > *新規* を実行し、 *スタートアップアフィルの再読み込み* を行った後、 *3Dビュー* エリアの *オブジェクト* > *選択オブジェクトの縮小* を実行します。選択中のオブジェクト *Cube* のサイズが1/2倍に縮小されます。|![選択オブジェクトの縮小](https://dl.dropboxusercontent.com/s/zinqogsgx4td6jw/blender_use_add-on_3.png "選択オブジェクトの縮小")|
|---|---|---|

この時、 *コンソール・ウィンドウ* には、以下のメッセージが出力されます。

```sh
サンプル2: 「Cube」を1/2倍に縮小しました。
```

また、 *コンソール* には以下のメッセージが出力されます。

```sh
サンプル2: オペレーション「OBJECT_OT_reduce_object」が実行されました。
```

<div id="process_start_end"></div>

---


### アドオンを無効化する

[1-4節](../chapter_01/04_Install_own_Add-on.md) を参考に、有効化したアドオンを無効化します。
アドオンを無効化すると、 *コンソール* に以下の文字列が出力されます。

```sh
サンプル2: アドオン「サンプル2」が無効化されました。
```

## ソースコードの解説

アドオンの動作を確認したところで、次にソースコードの解説をします。
前節で説明した内容から新しく追加された部分について説明します。

### 複数のオペレータクラスの定義

今回新たに解説する内容の1つとして、複数のオペレータクラスを登録する処理があります。
1つのファイルに複数のオペレータクラスを定義するには、作りたいオペレーションの数だけオペレーションクラスを作成する必要があります。
今回はオブジェクトの拡大と縮小のために、2つのオペーレーションクラスを以下のように作成しました。

```python
# オブジェクトを拡大するオペレーション
class EnlargeObject(bpy.types.Operator):

	bl_idname = "object.enlarge_object"
	bl_label = "選択オブジェクトの拡大"
	bl_description = "選択中のオブジェクトを拡大します"
	bl_options = {'REGISTER', 'UNDO'}

# ・・・(略)・・・


# オブジェクトを縮小するオペレーション
class ReduceObject(bpy.types.Operator):

	bl_idname = "object.reduce_object"
	bl_label = "選択オブジェクトの縮小"
	bl_description = "選択中のオブジェクトを縮小します"
	bl_options = {'REGISTER', 'UNDO'}

# ・・・(略)・・・
```

```bl_idname``` はオペレータクラスごとに固有の文字列である必要があるため、重複しないように気をつけてください。
重複しないようにするための方法は後ほど紹介します。

### コンソール・ウィンドウへの出力

オペレータクラスの ```execute()``` メソッドでは、メニューが実行された時にコンソール・ウィンドウへメッセージを出力する処理があります。
ここでは ```EnlargeObject``` クラスの ```execute()``` メソッドを用いて、コンソール・ウィンドウへメッセージを出力する方法を紹介します。

```python
    # メニューを実行した時に呼ばれるメソッド
    def execute(self, context):
        active_obj = context.active_object
        active_obj.scale = active_obj.scale * 2.0
        self.report(
            {'INFO'},
            "サンプル2: 「" + active_obj.name + "」を2倍に拡大しました。")
        print("サンプル2: オペレーション「" + self.bl_idname + "」が実行されました。")
```

```execute()``` メソッドに渡されてくる引数については、 [2-1節](01_Basic_of_Add-on_Development.md)で説明しました。

引数 ```context``` を利用することにより、現在のコンテキスト（実行状態）を取得することができます。
現在選択されているオブジェクトである ```context.active_object``` を ```active_obj``` に保存し、 ```active_obj.scale``` を2倍にすることで取得したオブジェクトのサイズを2倍に拡大することができます。

またユーザに対して、オペレーションを実行した後に拡大・縮小したことがわかるようなメッセージを *コンソール・ウィンドウ* に出力するために、 ```self.report()``` メソッドを用いています。
ここで ```execute()``` メソッドの引数である ```self``` は、オブジェクトのインスタンスです。

```self.report()``` メソッドに指定する必要がある引数は以下の通りです。

|引数|型|値の説明|
|---|---|---|
|第1引数|ディクショナリ型|出力メッセージの種類|
|第2引数|文字列|メッセージ本文|

表示したいメッセージの内容に応じてメッセージの種類を指定し、 *コンソール・ウィンドウ* へ出力することができます。
出力メッセージの種類には例えば以下のようなものがあります。

|種類|コンソール・<br>ウィンドウへの出力|情報ウィンドウの<br>メニューへの出力|利用場面|
|---|---|---|---|
|```INFO```|ハイライト表示（緑色）|通知メッセージ|オペレーションの成功通知やオペレーションにより変化した内容の通知など、参考程度にユーザへ通知したい情報|
|```WARNING```|ハイライト表示（橙色）|警告メッセージ|アドオンの実行条件に合わない時の通知など、ユーザに注意を促したい情報|
|```ERROR```|ハイライト表示（赤色）|エラーメッセージ|アドオンの実行に失敗した時の通知や本来起こらないはずのエラーが発生した時の通知など、ユーザが特に注意を払ってほしい情報|
|```OPERATOR```|表示|（なし）|エラー発生時に開発者へフィードバックするためのアドオンの内部情報|

今回のアドオンでは、オブジェクトが拡大・縮小されたことを知らせるために ```{'INFO'}``` を指定しています。
また ```active_obj.name``` で選択中のオブジェクト名を取得し、拡大・縮小したオブジェクト名も表示しています。

## Pythonコンソールを活用しよう

### bl_idname が重複していないか確認する

ソースコード解説の中で ```bl_idname``` が重複してはならないと書きました。しかし、 ```bl_idname``` が重複していないか確認するためにはどのようにしたら良いのでしょうか？
そんな時に役に立つのが、 *Pythonコンソール* です。

*Pythonコンソール* を利用することにより、例えば以下のようなことができます。

* 補完機能を使って、APIの正確な名称を調べることができる
* APIを実行することで、APIの効果を確認出来る
* 短いPythonのソースコードを実行し、簡単なテストができる
* アドオン作成時に命名した関数などが、APIや他のアドオンと重複しているかいないかを確認出来る

実際に *Pythonコンソール* を使い、今回作成したオペレータクラスの ```bl_idname``` が既存のAPIと重複していないかを確認してみます。
ここでは例として、 ```bl_idname``` に ```object.enlarge_object``` を指定できるか確認します。

Blenderのエリア構成が [1-3節](../chapter_01/03_Prepare_Add-on_development_environment.md) で紹介したように設定されていれば、左上のウィンドウが *Pythonコンソール* エリアとなります。


<div id="process_title"></div>

##### Work

<div id="process"></div>

|<div id="box">1</div>|オペレータクラスを登録すると ```bpy.ops.<オペレーションクラスのbl_idname>``` に登録されることを利用し、 *Pythonコンソール* に ```bpy.ops.object.enlarge_obje``` と入力してみます。入力が完了したら、WindowsやLinuxであれば *Ctrl + Space* 、Macであれば *control + space* を押します。ここで単語が補完されなければ、 ```bl_idname``` として ```bpy.ops.object.enlarge_object``` が利用されていないことになり、 ```bl_idname``` に指定可能であることがわかります。|![Pythonコンソール 使い道1](https://dl.dropboxusercontent.com/s/xazuoclt1k0y4t7/blender_python_console_1.png "Pythonコンソール 使い道1")|
|---|---|---|

<div id="process_sep"></div>

---

<div id="process"></div>

|<div id="box">2</div>|本節で作成したアドオンを有効化した後に、先ほどの操作を *Pythonコンソール* で行ってみます。*Pythonコンソール* に以下を打ち込んで、 *Enterキー* を押します。|![アドオン作成 手順1](https://dl.dropboxusercontent.com/s/6x7jkbaadtehb2e/blender_make_add-on_1.png "アドオン作成 手順1")|
|---|---|---|

<div id="process_start_end"></div>

---

```python
>>> bpy.ops.object.enlarge_object()
```

すると、 *3Dビュー* エリアのメニューから *オブジェクト* > *選択オブジェクトの拡大* を実行した時と同様の効果が得られることがわかります。
このようにオペレータクラスを登録すると、オペレータクラスの ```bl_idname``` が ```bpy.ops.<オペレーションクラスのbl_idname>``` として登録されます。

![Pythonコンソール 使い道2](https://dl.dropboxusercontent.com/s/rxa9lx8uk12ytoq/blender_python_console_2.png "Pythonコンソール 使い道2")

### Pythonコンソールのショートカットキー

Pythonコンソールでは以下のようなショートカットキーが利用できますので、作業効率化の際にぜひ活用してみてください。

|Windows|Mac|Linux|動作|
|----|----|----|----|
|Shift + F4|shift + fn + F4|Shift + F4|Python Consoleを開く|
|↑|↑|↑|過去に入力した履歴を戻る|
|↓|↓|↓|過去に入力した履歴を進める|
|Tab|tab|Tab|インデント|
|Shift + tab|shift + tab|shift + tab|アンインデント|
|Home|⌘ + ←|Home|行頭にカーソルを移動|
|End|⌘ + →|End|行末にカーソルを移動|
|Ctrl + ←|control + ←<br>fn + ←|Ctrl + ←|単語単位で左にカーソルを移動|
|Ctrl + →|control + →<br>fn + →|Ctrl + →|単語単位で右にカーソルを移動|
|Ctrl + Backspace|control + delete|Ctrl + Backspace|カーソルの位置から行頭に向かって単語単位で削除|
|Ctrl + Delete|control + fn + delete|Ctrl + Delete|カーソルの位置から行末に向かって単語単位で削除|
|Shift + Enter|shift + return|Shift + Enter|行を削除|
|Ctrl + Space|control + space|Ctrl + Space|ソースコード補完|
|Ctrl + ↑<br>Ctrl + ↓|control + ↑<br>control + ↓|Ctrl + ↑<br>Ctrl + ↓|コンソールのフルスクリーン化/解除|
|Ctrl + C|control + C|Ctrl + C|選択部分をクリップボードへコピー|
|Ctrl + V|control + V|Ctrl + V|クリップボードからの貼り付け|
|Shift + Ctrl + C|shift + control + C|Shift + Ctrl + C|Python Consoleで実行されたスクリプトを全てコピー<br>実行結果についてもコピーされるが、行頭に「#~ 」が自動的についてコメント化される|
※ ⌘はMacのcommandを指す

Macでショートカットキーを利用するためには、Mission Controlなどに標準で割り当たっているcontrolと⌘キーについて、以下の手順に沿って割り当てを解除する必要があります。

<div id="process_title"></div>

##### Work

<div id="process_noimg"></div>

|<div id="box">1</div>|*システム環境設定...* をクリック|
|---|---|

<div id="process_sep"></div>

---

<div id="process_noimg"></div>

|<div id="box">2</div>|*キーボード* をクリック|
|---|---|

<div id="process_sep"></div>

---

<div id="process_noimg"></div>

|<div id="box">3</div>|*ショートカット* タブをクリック|
|---|---|

<div id="process_sep"></div>

---

<div id="process_noimg"></div>

|<div id="box">4</div>|Blender Python Consoleで使いたいショートカットキーの割り当てを解除|
|---|---|

<div id="process_start_end"></div>

---

## まとめ

複数のオペレータクラスを作る方法や *コンソール・ウィンドウ* にメッセージを出力する方法を紹介しました。

また本節では、 *Pythonコンソール* の利用方法を紹介しました。
*Pythonコンソール* を利用することでAPIやテストしたい処理をその場で実行して動作確認できるため、アドオン開発の効率化のためにぜひ活用していきましょう。

<div id="point"></div>

### ポイント

<div id="point_item"></div>

* オペレーションを複数作るためには、作りたいオペレーションの数だけオペレータクラスを作成する必要がある
* ```self.report()``` メソッドを利用することで、 *コンソール・ウィンドウ* にメッセージを出力することができる
* 登録したオペレータクラスの ```bl_idname``` は ```bpy.ops.<bl_idname>``` に登録される
* *Pythonコンソール* を利用して、APIの確認や簡単なテストを行える
