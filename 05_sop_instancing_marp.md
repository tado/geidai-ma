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
# TouchDesigner基本 4<br/> - ジオメトリのインスタンシング

東京藝術大学芸術情報センター (AMC)
田所 淳

---

## 本日の内容

大量オブジェクトを GPU で効率的に描画する「ジオメトリインスタンシング」を学びます!

- ジオメトリインスタンシングとは
- CHOPによるインスタンシング
- SOPによるインスタンシング
- TOPによるインスタンシング

---

## ジオメトリインスタンシングとは

- 1つの元となるジオメトリをGPU上で効率的に複製し、大量に描画する機能
- Geometry COMPの「Instance」ページで設定
- **Copy SOPとの違い**:
  - Copy SOP: CPUで処理 → 大量複製は重くなる
  - インスタンシング: GPUで処理 → 大量複製でも高速

---

**メリット:**
- パフォーマンス向上: CPUではなくGPUで処理するため高速
- メモリ効率: 元データは1つで済むためメモリ使用量を抑制

**基本的な仕組み:**
1. Geometry COMPに元ジオメトリ (SOP) を接続
2. 「Instance」タブをオンにする
3. 「Instance OP」にデータを持つオペレータ (CHOP / TOP / SOP / DAT) を指定

---

## Instanceタブの設定

![height:400](https://yoppa.org/wp-content/uploads/2025/11/Screenshot-2025-05-06-165945.jpg)

---

**利用可能なデータソース:**

| オペレータ | 用途 |
|---|---|
| CHOP | チャンネルデータ（サンプル数 = インスタンス数） |
| TOP | ピクセルデータ（RGB値 = 座標や色） |
| SOP | ポイント座標・アトリビュート |
| DAT | テーブルデータ |

---

**制御可能なパラメータ:**
- **Translate** (tx, ty, tz): 各インスタンスの位置
- **Rotate** (rx, ry, rz): 回転
- **Scale** (sx, sy, sz): スケール
- **Color** (colorr, colorg, colorb, colora): 色と透明度
- Pivot: 回転・スケールの基点
- Texture Coordinate
- Custom Attributes

---

## 1. CHOPによるインスタンシング – 位置制御

![height:400](https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-06-165133.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/05/01_instancing-basic.toe)

---

- Sphere SOPを作成し、Geometry COMPに接続
- Geometry COMPのInstanceタブをオンにする
- Pattern CHOPを作成し、Instance OPに指定
  - Ramp: x軸方向にインスタンスを並べる
  - Sine: y軸方向に波状に並べる

---

## 2. CHOPによるインスタンシング – ランダム配置

![height:400](https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-06-170829.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/05/02_instancing-random.toe)

---

- Pattern CHOPで **Random** を選択し、tx / ty / tz に設定
- 大量のオブジェクトを3次元空間にランダム配置
- Pattern CHOPのサンプル数 = インスタンスの数

---

## 3. SOPによるインスタンシング – 位置制御

![height:400](https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-06-171543.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/05/03_instancing-sop.toe)

---

- SOPのままではインスタンシングに直接使用できない
- **SOP to CHOP** でSOPのデータをCHOPに変換して使用
- 頂点の座標がインスタンスの **Translate** (tx, ty, tz) に対応
- SOPの頂点ごとにインスタンスが1つ生成される

---

## 4. SOPによるインスタンシング応用 – ノイズ変形

![height:400](https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-06-173008.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/05/04_instancing-sop-noise.toe)

---

- Noise SOPを使って頂点位置を変形させる
- 変形後の頂点座標をSOP to CHOPで変換してInstanceに指定
- Phong MATを適用してマテリアルを設定
- ノイズの時間変化で動的なアニメーションを生成

---

## 5. TOPによるインスタンス着色

![height:400](https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-06-174012.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/05/05_instancing-noise-color.toe)

---

- Noise TOPを使ってインスタンスの色を制御
- **TOP to CHOP** でTOPデータをCHOPに変換
- **重要**: インスタンス数とTOPのピクセル数を一致させる必要がある
  - インスタンス数: 2048個
  - Noise TOPのサイズ: 2048 × 1
- TOPのRGB値がインスタンスの Color (colorr, colorg, colorb) に対応

---

## 6. TOPによるインスタンシング – 位置制御アニメーション

![height:400](https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-06-182812.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/05/06_instancing-noise-top.toe)

---

- TOPのRGB値をインスタンスの座標 (x, y, z) に対応させる
- フォーマットは **32-bit float (rgba)** を使用（精度が高いため）
- Noise TOPのノイズ変化で座標が動的にアニメーション
- インスタンス数とTOPのピクセル数を一致させる（例: 2048 × 1）

---

## 7. TOP + CHOPによるハイブリッドアニメーション

![height:400](https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-06-184044.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/05/07_instancing-ramp-top.toe)

---

- Noise TOP（x, y 座標制御）+ Pattern CHOP Ramp（z 座標制御）を組み合わせる
- TOPとCHOPを **Merge CHOP** で結合してInstance OPに指定
- カメラに向かって降下するようなエフェクトを実現
- 複数のデータソースを組み合わせることで表現の幅が広がる

---

## 8. 3Dノイズによる応用アニメーション

![height:400](https://yoppa.org/wp-content/uploads/2025/11/Screenshot-2025-11-11-074505.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/05/08_instancing-3Dnoise.toe)

---

- 3Dノイズの情報を位置・色・角度にそれぞれ対応させる
- 複数のNoise TOPを使い分けて各パラメータを個別に制御
- 動的で複雑な3Dアニメーションをインスタンシングで実現
- いろいろ組み合わせてオリジナルの表現を探ってみよう!
