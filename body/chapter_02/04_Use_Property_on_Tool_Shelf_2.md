<div id="sect_title_img_2_4"></div>

<div id="sect_title_text"></div>

# ツール・シェルフの<br>オプションを活用する②

<div id="preface"></div>

###### 本節では、オブジェクトを複製するサンプルアドオンを紹介します。前節で解説したツール・シェルフについてより詳しく解説します。

## 作成するアドオンの仕様

* *3Dビュー* エリアのメニューである *オブジェクト* に *選択オブジェクトの複製* を追加する
* 追加したメニューである *オブジェクト* > *選択オブジェクトの複製* を実行すると、選択中のオブジェクトを複製する
* *ツール・シェルフ* のオプションから、以下の複製したオブジェクトの配置先を選択可能とする
  * 3Dカーソル
  * 原点
  * 3Dビュー上に配置されているオブジェクト
* *ツール・シェルフ* のオプションから、複製したオブジェクトについて以下を指定可能とする
  * 拡大率
  * 回転角度
  * 配置場所からのオフセット

## アドオンを作成する

以下のソースコードを、 [1-4節](../chapter_01/04_Install_own_Add-on.md) を参考にして *テキスト・エディタ* に入力し、 ```sample_4.py``` という名前で保存してください。

[import](../../sample/src/chapter_02/sample_4.py)

## アドオンを使用する

### アドオンを有効化する

[1-4節](../chapter_01/04_Install_own_Add-on.md) を参考にして作成したアドオンを有効化すると、 *コンソール* に以下の文字列が出力されます。

```sh
サンプル4: アドオン「サンプル4」が有効化されました。
```

アドオン有効化後、*3Dビュー* エリアのメニューに *オブジェクト* > *選択オブジェクトの複製* が追加されていることを確認します。

### アドオンの機能を使用する

以下の手順に従って、作成したアドオンの機能を使ってみます。

<div id="process_title"></div>

##### Work

<div id="process"></div>

