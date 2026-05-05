# TouchDesigner基本 3 - SOPの基本

## メディアアート・プログラミング I

東京藝術大学芸術情報センター (AMC)
田所 淳

## 本日の内容

本日の講義では、SOP（Surface Operators）の基本的な使い方を学びます。まずSOPとは何かという基本から始め、プリミティブ図形（基本図形）の作成方法を確認します。続いて、レンダリングの基本としてカメラやライトの設定方法を習得し、最後にプロシージャルモデリングの基本へと発展させていきます。

## SOPの基本

SOP（Surface Operators）は、3Dジオメトリを生成・編集・操作するためのオペレーターファミリーです。ここでいうジオメトリとは、3D空間における形状や構造を表すデータのことです。SOPでは、点・線・ポリゴンメッシュ・NURBSカーブ/サーフェス・メタボールなど、さまざまなタイプの3Dデータを扱うことができます。

## 参考： SOP から POP へ — 新たなファミリーへの移行

TouchDesigner の 3D ジオメトリ処理を長年支えてきた SOP(Surface Operators)は、1995 年に設計された TouchDesigner 最古の演算子ファミリーです。しかし CPU を前提とした設計のため、近年急増する大規模点群や複雑なパーティクル表現をリアルタイムで扱うには性能面で限界が見え始めていました。

この課題に応えるべく、Derivative は 2025 年の Official リリースで POP(Point Operators) という新しい演算子ファミリーを正式に投入しました。これは DAT(2005 年)以来およそ 20 年ぶりとなるファミリー追加で、POP は GPU 上で動作するため SOP と比較して飛躍的に高いパフォーマンスを発揮します。さらに SOP・CHOP・TOP の利点を統合し、シェーダーを書かずにノードベースで GPU データを扱える汎用的な仕組みとなっています。

Derivative 自身、POP を SOP の「置き換えであり再考」と公式に位置付けており、長期的には POP が 3D 処理の主軸となる見通しです。ただし NURBS やベジェ曲面など SOP 固有の機能はまだ POP でカバーされておらず、当面は両者が共存する予定です。

POPについては回を改めて詳細に紹介していきます。


## プリミティブ図形の作成

SOPの基本的な使い方を学ぶために、まずはプリミティブ図形（基本図形）を作成してみましょう。

<img src="https://yoppa.org/wp-content/uploads/2025/04/Screenshot-2025-04-30-064325-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04/01_sop_primitives.toe)

TouchDesignerには、2Dと3Dのさまざまなプリミティブ図形を生成するSOPが用意されています。2D系では、Line SOPで直線を、Circle SOPで円を、Rectangle SOPで長方形を、Grid SOPで格子状の平面をそれぞれ作成することができます。3D系では、Box SOPで立方体を、Sphere SOPで球を、Tube SOPで円筒を、Torus SOPでトーラス（ドーナツ形）をそれぞれ生成することができます。

## SOPの操作

<img src="https://yoppa.org/wp-content/uploads/2025/04/Screenshot-2025-04-30-070619-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04-02_sop_manipurate.toe)

SOPのオペレーター上では、さまざまな操作を行うことができます。SOPを選択してパラメータウィンドウから設定を変更したり、ビューワーをアクティブにしてマウスで直接操作したりすることができます。また、ワイヤーフレーム表示・頂点の表示・法線の表示といった表示モードの切り替えも可能で、目的に応じて3Dジオメトリの確認方法を変えながら作業を進めることができます。

## SOPのレンダリング

SOPを配置するだけでは、3Dジオメトリの形状を定義しているに過ぎません。最終的に画像として表示するためには、Render TOPを使用してレンダリングを行う必要があります。Render TOPは、3Dシーンを2D画像としてレンダリングするためのコンポーネントです。3Dを2D画像としてレンダリングするためには、次の3つの要素が必要になります。

1. カメラ: 3Dシーンを撮影するための視点
2. ライト: 3Dシーンを照らすための光源
3. マテリアル: 3Dジオメトリの見た目を定義するための素材

実際にオペレーターを接続しながら解説していきます。

### SOPのレンダリング基本

<img src="https://yoppa.org/wp-content/uploads/2026/05/sop-render-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04-03_sop_render.toe)

