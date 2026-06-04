---
marp: true
paginate: true
---
<style>
section {
  font-family: 'Hiragino Sans W4';
  color: #444;
  font-size: 24px;
}
b, strong {
  font-family: 'Hiragino Sans W8';
}
ol {
  list-style-type: decimal;
}
h1, h2, h3, h4, h5, h6{
  font-family: 'Hiragino Sans W8';
  color: #2277cc;
}
pre, code {
  font-family: 'JetBrains Mono Slashed', 'Noto Sans JP';
  line-height: 1.3;
}
</style>

## メディアアート・プログラミング I
# TouchDesigner基本 6<br/> – POPで3Dオブジェクトを操作する

東京藝術大学芸術情報センター (AMC)
田所 淳

---

## 本日の内容

前回の POP の基礎を応用して、高度な3Dオブジェクト操作に挑戦します!

- 3D形状の複製
- ポイントの属性（Attribute）の操作
- ポイントの座標を動かす – アニメーション
- ポイントの移動の軌跡を描く
- ノイズで球を変形
- 変形した球の頂点に球を描画
- 3Dの形状をねじって表現
- 変形した形状を複製して合成
- PBR（物理ベースレンダリング）を試してみる
- POPの様々な属性を操作したアニメーション

---

## 1. 3D形状の複製

