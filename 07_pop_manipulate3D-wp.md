# TouchDesigner基本 6 – POPで3Dオブジェクトを操作する

## メディアアート・プログラミング I

東京藝術大学芸術情報センター (AMC)
田所 淳

## 本日の内容

前回に引き続き、TouchDesignerの新たなオペレーターファミリーであるPOPについて取り上げます。今回は前回学んだ基礎知識を応用し、より高度な3Dオブジェクトの操作に挑戦します。具体的には、形状の複製、属性（Attribute）の操作、アニメーション、軌跡の描画、ノイズによる形状の変形、形状の捻れ、さらに変形した形状の複製と合成など、幅広いトピックを扱います。これらの技法を組み合わせることで、GPUの高速処理を活かした複雑で有機的な3D表現が可能になります。

## 3D形状の複製

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-pointCopy-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/01_POP-copySphere.toe)

POPでは、1つのプリミティブ図形を元に、3D形状を空間内で大量に複製することができます。基本的な手順は次の通りです。まずSphere POPなどPOPのプリミティブ図形で元となる3D形状を用意します。次に**Point Generator POP**で複製先の座標群を生成します。Point Generator POPはグリッド状や球面状など、さまざまな配置パターンで座標を生成することができます。続いて**Copy POP**を使って、その座標に元の3D形状をコピーします。最後にGeometry COMPに追加してレンダリングすることで、複数の形状が空間に配置された表現が完成します。

Copy POPによる複製は、POPのGPUベース処理の恩恵を最大限に受けることができます。SOPのCopy SOPと同様の表現がGPU上で高速に処理されるため、複製数が増えても描画負荷を大幅に抑えることができます。

## ポイントの属性（Attribute）の操作

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-changeAttribute-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/02_POP-changeAttribute.toe)

POPでは、Random POP・Pattern POP・Attribute POPなどを使用することで、ポイントごとに属性（Attribute）を個別に設定することができます。ポイントが持つ主な属性は以下の通りです。**P: Position**はポイントの位置座標、**N: Normal**は表面の法線ベクトル、**Color**はポイントの色とアルファ値、**Tex**はテクスチャ座標、**Weight**はポイントのウェイト値、**PointScale**はレンダリング時のポイントのスケール・大きさ、**LineWidth**は線の太さを表します。これらの属性を自在に制御することが、POPによる豊かな表現の鍵となります。属性の詳細については [TouchDesigner POP Attributes 101](https://youtu.be/CVPAA0CPvuA?si=_2L1Tvd4UEAIdsiO) も参考にしてください。

この例では、**Random POP**を使用して各ポイントにランダムなPoint Scale値を設定しています。これにより、Copy POPで複製された球のサイズがポイントごとにランダムに変化し、大小さまざまな球が空間に散りばめられた表現を実現しています。単一の操作で複数のインスタンスに異なる属性を割り当てられるのも、POPならではの強みです。

## ポイントの座標を動かす – アニメーション

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-noiseMove-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/03_POP-moveSphere.toe)

各ポイントのP（位置座標）を個別に動かすことで、複製された3Dオブジェクトをそれぞれ独立してアニメーションさせることができます。動きのパターンを生成するには、Noise POP・Pattern POP・Particle POPなどを組み合わせて使用します。各ポイントの座標を毎フレーム更新することで、オブジェクトが有機的に動き回るアニメーションが生まれます。

この例では**Noise POP**を使用しています。Noise POPは一般的なPerlin Noise（Simplex Noise）による滑らかなゆらぎだけでなく、流体の渦のような複雑な流れ場を生み出す**Curl Noise**にも対応しています。Curl Noiseを使うと、各ポイントが発散のない滑らかな経路をたどるため、液体や気体の流れに似た、より自然で有機的なアニメーションを作り出すことができます。

## ポイントの移動の軌跡を描く

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-sphereTrail-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/04_POP-trailSphere.toe)

**Trail POP**を用いることで、各ポイントが移動してきた軌跡を線として残すことができます。Trail POPはポイントの過去の位置を一定のフレーム数分だけ記録し、その履歴をラインデータとして出力します。描いた軌跡は**Line MAT**を適用することで、3D空間に浮かぶ線として描画することができます。

さらに、レンダリングしたテクスチャに**Bloom TOP**でグロー（光の滲み）エフェクトを付加することで、軌跡が淡く光り輝くような映像表現が生まれます。動くポイントが空間に光の軌跡を刻む、幻想的なビジュアルを実現することができます。

## ノイズで球を変形

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-noiseSphere-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/05_POP-noiseSphere.toe)

これまでは複製したオブジェクトを操作してきましたが、POPではレンダリング対象となるプリミティブ図形自体のポイント情報も自由に操作することができます。**Noise POP**を使うと、図形の頂点をPerlin Noise（Simplex Noise）によってランダムに変位させ、有機的な凹凸のある形状を生成できます。

