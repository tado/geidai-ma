# TouchDesigner基本 6 – POPで3Dオブジェクトを操作する

## 本日の内容

- 前回に引続き、TouchDesignerの新たなオペレーターファミリーのPOPについて取り上げる
- 今回は、前回の知識を応用して、より高度な3Dのオブジェクトを操作していく
- 形状の複製、属性 (Atribute) の操作、アニメーション、軌跡の描画、ノイズによる形状の操作、形状の捻れ、物理ベースレンダリング (PBR) などのトピックスを扱う

## 3D形状の複製

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-pointCopy-scaled.jpg" width=640>

- POPのプリミティブ図形で元になる3D形状を用意、このサンプルではSphere
- Point Generator POPで図形を複製する座標を生成
- Copy POPで3Dの形状をコピー
- Geometory Compに追加してレンダリング

## ポイントの属性 (Attribute) の操作

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-changeAttribute-scaled.jpg" width=640>

- Random POP、Pattern POP、Attribute POPなどを使用することで、ポイントごとに属性 (Attribute) を設定していくことが可能
- ポイントの属性
  - P (Position): ポイントの位置座標
  - N (Normal): 表面の法線ベクトル
  - Color: ポイントの色とアルファ値
  - Tex: テクスチャ座標
  - Weight: ポイントのウェイト値
  - PointScale: レンダリング時のポイントのスケール・大きさ
  - LineWidth: 線の太さ
  - ...など
  - 参考: [TouchDesigner POP Attributes 101](https://youtu.be/CVPAA0CPvuA?si=_2L1Tvd4UEAIdsiO)
- この例では、Random POPを使用して各ポイントにランダムなPoint Scaleを設定することで球のサイズをランダムに設定している

## ポイントの座標を動かす - アニメーション

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-noiseMove-scaled.jpg" width=640>

- それぞれのポイントのP (座標) を個別に動かすことで球体を独立して動かすことが可能
- Noise POPやPatter POP、Particle POPなどを使用して動きのパターンを作る
- この例では、Noise POPを使用している
- Noise POPでは、Perlin Noise (Simplex Noise) だけでなく、Curl Noiseを使用したアニメーションも可能

## ポイントの移動の軌跡を描く

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-sphereTrail-scaled.jpg" width=640>

