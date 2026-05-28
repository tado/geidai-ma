# TouchDesigner基本 6 – POP入門

## メディアアート・プログラミング I

東京藝術大学芸術情報センター (AMC)
田所 淳

## 本日の内容

今回の講義では、TouchDesignerに新しく追加されたオペレーターファミリー「POP（Point Operators）」の基礎を学びます。POPはGPU上で3Dデータを高速に生成・操作できる仕組みであり、従来のSOPとは異なるアーキテクチャと考え方が必要です。まずPOPとは何か、どのようなデータ構造を持つかを理解し、Derivative社の公式チュートリアルを参照しながらPOPの全体像を把握していきます。

## POPとは?

「Forget Everything?」というキャッチコピーが示すように、POPはTouchDesignerのワークフローに新たな考え方をもたらすオペレーターファミリーです。

[What Are POPs? The New TouchDesigner Operator Family That Changes Everything](https://youtu.be/xCmn625J-vA?si=T8Y2wdXKvYgyOa6H)

<img src="https://yoppa.org/wp-content/uploads/2026/05/pop-forgeteverything-scaled.jpg" width="640">

POPは2005年のDAT導入以来となる、TouchDesignerにおける新しいオペレーターファミリーです。最大の特徴はGPUベースの処理にあります。3Dジオメトリや数値データの生成および編集をすべてGPU上で実行するため、従来のCPUベースであるSOPと比較して、大量のデータをリアルタイムで扱う際のパフォーマンスが大幅に向上しています。

POPが対象とするデータは多岐にわたります。ポイント、ポリゴン、ポイントクラウド、パーティクル、ラインストリップなど、あらゆる3Dデータの構築や操作に対応しています。POP内のデータはポイントリスト・頂点（Vertex）リスト・プリミティブリストで構成されており、各ポイントは位置（P）、法線（N）、色（Color）などのアトリビュートを保持します。さらに独自のカスタムアトリビュートも柔軟に追加することが可能です。

POPのオペレーターは主に3つのカテゴリに分類されます。**Generators系**は外部データに依存せず、POP内で直接ポイントやジオメトリを生成します。**Filters系**はTransformやNoiseなど、ポイントの位置やアトリビュートを加工・編集します。**to POP系**はTOPのピクセルデータやCHOPのチャンネルデータ、既存のSOPデータなどをPOP形式に変換して取り込みます。

また、POPはRender TOPを用いたレンダリングやインスタンシングのソースとして利用できるほか、DMX照明、LEDアレイ、レーザーなどの外部デバイスへ数値データを渡す用途にも適しています。

以下の動画は、POPの基本的な概念やサポートされるデータ構造、従来のSOPとの違いについて解説しており、導入として最適です。

[Intro to POPs: The New Operator Family in TouchDesigner](https://www.youtube.com/watch?v=bWfUk6MF8B0)

## POPについて学ぶ

<img src="https://learn.derivative.ca/wp-content/uploads/2025/11/109_10-cover.png" width="640">

[109 – POPs: Working with Points](https://learn.derivative.ca/courses/100-fundamentals/lessons/109-pops-working-with-points/)

Derivative社が最近公開した公式チュートリアル「109 – POPs: Working with Points」は、POPを体系的に学ぶための良くできた教材です。今回の講義はこのチュートリアルの内容を軸に進めていきます。

## POPの概要

TouchDesignerにおけるPOPは、GPUで高速処理される3Dデータ作成・操作ツールです。POPはGPUで並列処理を行うため、CPUベースのSOPに代わる高速かつ最新の操作手法として活用できます。

データの核となるのは「ポイント」であり、各ポイントは位置（P）、色（Color）、法線（N）などの属性を持ちます。ポイントを接続する構造体として「プリミティブ」があり、点、線、三角形、四角形などの形状が含まれます。プリミティブはポイントリストを参照する頂点（バーテックス）で構成されており、プリミティブや頂点自体にも属性を持たせることが可能です。このような構造により、粒子システムや点群、ポリゴンなど多様な3D形状をGPUアクセラレーションによって効率的に生成・編集することができます。

## POPの基本 – ポイントの属性（attribute）の値を表示

<img src="https://yoppa.org/wp-content/uploads/2026/05/pop-showinfo-scaled.jpg" width="640">

POPにもSOPと同様に、直線・円・球などさまざまな基本図形が用意されています。ここでは直線、円、球を取り上げ、各頂点の情報を実際に表示してみます。**POP to CHOP**を使うと、各頂点の情報をテーブル形式で確認することができます。

表示される情報は次の通りです。**Index**は頂点番号、**P(0)・P(1)・P(2)**は頂点の座標（x, y, z）、**N(0)・N(1)・N(2)**は法線ベクトル（x, y, z）、**Tex(0)・Tex(1)・Tex(2)**はテクスチャ座標（UVマップ情報、u, v, w）です。このようにPOP to CHOPを活用することで、POPが保持するアトリビュートの値を視覚的に把握することができます。

## POPのアトリビュート（属性）を使用する

アトリビュート（属性）は、POPにおける基本的なデータ要素です。POPは一連のポイントアトリビュートで構成されるポイントのリストを含んでいます。すべてのポイントには「P」と呼ばれる位置アトリビュートがあり、それぞれX・Y・Z次元に対応する3つのコンポーネント（P(0)、P(1)、P(2)）を持っています。

Derivative社は、TouchDesignerで頻繁に使用される「標準アトリビュート」と「共通アトリビュート」を定義しています。標準アトリビュートには位置（P）、色（Color）、法線（Normal）、テクスチャ（Tex）などがあります。一方、共通アトリビュートにはPointScale（ポイントスケール）、LineWidth（線幅）、Speed（速度）、Weight（重み）などが含まれます。

POPには、Attribute POPを使用する方法、一部のPOPにある「New Attribute（新規アトリビュート）」パラメータを使用する方法、または「Output Attribute Scope（出力アトリビュート範囲）」パラメータを使用する方法など、さまざまな方法で追加のアトリビュートを付与することができます。

以下のサンプルでは、Circle POPで円形の頂点を用意し、Pattern POPで頂点のサイズ（PointSize）を規則的に変化させています。さらにもう一つのPattern POPで色（Color.rgb）を規則的に変化させ、マテリアルをLine MATに設定してポイントスプライトを描画します。これにより、頂点のサイズと色が変化するアニメーションが生まれます。

<img src="https://yoppa.org/wp-content/uploads/2026/05/pop-addAttribute-scaled.jpg" width="640">

## POPにおける色の扱い

POPでは「Color」属性（赤・緑・青・アルファの4成分）を使って、ポイントごとに色を制御します。Attribute POPやPattern POPを使用することで、全体への一括適用やプロシージャルなグラデーションの作成が可能です。

Lookup Texture POPを使うと、UV座標（Tex(0)、Tex(1)など）に基づいて、テクスチャから各ポイントに色を割り当てることができます。また、位置情報などの数値を色に反映させる場合は、Normalize POPを用いて値を0〜1の範囲に変換するのが効果的です。

下記の例では、Movie File Inで取得したテクスチャのRGB情報を、Lookup Texture POPを用いてGrid POPの各頂点の色に適用しています。

<img src="https://yoppa.org/wp-content/uploads/2026/05/pop-lookupColor-scaled.jpg" width="640">

さらに、Noise POPを組み合わせることで、頂点の位置を動かしながら色を変化させるアニメーションを作成することもできます。下記の例では、Noise POPを使用して頂点の位置をダイナミックに動かしています。

<img src="https://yoppa.org/wp-content/uploads/2026/05/pop-lookupColorNoise-scaled.jpg" width="640">

## POPを使用したパーティクルプログラミング

Particle POPは、GPUを活用して数百万のパーティクルを高速にシミュレーションするTouchDesignerの中核機能です。フィードバックループを構築することで、複雑な物理挙動や継続的な進化を可能にします。

- Particle POPは、GPUアクセラレーションにより膨大な数のポイントをシミュレーションできる。
- 動作には「Target POP」パラメータへの参照が不可欠であり、通常は出力先にNull POPを配置して指定する。
- Particle POPとNull POPの間にオペレーターを挟むことで、次のフレームへ演算結果を引き継ぐループを構築できる。
- 放射設定では、定数レートだけでなく、属性マッピングによる初期状態や質量、抵抗の制御が可能。
- フィードバックループの活用により、力（PartForce）や速度減衰を累積させ、動的で複雑なパーティクル表現を実現する。

Copy Save