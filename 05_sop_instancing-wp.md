# TouchDesigner基本 5 – ジオメトリのインスタンシング

## メディアアート・プログラミング I

東京藝術大学芸術情報センター (AMC)
田所 淳

## 本日の内容

今回の講義では、大量のオブジェクトをGPUで効率的に描画するための「ジオメトリインスタンシング」について学びます。従来のCopy SOPを使ったアプローチとは異なり、インスタンシングはGPU上で処理を行うため、数千・数万という大量の複製を高速かつメモリ効率よく描画することができます。まずインスタンシングの基本概念と仕組みを確認し、CHOP・SOP・TOPといった各データソースを使ってインスタンスの位置や色を個別に制御する方法を、段階的なサンプルを通じて学んでいきます。

## ジオメトリインスタンシングとは

ジオメトリインスタンシングとは、1つの元となるジオメトリをGPU上で効率的に複製し、大量に描画する機能です。設定はGeometry COMPの「Instance」ページから行います。

従来のCopy SOPはCPU上で処理するため、複製数が増えるほど負荷が上がりリアルタイム描画が困難になります。一方、インスタンシングはGPUで処理するため、同じ数の複製でも大幅に高速で動作します。また、元となるジオメトリのデータは1つだけで済むため、メモリ使用量も抑えられるという利点があります。

基本的な仕組みは次の通りです。まずGeometry COMPに元ジオメトリ（SOP）を接続し、「Instance」タブをオンにします。次に「Instance OP」に位置や色などのデータを持つオペレータ（CHOP / TOP / SOP / DAT）を指定することで、各インスタンスを個別に制御できるようになります。

### Instanceタブの設定

<img src="https://yoppa.org/wp-content/uploads/2025/11/Screenshot-2025-05-06-165945.jpg" width="640">

Instance OPとして利用できるデータソースは以下の4種類です。CHOPはチャンネルデータを使い、サンプル数がそのままインスタンス数に対応します。TOPはピクセルデータを使い、各ピクセルのRGB値が座標や色に対応します。SOPはポイント座標やアトリビュートを使い、各ポイントに1つのインスタンスが対応します。DATはテーブルデータとして数値を記述して使用します。

インスタンシングで制御できる主なパラメータは次の通りです。**Translate**（tx, ty, tz）で各インスタンスの位置を、**Rotate**（rx, ry, rz）で回転を、**Scale**（sx, sy, sz）でスケールを制御します。また、**Color**（colorr, colorg, colorb, colora）で色や透明度を個別に設定することも可能です。その他にもPivot（回転・スケールの基点）、Texture Coordinate、Custom Attributesなどを制御できます。

## CHOPによるインスタンシング

### 1. CHOPによるインスタンシング – 位置制御

<img src="https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-06-165133.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/05-01_instancing-basic.toe)

まずはCHOPを使ったインスタンシングの基本から始めます。Sphere SOPを作成してGeometry COMPに接続し、Geometry COMPのInstanceタブをオンにします。次にPattern CHOPを作成してInstance OPに指定します。Pattern CHOPでRampを設定するとx軸方向にインスタンスが均等に並び、Sineを設定するとy軸方向に波状に配置することができます。このように、CHOPのサンプル数がそのままインスタンスの総数になります。

### 2. CHOPによるインスタンシング – ランダム配置

<img src="https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-06-170829.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/05-02_instancing-random.toe)

次に、大量のオブジェクトを3次元空間にランダムに配置してみましょう。Pattern CHOPでパターンの種類として**Random**を選択し、tx・ty・tzのチャンネルにそれぞれ設定します。これにより、各インスタンスがランダムな位置に配置されます。Pattern CHOPのサンプル数を増やすだけでインスタンスの数を増やすことができ、3次元空間に大量のオブジェクトを高速に描画することが可能です。

## SOPによるインスタンシング

### 3. SOPによるインスタンシング – 位置制御

