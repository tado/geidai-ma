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
# TouchDesigner基本 3<br/> - SOPの基本

東京藝術大学芸術情報センター (AMC)
田所 淳

---

## 本日の内容

SOP (Surface Operators) の基本的な使い方を学びます!

- SOPの基本
- プリミティブ図形の作成
- レンダリングの基本
- カメラとライトの設定
- プロシージャルモデリングの基本

---

## SOPの基本

- SOP = Surface Operators
- 3Dジオメトリを生成、編集、操作するためのオペレーターファミリー
  - ジオメトリ: 3D空間における形状や構造を表すデータ
- 点、線、ポリゴンメッシュ、NURBSカーブ/サーフェス、メタボールなど、さまざまなタイプの3Dデータを扱うことができる

---

## 参考: SOP から POP へ — 新たなファミリーへの移行

- SOP(Surface Operators) は 1995 年設計の TouchDesigner 最古の演算子ファミリー
- CPU 前提の設計 → 大規模点群・複雑なパーティクルのリアルタイム処理に限界
- 2025 年の Official リリースで **POP (Point Operators)** を正式投入
  - DAT (2005 年) 以来、約 20 年ぶりのファミリー追加

---

- POP は GPU 上で動作 → SOP より飛躍的に高いパフォーマンス
- SOP・CHOP・TOP の利点を統合
  - シェーダーを書かずにノードベースで GPU データを処理可能
- Derivative は POP を SOP の「**置き換えであり再考**」と公式に位置付け
- 長期的には POP が 3D 処理の主軸になる見通し
- NURBS やベジェ曲面など SOP 固有の機能はまだ POP ではカバーされておらず、当面は両者が共存
- POP については、別の回で詳細に紹介

---

## プリミティブ図形の作成

SOPの基本的な使い方を学ぶために、まずはプリミティブ図形 (基本図形) を作成してみましょう

![height:400](https://yoppa.org/wp-content/uploads/2025/04/Screenshot-2025-04-30-064325-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04/01_sop_primitives.toe)

---

2D
- Line SOP: 直線を作成
- Circle SOP: 円を作成
- Rectangle SOP: 長方形を作成
- Grid SOP: 格子状の平面を作成

3D
- Box SOP: 立方体を作成
- Sphere SOP: 球を作成
- Tube SOP: 円筒を作成
- Torus SOP: トーラスを作成

---

## SOPの操作

SOPのオペレータ上で様々な操作を行うことができます

![height:400](https://yoppa.org/wp-content/uploads/2025/04/Screenshot-2025-04-30-070619-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04-02_sop_manipurate.toe)

---

- SOPを選択して、パラメータウィンドウで設定を変更
- ビューワーアクティブにして、マウスで操作
- 表示の切り替え
  - ワイヤーフレーム表示
  - 頂点の表示
  - 法線の表示 など

---

## SOPのレンダリング

- SOPを配置は3Dジオメトリの形状を作成しただけ
- 最終的に画像として表示するためには、Render TOPを使用してレンダリングを行う必要がある
- Render TOPは、3Dシーンを2D画像としてレンダリングするためのコンポーネント
- 3Dを2D画像としてレンダリングするために必要な3つの要素
  1. カメラ: 3Dシーンを撮影するための視点
  2. ライト: 3Dシーンを照らすための光源
  3. マテリアル: 3Dジオメトリの見た目を定義するための素材

実際にオペレーターを接続しながら解説していきます!

---

### SOPのレンダリング基本

![height:400](https://yoppa.org/wp-content/uploads/2026/05/sop-render-scaled.jpg)
  
[ダウンロード](https://github.com/tado/tdexamples/blob/main/04-03_sop_render.toe)

---

- SOPでプリミティブ図形を作成
- Geometry COMPを作成し、SOPを接続
- Camera COMPとLight COMPを配置
- Render TOPを作成し、Camera COMPとLight COMPが自動的に接続される

---

### 参考: POPのレンダリング

![height:400](https://yoppa.org/wp-content/uploads/2026/05/pop-render-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04/04_pop_render.toe)

---

- POPでもSOPど同様に3Dプリミティブのレンダリングが可能
- SOPの機能の一部は徐々にPOPに移行していく予定
- POPについては、別の回で詳細にとりあげます

---

### SOPにマテリアルの適用 + 回転

![height:400](https://yoppa.org/wp-content/uploads/2026/05/sop-material-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04/05_sop_material.toe)

---

- Phone MATを作成し、Geometry COMPに接続
- Render TOPに接続されたGeometry COMPのマテリアルを変更することで、ジオメトリの見た目を変更できる

---

### SOPのレンダリング - 様々なパラメータを調整

![height:400](https://yoppa.org/wp-content/uploads/2026/05/sop-materal2-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04/06_sop_transform.toe)

---

様々なパラメータを調整して、3Dジオメトリの見た目を変更してみよう!
- カメラの画角や位置
- マテリアルの色や質感
- ライトの位置や強度
- SOPの形状やサイズ など

---

## 応用: プロシージャルモデリング

- SOPを使って、プロシージャルに3Dジオメトリを生成することができる
- プロシージャルモデリングとは、数式やアルゴリズムを使って自動的に3D形状を生成する手法
- SOPのパラメータを数式やCHOPのデータにリンクさせることで、動的に変化するジオメトリを作成できる

---

### SOPのMergeとCopyとTransformでモデリング

![height:400](https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-01-145105-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04/07_sop_copy.toe)

---

- Merge SOPを使って、複数のジオメトリを結合
- Copy SOPを使って、ジオメトリを複製
- 複製の際に位置や回転やスケールを指定することができる 
  - Copy SOPのTransformタブで設定
- Transform SOPを使って、複製したジオメトリの位置や回転を変更
- いろいろ試してみましょう!

---

### Sweep SOPで面を回転しながら押し出して整形

![height:400](https://yoppa.org/wp-content/uploads/2026/05/sop-sweep-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04/08_sop_sweep.toe)

---

- Sweep SOPを使って、形態を指定したパスに沿って回転しながら押し出す
- Sweep SOPのoutputで、**Skin Output**を**On**に
- Convert SOPで、Convert to **Polygon**を指定
- Facet SOPで**Compute Normal**を**On**に
- あとは、これまでと同様にレンダリングしてTOPに

---

### より複雑なプロシージャルモデリング

![height:400](https://yoppa.org/wp-content/uploads/2025/05/Screenshot-2025-05-01-160525-scaled.jpg)

[ダウンロード](https://github.com/tado/tdexamples/blob/main/04/09_sop_complex.toe)

---

- 様々なSOPを組み合わせて、より複雑なプロシージャルモデリングに挑戦!
- Line SOPとSweep SOPを組み合わせて、複雑な形状を作成
- Line SOPをPattern SOPで変形させて波打つような形状へ
- Coyp SOPを使って複製していく

---

## 実習: プロシージャルモデリングに挑戦

ここまでの内容を踏まえて、Copy SOPやTransform SOPを使って、プロシージャルモデリングに挑戦してみましょう!

- 例えば、以下のような形状に挑戦してみてください
  - 複数の円柱を並べた形状
  - 波打つような形状
  - 複雑な幾何学模様

---

いくつか応用的なサンプルを作成しました。参考にしてください。

- [応用サンプル 1](https://github.com/tado/tdexamples/blob/main/04/10_sop_procedual3D.toe)
- [応用サンプル 2](https://github.com/tado/tdexamples/blob/main/04/11_sop_uneune.toe)
- [応用サンプル 3](https://github.com/tado/tdexamples/blob/main/04/12_sop_uneNoise.toe)
