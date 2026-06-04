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

- Trail POPを用いることで移動の軌跡を描くことができる
- 描いた軌跡は、Line MATで線として描画
- レンダリングしたテクスチャーにBloop TOPでエフェクトを付加している

## ノイズで球を変形

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-noiseSphere-scaled.jpg" width=640>

- レンダリングするPOPのプリミティブ図形自体のポイント情報も操作可能
- Noise POPを用いると図形をPerlin Noise (Simplex Noise) を使用して変形できる
- Noise POP の Output > Combine Operation を Translate along Normal にすると法線ベクトルの方向に変形するようになる。より自然な凹凸ができる。
- 変形操作を行った後は、法線ベクトルを補正する必要がある
- Noise POPの場合は Output > Computer Point Normals をONにする
- SOPと違いGPU上で演算が行なわれるので、負荷が少ない

## 変形した球の頂点に球を描画

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-SphereNoiseCopy-scaled.jpg" width=640>

- Copy POPを用いて、変形した球の頂点のポイントに球を描画
- SOPのGeometory Instancingした際と同様の表現が簡単に可能
- SOPと違い、GPUベースで計算するので、負荷が軽い

## 3Dの形状をねじって表現

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-torusTwist-scaled.jpg" width=640>

- Twist POPを使用すると、3Dの形状を「ねじった」ような形状を生成できる
- 変形した後は、狂った法線ベクトルを修正する必要がある
- この例では、Twist POPで変形した後で、Normal POPを使用して法線ベクトルを補正している

## 変形した形状を複製して合成

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-torusTwistCopy-scaled.jpg" width=640>

- Twist POPで変形した形態を、Copy POPで複製している
- 移動、回転、スケールの変更などを駆使することで、とても複雑な形態が生成される
- Torus以外の形態でも試してみる


## レンダリングを工夫する - PBR (物理ベースレンダリング) を試してみる

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-torusTwistPBR-scaled.jpg" width=640>

- PBR（物理ベースレンダリング）とは、現実世界の光の物理的な振る舞いをシミュレートし、3Dオブジェクトの質感や光の反射を極めてリアルに描画するコンピュータグラフィックスの技術です。
- TouchDesignerでは、PBR MATを使用することで物理ベースレンダリングが可能
- 先程作成したTwistしたトーラスをコピーした物体を、PBRでレンダリングしてみる

## POPの様々な属性を操作したアニメーション

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-instancing-scaled.jpg" width=640>

- 最後の実践的なサンプルとして、POPの様々な属性を操作したアニメーションを紹介する
- 画面の奥から大量の物体が回転しながら迫ってくる
- 以下のような手順で制作している
- Point Generator POPで元になるポイントを生成
- Random POPで着色
- Pattern POPで物体の大きさにばらつきを持たせる
- Random POPとMath POPを組み合わせて物体を回転
- Pattern POPで P(0)、P(1) にrandomを設定して(x, y)の位置をランダムに、P(2) はRampを設定して奥から迫ってくる動きを作成
- Geometory Comp、Camera Comp、Light Comp、Render TOP、Phong MATでレンダリング
- HSV Adjast TOP、Bloom TOPで色調補正と輝きを追加
- 以上の操作で、本格的なアニメーションが完成できました!