|<div id="box">1</div>|Blender起動直後に生成される *Cube* を選択し追加されたメニューを実行すると、選択したCubeが複製されます。|![オブジェクトの複製1](https://dl.dropboxusercontent.com/s/p2vi68nprlto5gc/use_add-on_1.png "オブジェクトの複製1")|
|---|---|---|

また、*コンソール・ウィンドウ* に以下のメッセージが表示されます。

```sh
サンプル4: 「Cube」を複製しました。
```

<div id="process_sep"></div>

---

<div id="process"></div>

|<div id="box">2</div>|*3Dビュー* エリアの *ツール・シェルフ* に表示されたオプションの値を変更すると、変更した値に応じて複製されたオブジェクトの形が変化します。|![オブジェクトの複製2](https://dl.dropboxusercontent.com/s/if9ztwmoc09nqt1/use_add-on_2.png "オブジェクトの複製2")|
|---|---|---|

今回のサンプルでは、以下に示すオプションが利用可能です。

|オプション名|オプション指定による動作への影響|
|---|---|
|*配置位置*|*3Dカーソル* : 3Dカーソルの位置に複製オブジェクトを配置します <br> *原点* : 3Dビューエリアの原点に複製オブジェクトを配置します <br> *オブジェクト名* : 選択したオブジェクトの中心位置に複製オブジェクトを配置します|
|*拡大率*|複製したオブジェクトについて、複製元のオブジェクトに対する拡大率を設定します|
|*回転角度*|複製したオブジェクトについて、複製元オブジェクトからの回転角度の差分をオイラー角で設定します|
|*オフセット*|複製したオブジェクトについて、配置位置からのオフセット位置を指定します|

<div id="process_start_end"></div>

---


### アドオンを無効化する

[1-4節](../chapter_01/04_Install_own_Add-on.md) を参考にしてアドオンを無効化すると、 *コンソール* に以下の文字列が出力されます。

```sh
サンプル4: アドオン「サンプル4」が無効化されました。
```

## ソースコードの解説

今回作成したアドオンは、[2-3節](03_Use_Property_on_Tool_Shelf_1.md) を改造したものです。
ここでは、 [2-3節](03_Use_Property_on_Tool_Shelf_1.md) で解説していない点について解説します。
なお新しく登場したBlender APIの説明は、ソースコードのコメントとして記載しています。

### セレクトボックスの追加

```EnumProperty``` クラスを用いると、ユーザがセレクトボックスにより値を選択することが可能なUIを作成することできます。
```EnumProperty``` クラスを用いてセレクトボックスの選択項目を追加するためには、```EnumProperty``` クラス作成時の引数 ```items``` に選択項目のリストを渡す必要があります。

以下に 選択項目として *3Dカーソル* と *原点* を追加するコードを示します。

```python
location_list = [
    ('3D_CURSOR', "3Dカーソル", "3Dカーソル上に配置します"),
    ('ORIGIN', "原点", "原点に配置します")]

location = EnumProperty(
    name = "配置位置",
    description = "複製したオブジェクトの配置位置",
    items = location_list
)
```

```items``` に以下の要素からなるタプルの配列を渡しています。

|要素|要素の意味|
|---|---|
|第1要素|識別子(項目選択時に変数 ```items``` に設定される値)|
|第2要素|セレクトボックスに表示される項目名|
|第3要素|セレクトボックスに表示される項目の説明|

これでセレクトボックスを作ることができましたが、3Dビュー上に配置されているオブジェクトの位置に複製したオブジェクトを配置するための項目が足りません。
基本的には *3Dビュー* 上に生成されているオブジェクト名一覧を項目に追加すれば良さそうですが、*3Dビュー* エリア上に配置されているオブジェクトは処理の実行状況に応じて変化するため、 ```items``` に渡す項目リストは動的に生成する必要があります。
例えば、*3Dビュー* エリアのメニュー *オブジェクト* > *選択オブジェクトの複製* を実行後に *オブジェクト* > *選択オブジェクトの複製* を再度実行した場合、最初に複製したオブジェクトを複製オブジェクトの配置先として選べる必要があります。

この問題に対し、紹介したサンプルでは以下のようにして ```EnumProperty``` クラスにより作られるセレクトボックスの選択項目を動的に追加する処理を記載しています。

```python
# EnumPropertyで表示したい項目リストを作成する関数
def location_list_fn(scene, context):
    items = [
        ('3D_CURSOR', "3Dカーソル", "3Dカーソル上に配置します"),
        ('ORIGIN', "原点", "原点に配置します")]
    items.extend([('OBJ_' + o.name, o.name, "オブジェクトに配置します") for o in bpy.data.objects])

    return items

# 選択したオブジェクトを複製するアドオン
class ReplicateObject(bpy.types.Operator):

    bl_idname = "object.replicate_object"
    bl_label = "選択オブジェクトの複製"
    bl_description = "選択中のオブジェクトを複製します"
    bl_options = {'REGISTER', 'UNDO'}

    location = EnumProperty(
        name = "配置位置",
        description = "複製したオブジェクトの配置位置",
        items = location_list_fn
    )
```

サンプルでは ```EnumProperty``` クラスの ```items``` に項目リストを渡す代わりに、項目リストを返す```location_list_fn()``` 関数を渡しています。
```location_list_fn()``` 関数は、3Dカーソルと原点についてセレクトボックスの選択項目リストを作成した後、 3Dビュー上に置かれている全オブジェクトについてセレクトボックスの選択項目を作成し、リストへ追加しています。
最後に作成したリストを返して関数が復帰します。
このように、セレクトボックスの選択項目リストを返す関数を ```EnumProperty``` クラスの ```items``` に指定することで、選択項目を動的に追加することができます。

### FloatVectorPropertyの引数subtypeとunit

サンプルでは、配置位置の他にも拡大率・回転角度・配置位置からのオフセットを、 *ツール・シェルフ* から指定できます。
これらの指定が可能となるように ```FloatVectorProperty``` クラスのインスタンス化時に、目的に沿った値に応じて引数 ```subtype``` を指定しています。
```subtype``` に指定可能な値とUIの例を以下に示します。

|値|値の説明|UI例|
|---|---|---|
|```NONE```|3要素から構成されるプロパティグループ|![オプション SUBTYPE NONE](https://dl.dropboxusercontent.com/s/2zcjq2iq5rrn85g/option_subtype_none.png "オプション SUBTYPE NONE")|
|```COLOR```|カラーパレット|![オプション SUBTYPE NONE](https://dl.dropboxusercontent.com/s/90sezitg7jsjhy8/option_subtype_color.png "オプション SUBTYPE COLOR")|
|```TRANSLATION```|X軸、Y軸、Z軸の3要素から構成されるプロパティグループ（単位はcm、mなど）|![オプション SUBTYPE TRANSLATION](https://dl.dropboxusercontent.com/s/a6fqhmw68vn6sqh/option_subtype_translation.png "オプション SUBTYPE TRANSLATION")|
|```VELOCITY```|X軸、Y軸、Z軸の3要素から構成されるプロパティグループ（単位はm/s）|![オプション SUBTYPE VELOCITY](https://dl.dropboxusercontent.com/s/mbb7er5ubn1no1f/option_subtype_velocity.png "オプション SUBTYPE VELOCITY")|
|```ACCELERATION```|X軸、Y軸、Z軸の3要素から構成されるプロパティグループ（単位はm/s<sup>2</sup>）|![オプション SUBTYPE ACCELERATION](https://dl.dropboxusercontent.com/s/pg3swy8nbk8p8ih/option_subtype_acceleration.png "オプション SUBTYPE ACCELERATION")|
|```EULER```|X軸、Y軸、Z軸の3要素から構成されるプロパティグループ（単位は°）|![オプション SUBTYPE EULER](https://dl.dropboxusercontent.com/s/r63sl5mv09v5f3h/option_subtype_euler.png "オプション SUBTYPE EULER")|
|```QUATERNION```|3要素（W、X、Y）から構成されるプロパティグループ|![オプション SUBTYPE QUATERNION](https://dl.dropboxusercontent.com/s/r82kz22g3eaba7h/option_subtype_quaternion.png "オプション SUBTYPE QUATERNION")|
|```AXISANGLE```|3要素（W、X、Y）から構成されるプロパティグループ（単位は°）|![オプション SUBTYPE AXISANGLE](https://dl.dropboxusercontent.com/s/5f8jn9423abkp5d/option_subtype_axisangle.png "オプション SUBTYPE AXISANGLE")|
|```XYZ```|X軸、Y軸、Z軸の3要素から構成されるプロパティグループ|![オプション SUBTYPE XYZ](https://dl.dropboxusercontent.com/s/p7rp8m1wiamr85n/option_subtype_xyz.png "オプション SUBTYPE XYZ")|

また引数 ```unit``` を指定すると、以下のような単位をUIに表示させることができます。

|値|値の説明|UI上での表示単位<br>（表示単位を度数表記・メートル法にした場合）|
|---|---|---|
|```NONE```||引数 ```subtype```に指定した値に応じて表示される単位が決定|
|```LENGTH```|長さ|cm|
|```AREA```|面積|cm<sup>2</sup>、m<sup>2</sup>など|
|```VOLUME```|長さ|cm<sup>3</sup>、m<sup>3</sup>など|
|```ROTATION```|角度|°|
|```TIME```|時間|（単位なし）|
|```VELOCITY```|速度|m/s|
|```ACCELERATION```|加速度|m/s<sup>2</sup>|

## まとめ

[2-3節](03_Use_Property_on_Tool_Shelf_1.md) で紹介した *ツール・シェルフ* の *プロパティ* を用いて、より複雑なアドオンを作成しました。
*ツール・シェルフ* の *プロパティ* を用いることで、直前に行った操作に対してユーザがより細かい調整を行った上で、操作を再実行することができることを説明しました。
ただし特定の機能に対してプロパティを追加しすぎると、指示できる項目が多すぎてかえってわかりづらくなるという問題もありますので、本当に必要な項目であるかを意識しながらプロパティを追加していきましょう。

<div id="point"></div>

### ポイント

<div id="point_item"></div>

* ```EnumProperty``` 生成時の引数 ```items``` に選択項目リストを返す関数を指定することで、セレクトボックスの選択項目を動的に追加できる
* プロパティクラス作成時に、引数 ```subtype``` を指定することで、目的に沿ったプロパティを簡単に作成できる
* プロパティクラス作成時に、引数 ```unit``` を指定することで、UIに単位を表示することができる
