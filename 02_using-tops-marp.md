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
# TOPによるテクスチャーを操作

東京藝術大学芸術情報センター (AMC)
田所 淳

---

## 本日の内容

- Top (Texture Operator) を使用して画像 (テクスチャー) の操作、合成、画像効果などを学びます
- まず前回の内容を復習してから、徐々に高度な内容へと発展させます

---

## 前回の復習 - バナナを回す

実際にプログラムを作成しながら復習していきましょう!

![height:420](https://github.com/tado/geidai-ma/blob/main/img/rotate-banana.jpg?raw=true)
- [サンプルファイル](https://github.com/tado/tdexamples/blob/main/01/01_rotateBanana01.toe)

---

## 応用 1: 文字を重ねる

回転するバナナの上に文字を重ねてみましょう

1. まずText TOPで文字を配置したTOPを作成
2. 回転するバナナと文字をOver TOPで合成

※ ポイント! 合成するTOPの解像度 (縦横のサイズ) を統一する!

---

## 応用 2: タイリング

画像を縮小してタイル状に並べてみる、方法は2つ

1. Title TOPを使用する
2. Transform TOPで縮小してタイリング

それぞれの方法を実際に作成しながら試してみましょう

---

## 応用 3: 画像処理

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

## 応用 4: フィードバック

画像をフィードバックさせながら軌跡を描いていく

- Feedback TOP、Level TOP、Over TOPを組み合わせ
- パッチの接続はちょっと複雑
- 実際に作成しながら解説していきます!

---

## 実習!

ここまでの内容を踏まえて、TOPでいろいろ試してみましょう!

- 読み込む画像を変更
- LFOで回転するだけでなく、サイズの変更や位置の移動など
- 新たな画像処理を試してみる
- 画像処理を複数適用してみる
...etc.

