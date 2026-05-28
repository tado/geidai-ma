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
# TouchDesigner基本 5<br/> – POP入門

東京藝術大学芸術情報センター (AMC)
田所 淳

---

## 本日の内容

GPU で高速処理される新しいオペレーターファミリー「POP」の基礎を学びます!

- POPとは?
- POPでできること
- POPについて学ぶ
- POPの概要
- ポイントの属性（attribute）の値を表示
- POPのアトリビュート（属性）を使用する
- POPにおける色の扱い
- POPを使用したパーティクルプログラミング

---

## POPとは?

![height:400](https://yoppa.org/wp-content/uploads/2026/05/pop-forgeteverything-scaled.jpg)

[What Are POPs? The New TouchDesigner Operator Family That Changes Everything](https://youtu.be/xCmn625J-vA?si=T8Y2wdXKvYgyOa6H)

---

## POPとは?

- 2005年のDAT導入以来となる、TouchDesignerの**新しいオペレーターファミリー**
- **GPUベース**で3Dジオメトリや数値データを生成・編集
  - 従来のSOP（CPUベース）と比べ、大量データのリアルタイム処理が大幅に高速化
- 対象とするデータ: ポイント、ポリゴン、ポイントクラウド、パーティクル、ラインストリップ

---

## POPのデータ構造

- POP内のデータはポイントリスト・頂点（Vertex）リスト・プリミティブリストで構成
- 各ポイントが持つアトリビュート:
  - 位置 **P**、法線 **N**、色 **Color** など
  - カスタムアトリビュートも追加可能

**オペレーターの主な分類:**
| 種類 | 概要 |
|---|---|
| Generators系 | 外部データなしでPOP内からポイントを生成 |
| Filters系 | Transform・Noiseなどで位置や属性を加工 |
| to POP系 | TOP・CHOP・SOPのデータをPOP形式に変換 |

---

## POPの用途

- Render TOPでのレンダリングやインスタンシングのソースとして利用可能
- DMX照明・LEDアレイ・レーザーなど外部デバイスへの数値データ出力にも対応

[Intro to POPs: The New Operator Family in TouchDesigner](https://www.youtube.com/watch?v=bWfUk6MF8B0)

---

## POPでできること

![height:400](https://yoppa.org/wp-content/uploads/2026/05/popGuide-scaled.jpg)

[POPs Examples Package をダウンロード](https://www.dropbox.com/scl/fo/dvvqnl61dgmicxoebl4sy/AFuNixO4WWcAbyM5KkUi9F4?rlkey=f152v4uuzou81c7477w1yf6im&st=zzro8oie&dl=0) → **POPGuide/POPGuide.toe** を開いてみよう

---

## POPについて学ぶ

![height:400](https://learn.derivative.ca/wp-content/uploads/2025/11/109_10-cover.png)

[109 – POPs: Working with Points](https://learn.derivative.ca/courses/100-fundamentals/lessons/109-pops-working-with-points/)

Derivative社の公式チュートリアル — 今回の講義はこれを軸に進めます

---

## POPの概要

- POPはGPUで並列処理 → CPUベースのSOPに代わる高速・最新の操作手法
- データの核は**「ポイント」**: 位置（P）、色（Color）、法線（N）などの属性を持つ
- **「プリミティブ」**: ポイントを接続する構造体（点・線・三角形・四角形など）
- プリミティブは**頂点（バーテックス）**で構成され、プリミティブや頂点自体にも属性を持たせることが可能
- 粒子システム・点群・ポリゴンなど多様な3D形状をGPUアクセラレーションで生成・編集できる

---

## POPの基本 – ポイントの属性の値を表示

![height:400](https://yoppa.org/wp-content/uploads/2026/05/pop-showinfo-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/06/01_pop-showinfo.toe)

---

- POPにもSOPと同様に直線・円・球などの基本図形が用意されている
- **POP to CHOP** を使うと各頂点の情報をテーブル形式で確認できる

**確認できる情報:**
| アトリビュート | 内容 |
|---|---|
| Index | 頂点番号 |
| P(0), P(1), P(2) | 頂点の座標 (x, y, z) |
| N(0), N(1), N(2) | 法線ベクトル (x, y, z) |
| Tex(0), Tex(1), Tex(2) | テクスチャ座標 (u, v, w) |

---

## POPのアトリビュート（属性）を使用する

- アトリビュートはPOPの基本的なデータ要素
- すべてのポイントは位置アトリビュート **「P」** を持つ（P(0)=x, P(1)=y, P(2)=z）

**標準アトリビュート:** 位置（P）、色（Color）、法線（Normal）、テクスチャ（Tex）

**共通アトリビュート:** PointScale、LineWidth、Speed、Weight など

---

**追加アトリビュートの付与方法:**
- **Attribute POP** を使用する
- 一部のPOPにある **「New Attribute」** パラメータを使用する
- 一部のPOPにある **「Output Attribute Scope」** パラメータを使用する

---

## POPのアトリビュート – サンプル

![height:400](https://yoppa.org/wp-content/uploads/2026/05/pop-addAttribute-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/06/02_pop-addAttribute.toe)

---

- Circle POPで円形の頂点を用意
- Pattern POPで頂点の大きさ（**PointSize**）を規則的に変化
- もう一つのPattern POPで色（**Color.rgb**）を規則的に変化
- マテリアルを **Line MAT** に設定してポイントスプライトを描画
- → 頂点のサイズと色が変化するアニメーション

---

## POPにおける色の扱い

- **「Color」属性**（R・G・B・A の4成分）でポイントごとに色を制御
- Attribute POP / Pattern POPで全体への一括適用やグラデーションが可能
- **Lookup Texture POP**: UV座標（Tex(0), Tex(1)）に基づきテクスチャから色を割り当て
- **Normalize POP**: 位置などの数値を 0〜1 に変換してから色に反映させると効果的

---

## POPにおける色の扱い – テクスチャから色を適用

![height:400](https://yoppa.org/wp-content/uploads/2026/05/pop-lookupColor-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/06/03_pop-lookupColor.toe)

Movie File In のテクスチャRGBを **Lookup Texture POP** で Grid POP の各頂点に適用

---

## POPにおける色の扱い – Noise POPでアニメーション

![height:400](https://yoppa.org/wp-content/uploads/2026/05/pop-lookupColorNoise-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/06/04_pop-lookupColorNoise.toe)

**Noise POP** を追加して頂点の位置をダイナミックに動かすアニメーション

---

## POPを使用したパーティクルプログラミング

- **Particle POP**: GPUアクセラレーションで数百万のパーティクルを高速シミュレーション
- フィードバックループで複雑な物理挙動を実現

**基本的な構成:**
1. Particle POPの **「Target POP」** に Null POP を指定
2. Particle POP と Null POP の間にオペレーターを挟む
3. → 演算結果が次のフレームへ引き継がれるループが成立

---

**フィードバックループのポイント:**
- 放射設定: 定数レートだけでなく属性マッピングで初期状態・質量・抵抗を制御
- 力（**PartForce**）や速度減衰を累積 → 動的で複雑なパーティクル表現

---

## 5. パーティクル – 雨のアニメーション

![height:400](https://yoppa.org/wp-content/uploads/2026/05/pop-particleRain-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/06/05_pop-particleRain.toe)

---

- Grid POPの頂点をParticle POPで上から下へ落下させる
- **Noise POP** を組み合わせて落下の動きにゆらぎを持たせる

---

## 6. パーティクル – 球面からの放射

![height:400](https://yoppa.org/wp-content/uploads/2026/05/pop-particleSphere-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/06/06_pop-particleSphere.toe)

---

- **Sphere POP** からパーティクルを発生させる
- 初期速度を **Sphere POPの法線ベクトル** に設定するのがポイント
- → 球面から外側に向かってパーティクルが放射するエフェクト