<img src="https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-06-171543.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/05-03_instancing-sop.toe)

SOPの頂点位置をインスタンスの配置に利用することもできます。ただし、SOPはそのままではInstance OPとして直接使用できないため、**SOP to CHOP**を使ってSOPのデータをCHOPに変換する必要があります。変換後、頂点の座標がインスタンスのTranslate（tx, ty, tz）に対応し、SOPの頂点ごとに1つのインスタンスが生成されます。これにより、任意の3D形状の表面にインスタンスを配置するような表現が可能になります。

### 4. SOPによるインスタンシング応用 – ノイズ変形

<img src="https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-06-173008.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/05-04_instancing-sop-noise.toe)

SOPを使ったインスタンシングをさらに発展させ、Noise SOPを加えることで頂点位置を動的に変形させてみましょう。Noise SOPで変形させた後の頂点座標をSOP to CHOPで変換してInstance OPに指定します。Phong MATを適用してマテリアルも設定することで、ライティングも伴った豊かな表現になります。ノイズの時間変化によって頂点位置が刻々と変わるため、有機的に動くアニメーションを生成することができます。

## TOPによるインスタンシング

### 5. TOPによるインスタンス着色

<img src="https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-06-174012.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/05-05_instancing-noise-color.toe)

TOPを使って各インスタンスの色を個別に制御してみましょう。Noise TOPを作成し、**TOP to CHOP**でTOPのピクセルデータをCHOPに変換します。変換されたRGB値がインスタンスのColor（colorr, colorg, colorb）に対応します。このとき重要なのは、**インスタンス数とTOPのピクセル数を一致させる**ことです。たとえばインスタンス数を2048個にする場合、Noise TOPのサイズも2048×1ピクセルに設定します。

### 6. TOPによるインスタンシング – 位置制御アニメーション

<img src="https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-06-182812.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/05-06_instancing-noise-top.toe)

TOPのRGB値を座標として利用し、インスタンスの位置をアニメーションさせることができます。TOPのR値をx座標、G値をy座標、B値をz座標に対応させることで、ピクセルの色がそのままインスタンスの空間配置を決定します。このとき、ピクセルフォーマットは**32-bit float（rgba）**を使用することで精度の高い座標値を扱えます。Noise TOPのノイズが時間とともに変化することで、インスタンスが滑らかに動くアニメーションが生まれます。インスタンス数とTOPのピクセル数を合わせる点はカラー制御のときと同様です（例: 2048×1）。

### 7. TOP + CHOPによるハイブリッドアニメーション

<img src="https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-06-184044.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/05-07_instancing-ramp-top.toe)

TOPとCHOPを組み合わせることで、それぞれの長所を活かしたより複雑なアニメーションが実現できます。Noise TOPでx・y座標を制御し、Pattern CHOP（Ramp）でz座標を制御します。2つのデータソースは**Merge CHOP**で結合してから Instance OPに指定します。この構成により、xy平面ではTOPのノイズが揺らぎを与え、z軸方向にはRampによって手前に向かって降下するようなエフェクトを作ることができます。複数のデータソースを組み合わせることで、表現の幅が大きく広がります。

### 8. 3Dノイズによる応用アニメーション

<img src="https://yoppa.org/wp-content/uploads/2025/11/Screenshot-2025-11-11-074505.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/05-08_instancing-3Dnoise.toe)

最後に、3Dノイズの情報を位置・色・角度のすべてに対応させた、より複雑な応用アニメーションに挑戦してみましょう。複数のNoise TOPを用意し、それぞれを位置制御・カラー制御・回転制御に割り当てることで、各パラメータを独立して細かくコントロールすることができます。ノイズが時間とともに変化するにつれて、インスタンスの位置・色・向きがすべて動的に変わり、複雑で有機的な3Dアニメーションが生み出されます。ここまで学んだ技術を自由に組み合わせ、オリジナルの表現を探ってみてください。
