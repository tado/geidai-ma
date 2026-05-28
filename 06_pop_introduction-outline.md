# TouchDesigner基本 5 – POP入門

## POPとは?

Forget Everything? 

[What Are POPs? The New TouchDesigner Operator Family That Changes Everything](https://youtu.be/xCmn625J-vA?si=T8Y2wdXKvYgyOa6H)

<img src="https://yoppa.org/wp-content/uploads/2026/05/pop-forgeteverything-scaled.jpg" width="640">

* **新しいオペレーターファミリー**: 2005年のDAT導入以来となる、TouchDesignerの新しいオペレーターファミリーである。
* **GPUベースの処理**: 3Dジオメトリや数値データの生成および編集をすべてGPU上で実行する。従来のSOP（CPUベース）と比較して、大量のデータを扱う際のリアルタイムパフォーマンスが大幅に向上している。
* **対象とするデータ**: ポイント、ポリゴン、ポイントクラウド、パーティクル、ラインストリップなど、あらゆる3Dデータの構築や操作に対応している。
* **データ構造**: POP内のデータは、ポイントリスト、頂点（Vertex）リスト、プリミティブリストで構成される。各ポイントは位置（P）、法線（N）、色（Color）などのアトリビュートを保持し、独自のカスタムアトリビュートも柔軟に追加可能である。
* **オペレーターの主な分類**:
* **Generators系**: 外部データに依存せず、POP内で直接ポイントやジオメトリを生成する。
* **Filters系**: TransformやNoiseなど、ポイントの位置やアトリビュートを加工・編集する。
* **to POP系**: TOPのピクセルデータやCHOPのチャンネルデータ、既存のSOPデータなどをPOP形式に変換して取り込む。


* **レンダリングと外部連携**: Render TOPを用いたレンダリングやインスタンシングのソースとして利用できるほか、DMX照明、LEDアレイ、レーザーなどの外部デバイスへ数値データを渡す用途にも適している。

[Intro to POPs: The New Operator Family in TouchDesigner](https://www.youtube.com/watch?v=bWfUk6MF8B0)
この動画はPOPの基本的な概念やサポートされるデータ構造、従来のSOPとの違いについて解説しており、導入として最適です。

## POPでできること

- まずは、POPで何ができるのか体感してみる
- Derivativeの提供しているサンプル集 [POPs Examples Package](https://www.dropbox.com/scl/fo/dvvqnl61dgmicxoebl4sy/AFuNixO4WWcAbyM5KkUi9F4?rlkey=f152v4uuzou81c7477w1yf6im&st=zzro8oie&dl=0) をダウンロード
- POPGuide/POPGuide.toe を開く
- いろいろなサンプルを眺めてみましょう

<img src="https://yoppa.org/wp-content/uploads/2026/05/popGuide-scaled.jpg" width="640">

## POPについて学ぶ

<img src="https://learn.derivative.ca/wp-content/uploads/2025/11/109_10-cover.png" width="640">

[109 – POPs: Working with Points](https://learn.derivative.ca/courses/100-fundamentals/lessons/109-pops-working-with-points/)

- 最近追加されたDerivative社の公式のチュートリアルがとても良くできている
- この内容を元に進めていきたい

## POPの概要

TouchDesignerにおけるPOP（Particle Operators）は、GPUで高速処理される3Dデータ作成・操作ツールです。点（ポイント）や属性情報を基盤とし、頂点やプリミティブを通じて効率的で現代的な3Dワークフローを実現します。

- POPはGPUで並列処理を行うため、SOPに代わる高速かつ最新の操作手法として活用できる。
- データの核となるのは「ポイント」であり、位置（P）、色（Color）、法線（N）などの属性を持つ。
- ポイントを接続する構造体として「プリミティブ」があり、点、線、三角形、四角形などが含まれる。
- プリミティブはポイントリストを参照する頂点（バーテックス）で構成され、プリミティブや頂点自体にも属性を持たせることが可能。
- 粒子システムや点群、ポリゴンなど、多様な3D形状をGPUアクセラレーションによって効率的に生成・編集できる。

## POPの基本 - ポイントの属性 (attribute) の値を表示

<img src="https://yoppa.org/wp-content/uploads/2026/05/pop-showinfo-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/06/01_pop-showinfo.toe)

- POPもSOPと同様に様々な基本図形が用意されている
- ここでは、直線、円、球をとりあげて、頂点の情報を表示してみる
- POP to Chopをするとそれぞれの頂点の情報がテーブル形式で閲覧できる
- 以下の情報が確認できる
  - Index: 頂点番号
  - P(0), P(1), P(2): 頂点の座標 (x, y, z)
  - N(0), N(1), N(2): 法線ベクトル (x, y, z)
  - Tex(0), Tex(1), Tex(2): テクスチャ座標（UVマップ情報）(u, v, w)

 ## POPのアトリビュート (属性) を使用する

アトリビュート（属性）は、POP（Particle Operator）における基本的なデータ要素です。POPは、一連のポイントアトリビュートで構成されるポイントのリストを含んでいます。すべてのポイントには「P」と呼ばれる位置アトリビュートがあり、それぞれX、Y、Z次元に対応する3つのコンポーネント（P(0)、P(1)、P(2)）を持っています。

Derivative社は、TouchDesignerで頻繁に使用される「標準アトリビュート」と「共通アトリビュート」を定義しています。標準アトリビュートには、位置（P）、色（Color）、法線（Normal）、テクスチャ（Tex）などがあります。一方、共通アトリビュートには、PointScale（ポイントスケール）、LineWidth（線幅）、Speed（速度）、Weight（重み）などが含まれます。

POPには、以下のようなさまざまな方法で追加のアトリビュートを付与できます。

- Attribute POPを使用する
- 一部のPOPにある「New Attribute（新規アトリビュート）」パラメータを使用する
- 一部のPOPにある「Output Attribute Scope（出力アトリビュート範囲）」パラメータを使用する

下記のサンプルでは、以下のことをしている。

- Circle POPで円形の頂点を用意
- Pattern POPで、頂点の大きさ (PointSize) を規則的に変化
- もう一つのPattern POPで色 (Color.rgb を規則的に変化
- マテリアルをLine MATにしてポイントスプライトを描画
- 頂点のサイズと色が変化するアニメーションへ

<img src="https://yoppa.org/wp-content/uploads/2026/05/pop-addAttribute-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/06/02_pop-addAttribute.toe)

## POPにおける色の扱い

- POPでは「Color」属性（赤・緑・青・アルファの4成分）を使ってポイントごとに色を制御する。
- Attribute POPやPattern POPを使用することで、全体への一括適用やプロシージャルなグラデーション作成が可能。
- Lookup Texture POPを使うと、UV座標（Tex(0), Tex(1)等）に基づき、テクスチャから各ポイントに色を割り当てられる。
- 位置情報などの数値を色に反映させる場合、Normalize POPを用いて値を0〜1の範囲に変換するのが効果的である。
- 下記の例では、Movie File Inで取得したテクスチャーのRGB情報を、Lookup Texture POPを用いてGrid POPの頂点の色に適用している

<img src="https://yoppa.org/wp-content/uploads/2026/05/pop-lookupColor-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/06/03_pop-lookupColor.toe)

- さらに少しの工夫で頂点と色を使用したアニメーションを作成可能
- 下記の例は、Noise POPを使用して頂点の位置を動かしている

<img src="https://yoppa.org/wp-content/uploads/2026/05/pop-lookupColorNoise-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/06/04_pop-lookupColorNoise.toe)

## POPを使用したパーティクルプログラミング

Particle POPは、GPUを活用して数百万のパーティクルを高速にシミュレーションするTouchDesignerの中核機能です。フィードバックループを構築することで、複雑な物理挙動や継続的な進化を可能にします。

- Particle POPは、GPUアクセラレーションにより膨大な数のポイントをシミュレーションできる。
- 動作には「Target POP」パラメータへの参照が不可欠であり、通常は出力先にNull POPを配置して指定する。
- Particle POPとNull POPの間にオペレーターを挟むことで、次のフレームへ演算結果を引き継ぐループを構築できる。
- 放射設定では、定数レートだけでなく、属性マッピングによる初期状態や質量、抵抗の制御が可能。
- フィードバックループの活用により、力（PartForce）や速度減衰を累積させ、動的で複雑なパーティクル表現を実現する。
- 下記の例では、Grid Popの頂点をParticle POPによって雨のように上から下に向かって落ちてくるようにしている。
- Noise Popを使用して動きにゆらぎを持たせている

<img src="https://yoppa.org/wp-content/uploads/2026/05/pop-particleRain-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/06/05_pop-particleRain.toe)

- もう一つ別のParticleの例
- このサンプルはSphere POPからParticleを発生させている
- 初期速度をSphere POPの法線ベクトルに設定する箇所がポイント

<img src="https://yoppa.org/wp-content/uploads/2026/05/pop-particleSphere-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/06/06_pop-particleSphere.toe)