まずSOPでプリミティブ図形を作成し、Geometry COMPを作成してSOPを接続します。続いてCamera COMPとLight COMPを配置し、Render TOPを作成すると、Camera COMPとLight COMPが自動的に接続されます。この手順でシンプルな3Dレンダリングの基本を理解することができます。

### 参考: POPのレンダリング

<img src="https://yoppa.org/wp-content/uploads/2026/05/pop-render-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04/04_pop_render.toe)

POPでもSOPと同様に、3Dプリミティブのレンダリングが可能です。SOPの機能の一部は今後徐々にPOPへ移行していく予定となっています。POPについては、別の回で詳しく取り上げます。

### SOPにマテリアルの適用 + 回転

<img src="https://yoppa.org/wp-content/uploads/2026/05/sop-material-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04/05_sop_material.toe)

Phong MATを作成してGeometry COMPに接続することで、ジオメトリにマテリアルを適用することができます。Render TOPに接続されたGeometry COMPのマテリアルを変更することで、ジオメトリの見た目を自由に変えることができます。

### SOPのレンダリング - 様々なパラメータを調整

<img src="https://yoppa.org/wp-content/uploads/2026/05/sop-materal2-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04/06_sop_transform.toe)

さまざまなパラメータを調整して、3Dジオメトリの見た目を変更してみましょう。カメラの画角や位置、マテリアルの色や質感、ライトの位置や強度、SOPの形状やサイズなど、調整できるパラメータは多岐にわたります。いろいろと試行錯誤しながら、自分なりの表現を探ってみてください。

## 応用: プロシージャルモデリング

SOPを使うことで、プロシージャルに3Dジオメトリを生成することができます。プロシージャルモデリングとは、数式やアルゴリズムを使って自動的に3D形状を生成する手法です。SOPのパラメータを数式やCHOPのデータにリンクさせることで、動的に変化するジオメトリを作成することができます。

### SOPのMergeとCopyとTransformでモデリング

<img src="https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-01-145105-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04/07_sop_copy.toe)

Merge SOPを使って複数のジオメトリを結合し、Copy SOPを使ってジオメトリを複製することができます。複製の際には位置・回転・スケールをCopy SOPのTransformタブで設定することが可能です。さらにTransform SOPを使って複製したジオメトリの位置や回転を変更することもできます。いろいろ試してみましょう。

### Sweep SOPで面を回転しながら押し出して整形

<img src="https://yoppa.org/wp-content/uploads/2026/05/sop-sweep-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04/08_sop_sweep.toe)

Sweep SOPを使うと、指定した形状をパスに沿って回転させながら押し出し、独特の3D形状を生成することができます。まずSweep SOPのOutputタブで **Skin Output** を **On** にします。次にConvert SOPを接続してConvert to **Polygon** を指定することで、スキンをポリゴンメッシュとして扱えるようにします。さらにFacet SOPで **Compute Normal** を **On** にすることで、ライティングが正しく計算されるよう法線情報を整えます。あとはこれまでと同様にGeometry COMPに接続してレンダリングし、Render TOPで画像として出力すれば完成です。

### 応用: より複雑なプロシージャルモデリング

<img src="https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-01-160525-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04/09_sop_complex.toe)

さまざまなSOPを組み合わせることで、より複雑なプロシージャルモデリングに挑戦することができます。例えば、Line SOPとSweep SOPを組み合わせることで複雑な形状を作ることができます。さらにLine SOPをPattern SOPで変形させて波打つような形状へと変え、Copy SOPで複製を重ねていくことで、複雑で個性的な3Dジオメトリを生成することができます。

## 実習: プロシージャルモデリングに挑戦

ここまでの内容を踏まえて、Copy SOPやTransform SOPを使って、プロシージャルモデリングに挑戦してみましょう。例えば、複数の円柱を並べた形状、波打つような形状、複雑な幾何学模様など、自分なりのアイデアで自由に形状を作成してみてください。

## 応用サンプル

いくつか応用的なサンプルを作成しました。参考にしてください。

- [応用サンプル 1](https://github.com/tado/tdexamples/blob/main/04/10_sop_procedual3D.toe)
- [応用サンプル 2](https://github.com/tado/tdexamples/blob/main/04/11_sop_uneune.toe)
- [応用サンプル 3](https://github.com/tado/tdexamples/blob/main/04/12_sop_uneNoise.toe)
