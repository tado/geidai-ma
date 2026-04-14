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
  font-family: 'Hiragino Sans W7';
}
ol {
  list-style-type: decimal;
}
h1, h2, h3, h4, h5, h6{
  font-family: 'Hiragino Sans W7';
  color: #2277cc;
}
pre, code {
  font-family: 'JetBrains Mono Slashed', 'Noto Sans JP';
  line-height: 1.3;
}
</style>

## メディアアート・プログラミング I
# TOPによるテクスチャー操作

東京藝術大学芸術情報センター (AMC)
田所 淳

---

## 本日の内容

本日は、Top (Texture Operator) を使用して画像 (テクスチャー) の操作、合成、画像効果などを学びます。まず前回の内容を復習してから、徐々に高度な内容へと発展させます。最後にNoise TOPを素材にして、独自の表現に挑戦します!

---

## TOPについて

Texture Operators（テクスチャ・オペレーター）、通称 TOPs は、リアルタイムでGPUベースのコンポジット（合成）や画像処理を行うイメージ・オペレーターです。TOPsのすべての計算はシステムのGPU上で行われます。TOPsは、テクスチャの準備、画像や動画ストリームの合成、コントロールパネルの要素作成、32ビット浮動小数点データの操作、その他ほぼあらゆる画像関連のタスクに使用できます。また、TOPsは多くのフォーマットをサポートしており、ハイダイナミックレンジ（HDR）画像を扱うための浮動小数点画像フォーマットにも対応しています。

すべてのレンダリングとコンポジット（合成）は、TOPsを使用してオフスクリーンで行われます。データは任意の解像度にスケーリング可能で、制限はグラフィックRAMの容量とグラフィックカードの最大解像度のみです。

注：TouchDesignerの非商用版（Non-Commercial）は、解像度が1280x1280までに制限されています。

---

## 前回の復習 - バナナを回す

実際にプログラムを作成しながら復習していきましょう!

![height:400](https://github.com/tado/geidai-ma/blob/main/img/rotate-banana.png?raw=true)
[サンプルファイル](https://github.com/tado/tdexamples/blob/main/01/01_rotateBanana.toe)

---

## バナナを回す応用 - TOPを組み合わせてみる

このバナナを回すプログラムを応用して、いろいろTOPについて学んでいきましょう。以下の順番に基本から徐々に高度な応用へと進んでいきます。

- 応用 1: 文字を重ねる応用 1: 文字を重ねる
- 応用 2: タイリング
- 応用 3: 様々な画像処理
- 応用 4: フィードバック

---

### 応用 1: 文字を重ねる応用 1: 文字を重ねる

回転するバナナの上に文字を重ねてみましょう

1. まずText TOPで文字を配置したTOPを作成
2. 回転するバナナと文字をOver TOPで合成

※ ポイント! 合成するTOPの解像度 (縦横のサイズ) を統一する!

---

![height:500](https://github.com/tado/geidai-ma/blob/main/img/rotate-banana-over.png?raw=true)
[サンプルファイル](https://github.com/tado/tdexamples/blob/main/01/02_rotateBanana-over.toe)

---

### 応用 2: タイリング
画像を縮小してタイル状に並べてみる、方法は2つ

1. Title TOPを使用する
2. Transform TOPで縮小してタイリング

それぞれの方法を実際に作成しながら試してみましょう

---

![height:500](https://github.com/tado/geidai-ma/blob/main/img/rotate-banana-tile.png?raw=true)
[サンプルファイル](https://github.com/tado/tdexamples/blob/main/01/03_rotateBanana-tile.toe)


---

### 応用 3: 様々な画像処理

TOPにはPhotshopのようなテクスチャにリアルタイムに画像処理を行うことができる
いろいろ試してみましょう!

- Level TOP: レベルやコントラストの補正や透明度の変更など
- HSV adjust TOP: 色相、明度、彩度を補正
- Mono TOP: モノクローム化
- Edge TOP: 輪郭抽出
- Blur TOP: ぼかし
- Bloom TOP: 輝くような効果

画像処理したTOPをSwitch TOPで切り替えてみる!

---

![height:500](https://github.com/tado/geidai-ma/blob/main/img/rotate-banana-filter.png?raw=true)
[サンプルファイル](https://github.com/tado/tdexamples/blob/main/01/04_imageProcessing.toe)

---

### 応用 4: フィードバック

画像をフィードバックさせながら軌跡を描いていく

- Feedback TOP、Level TOP、Over TOPを組み合わせ
- パッチの接続はちょっと複雑
- 実際に作成しながら解説していきます!

---

![height:500](https://github.com/tado/geidai-ma/raw/main/img/rotate-banana-feedback.png?raw=true)
[サンプルファイル](https://github.com/tado/tdexamples/blob/main/01/05_feedback.toe)

---

## 実習!

ここまでの内容を踏まえて、TOPでいろいろ試してみましょう!

- 読み込む画像を変更
- LFOで回転するだけでなく、サイズの変更や位置の移動など
- 新たな画像処理を試してみる
- 画像処理を複数適用してみる
...etc.