![height:400](https://yoppa.org/wp-content/uploads/2026/06/POP-pointCopy-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/01_POP-copySphere.toe)

---

**基本的な手順:**
1. Sphere POPなどプリミティブ図形で元の形状を用意
2. **Point Generator POP** で複製先の座標群を生成
   - グリッド状・球面状など、さまざまな配置パターンに対応
3. **Copy POP** で元の形状をその座標にコピー
4. Geometry COMPに追加してレンダリング

→ SOPのCopy SOPと同様の表現がGPU上で高速処理される

---

## 2. ポイントの属性（Attribute）の操作

![height:400](https://yoppa.org/wp-content/uploads/2026/06/POP-changeAttribute-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/02_POP-changeAttribute.toe)

---

Random POP・Pattern POP・Attribute POPなどでポイントごとに属性を個別設定できる

| 属性 | 内容 |
|---|---|
| **P (Position)** | ポイントの位置座標 |
| **N (Normal)** | 表面の法線ベクトル |
| **Color** | ポイントの色とアルファ値 |
| **Tex** | テクスチャ座標 |
| **PointScale** | レンダリング時のスケール・大きさ |
| **LineWidth** | 線の太さ |

参考: [TouchDesigner POP Attributes 101](https://youtu.be/CVPAA0CPvuA?si=_2L1Tvd4UEAIdsiO)

---

この例では **Random POP** で各ポイントにランダムな PointScale を設定

- Copy POPで複製された球のサイズがポイントごとにランダムに変化
- 単一の操作で複数インスタンスに異なる属性を割り当てられる

---

## 3. ポイントの座標を動かす – アニメーション

![height:400](https://yoppa.org/wp-content/uploads/2026/06/POP-noiseMove-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/03_POP-moveSphere.toe)

---

- 各ポイントのP（位置座標）を個別に更新することで、オブジェクトを独立してアニメーション
- Noise POP / Pattern POP / Particle POP などで動きのパターンを生成

**この例では Noise POP を使用:**
- **Perlin Noise（Simplex Noise）**: 滑らかなゆらぎ
- **Curl Noise**: 流体の渦のような複雑な流れ場
  - 発散のない滑らかな経路をたどるため、液体・気体の流れに近い有機的な動きになる

---

## 4. ポイントの移動の軌跡を描く

![height:400](https://yoppa.org/wp-content/uploads/2026/06/POP-sphereTrail-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/04_POP-trailSphere.toe)

---

- **Trail POP**: ポイントの過去の位置を一定フレーム数記録し、ラインデータとして出力
- **Line MAT** で軌跡を3D空間の線として描画
- レンダリング後に **Bloom TOP** でグロー（光の滲み）エフェクトを付加
  - → 光の軌跡が空間に刻まれる幻想的なビジュアルが生まれる

---

## 5. ノイズで球を変形

![height:400](https://yoppa.org/wp-content/uploads/2026/06/POP-noiseSphere-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/05_POP-noiseSphere.toe)

---

- **Noise POP** で図形の頂点をPerlin Noise（Simplex Noise）でランダムに変位させる
- **Output > Combine Operation** を **「Translate along Normal」** に設定
  - → 法線ベクトルの方向に沿って変位 → より自然な有機的な凹凸が生まれる

**変形後の法線補正が必要:**
- Noise POPの **Output > Compute Point Normals** をONにする
  - → 変形後の形状に合った正しい法線ベクトルが自動計算される
- SOPと異なりGPU上で演算 → 高解像度メッシュでも処理負荷が軽い

---

## 6. 変形した球の頂点に球を描画

![height:400](https://yoppa.org/wp-content/uploads/2026/06/POP-SphereNoiseCopy-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/06_POP-noiseSphereCopy.toe)

---

- ノイズ変形させた球の各頂点を座標として利用し、**Copy POP** で別の球を配置
- 変形した球面の凹凸に沿って無数の小球が並ぶ複雑な3D形状を生成
- SOPのジオメトリインスタンシングと同様の表現をGPUベースで実現
  - → 複製数が増えても処理負荷が軽く、リアルタイム性能に優れる
- 変形アニメーションと組み合わせると球面と表面の小球が連動して動く

---

## 7. 3Dの形状をねじって表現

![height:400](https://yoppa.org/wp-content/uploads/2026/06/POP-torusTwist-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/07_POP-twistTorus.toe)

---

- **Twist POP**: 指定した軸を中心に、頂点位置に応じて回転量を変化させ形状を螺旋状に捻る
  - トーラスや円柱など軸対称の形状に適用すると特に印象的
- 変形後は法線ベクトルが崩れるため **法線補正が必要**
  - Twist POP の後に **Normal POP** を適用して法線を再計算
  - → ライティングが正しく計算されるようになる

---

## 8. 変形した形状を複製して合成

![height:400](https://yoppa.org/wp-content/uploads/2026/06/POP-torusTwistCopy-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/08_POP-twistTorusCopy.toe)

---

- Twist POPで変形した形状を **Copy POP** でさらに複製
- 移動・回転・スケールの変更を組み合わせることで複雑な形態が生成される
- Torus以外のプリミティブ形状でも試してみよう
  - 形状・捻りの強さ・複製パターンを変えてオリジナルの3D表現を探ろう!

---

## 9. PBR（物理ベースレンダリング）を試してみる

![height:400](https://yoppa.org/wp-content/uploads/2026/06/POP-torusTwistPBR-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/09_POP-twistTorusPBR.toe)

---

**PBR（Physical Based Rendering）とは:**
- 現実世界の光の物理的な振る舞いをシミュレートする技術
- 金属・プラスチック・布など素材ごとの光の吸収・反射をリアルに再現
- → フォトリアルなレンダリングが可能

**TouchDesignerでの使用方法:**
- **PBR MAT** を使用するだけで物理ベースレンダリングを適用可能
- Twist+Copyしたトーラスにそのまま適用してみよう
- **Metallic**（金属感）や **Roughness**（表面の粗さ）を調整
  - → 同じ形状でもまったく異なる質感・印象の映像表現が得られる

---

## 10. POPの様々な属性を操作したアニメーション

![height:400](https://yoppa.org/wp-content/uploads/2026/06/POP-instancing-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/07/10_POP-instancing.toe)

---

これまで学んだ属性操作を総動員した集大成サンプル
「画面の奥から大量の物体が回転しながら迫ってくる」アニメーション

**制作手順:**
1. **Point Generator POP** で元となるポイント群を生成
2. **Random POP** でポイントごとに色（Color）を割り当てて着色
3. **Pattern POP** で PointScale にばらつき → 物体のサイズをランダムに変化
4. **Random POP + Math POP** を組み合わせて各物体に回転を加える
5. **Pattern POP** で P(0)・P(1) にランダム値（x・y位置をランダムに散らす）、P(2) にRamp（奥から手前へ迫る動き）を設定

---

6. Geometry COMP・Camera COMP・Light COMP・Render TOP・**Phong MAT** でシーンをレンダリング
7. **HSV Adjust TOP** で色調補正
8. **Bloom TOP** でグローの輝きを追加 → 完成!

→ POPの属性操作を組み合わせることで、本格的なアニメーションが効率よく制作できる