Noise POPのパラメータで **Output > Combine Operation** を **「Translate along Normal」** に設定すると、各頂点を法線ベクトルの方向に沿って変位させることができます。これにより、球面が内側・外側に自然に膨らむような、より有機的な凹凸が生まれます。

ただし、頂点の位置を変形させると法線ベクトルの方向が崩れるため、変形後は法線を再計算して補正する必要があります。Noise POPの場合は **Output > Compute Point Normals** をONにすることで、変形後の形状に合わせた正しい法線ベクトルが自動的に計算されます。また、SOPと異なりGPU上で演算が行われるため、高解像度のメッシュに対しても処理負荷を大幅に抑えることができます。

## 変形した球の頂点に球を描画

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-SphereNoiseCopy-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/06_POP-noiseSphereCopy.toe)

前の例でノイズ変形させた球の各頂点を座標として利用し、そこに別の球を**Copy POP**で配置することができます。変形した球面の凹凸に沿って無数の小球が並ぶ、複雑な3D形状を生み出すことができます。

この手法は、SOPのジオメトリインスタンシングと同様の表現をPOPで実現するものです。SOPのインスタンシングと同様に複製数が増えても軽快に動作しますが、POPはGPUベースで計算するため処理負荷がさらに軽く、リアルタイム性能に優れています。変形アニメーションと組み合わせることで、球面が脈打ちながら表面の小球も連動して動くような、有機的な表現も可能です。

## 3Dの形状をねじって表現

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-torusTwist-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/07_POP-twistTorus.toe)

**Twist POP**を使用すると、3Dの形状を「ねじった」ような形状に変形させることができます。Twist POPは指定した軸を中心に、頂点の位置に応じて回転量を変化させながら形状全体を螺旋状に捻ります。トーラスや円柱など、軸対称の形状に適用すると特に印象的な結果が得られます。

Noise POPの場合と同様に、Twist POPで頂点の位置を変形させると法線ベクトルが正しくなくなるため、変形後の法線補正が必要です。この例では、Twist POPで変形した後に**Normal POP**を適用して法線ベクトルを再計算し、ライティングが正しく計算されるように補正しています。

## 変形した形状を複製して合成

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-torusTwistCopy-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/08_POP-twistTorusCopy.toe)

Twist POPで変形させた形状を、さらに**Copy POP**で複製することで、より複雑な3D表現が生まれます。複製したオブジェクトに対して移動・回転・スケールの変更などを加えることで、単純な図形の組み合わせとは思えないほど複雑で独自性のある形態が生成されます。

トーラス以外のプリミティブ形状にTwist POPを適用してみても、それぞれ異なる味わいの造形が得られます。さまざまな形状・捻りの強さ・複製のパターンを試しながら、オリジナルの3D表現を探ってみましょう。

## レンダリングを工夫する – PBR（物理ベースレンダリング）を試してみる

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-torusTwistPBR-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/09_POP-twistTorusPBR.toe)

PBR（物理ベースレンダリング）とは、現実世界における光の物理的な振る舞いをシミュレートし、3Dオブジェクトの質感や光の反射を極めてリアルに描画するコンピュータグラフィックスの技術です。金属・プラスチック・布など、素材ごとの光の吸収や反射の特性を物理的に正確に再現できるため、フォトリアルなレンダリングが可能になります。

TouchDesignerでは**PBR MAT**を使用することで物理ベースレンダリングが実現できます。先ほど作成したTwist POPで変形しCopy POPで複製したトーラスの複合体に、PBR MATを適用してレンダリングしてみましょう。メタリックな質感や表面の粗さ（Roughness）などのパラメータを調整することで、同じ形状でもまったく異なる印象の映像表現が得られます。

## POPの様々な属性を操作したアニメーション

<img src="https://yoppa.org/wp-content/uploads/2026/06/POP-instancing-scaled.jpg" width="640">

最後の実践的なサンプルとして、POPの様々な属性を組み合わせて操作することで、「画面の奥から大量の物体が回転しながら迫ってくる」本格的なアニメーションを制作します。これまで学んだ技術を総動員した集大成となるサンプルです。

制作の手順は次の通りです。まず**Point Generator POP**で元となるポイント群を生成します。次に**Random POP**でポイントごとに異なる色（Color）を割り当てて着色します。続いて**Pattern POP**でPointScaleにばらつきを持たせ、物体のサイズをランダムに変化させます。さらに**Random POP**と**Math POP**を組み合わせて各物体に回転を加えます。そして**Pattern POP**でP(0)・P(1)にランダム値を設定してx・y方向の位置をランダムに散らし、P(2)にはRampを設定することで手前に向かって迫ってくるz方向の動きを作り出します。

最後にGeometry COMP・Camera COMP・Light COMP・Render TOP・Phong MATを組み合わせてシーンをレンダリングし、**HSV Adjust TOP**で色調を補正した後、**Bloom TOP**でグローの輝きを加えれば完成です。POPの属性操作を駆使することで、このような本格的なアニメーションを効率よく制作することができます。