# TOPによるテクスチャー操作

<img src="https://i0.wp.com/yoppa.org/wp-content/uploads/2025/04/Screenshot-2025-04-17-062659-scaled.jpg?ssl=1" width="640">



## 本日の内容

本日は、Top (Texture Operator) を使用して画像 (テクスチャー) の操作、合成、画像効果などを学びます。まず前回の内容を復習してから、徐々に高度な内容へと発展させます。最後にNoise TOPを素材にして、独自の表現に挑戦します!



## TOPについて

Texture Operators（テクスチャ・オペレーター）、通称 TOPs は、リアルタイムでGPUベースのコンポジット（合成）や画像処理を行うイメージ・オペレーターです。TOPsのすべての計算はシステムのGPU上で行われます。TOPsは、テクスチャの準備、画像や動画ストリームの合成、コントロールパネルの要素作成、32ビット浮動小数点データの操作、その他ほぼあらゆる画像関連のタスクに使用できます。また、TOPsは多くのフォーマットをサポートしており、ハイダイナミックレンジ（HDR）画像を扱うための浮動小数点画像フォーマットにも対応しています。

すべてのレンダリングとコンポジット（合成）は、TOPsを使用してオフスクリーンで行われます。データは任意の解像度にスケーリング可能で、制限はグラフィックRAMの容量とグラフィックカードの最大解像度のみです。

注：TouchDesignerの非商用版（Non-Commercial）は、解像度が1280x1280までに制限されています。

## CPU vs. GPU

CPUとGPUによる描画の違いをデモンストレーション。

[動画](https://www.youtube.com/watch?v=WmW6SD-EHVY)  
<img src="https://i.ytimg.com/vi/WmW6SD-EHVY/maxresdefault.jpg" width="640">


## 前回の復習 - バナナを回す

実際にプログラムを作成しながら復習していきましょう!

<img src="https://github.com/tado/geidai-ma/blob/main/img/rotate-banana.png?raw=true" width="640">

[サンプルファイル](https://github.com/tado/tdexamples/blob/main/01/01_rotateBanana.toe)



## バナナを回す応用 - TOPを組み合わせてみる

このバナナを回すプログラムを応用して、いろいろTOPについて学んでいきましょう。以下の順番に基本から徐々に高度な応用へと進んでいきます。

- 応用 1: 文字を重ねる
- 応用 2: タイリング
- 応用 3: 様々な画像処理
- 応用 4: フィードバック



### 応用 1: 文字を重ねる

回転するバナナの上に文字を重ねてみましょう

1. まずText TOPで文字を配置したTOPを作成
2. 回転するバナナと文字をOver TOPで合成

※ ポイント! 合成するTOPの解像度 (縦横のサイズ) を統一する!



<img src="https://github.com/tado/geidai-ma/blob/main/img/rotate-banana-over.png?raw=true" width="640">

[サンプルファイル](https://github.com/tado/tdexamples/blob/main/01/02_rotateBanana-over.toe)



### 応用 2: タイリング
画像を縮小してタイル状に並べてみる、方法は2つ

1. Tile TOPを使用する
2. Transform TOPで縮小してタイリング

それぞれの方法を実際に作成しながら試してみましょう



<img src="https://github.com/tado/geidai-ma/blob/main/img/rotate-banana-tile.png?raw=true" width="640">

[サンプルファイル](https://github.com/tado/tdexamples/blob/main/01/03_rotateBanana-tile.toe)




### 応用 3: 様々な画像処理

TOPでは、Photoshopのようにテクスチャに対してリアルタイムで画像処理を行うことができます
いろいろ試してみましょう!

- Level TOP: レベルやコントラストの補正や透明度の変更など
- HSV adjust TOP: 色相、明度、彩度を補正
- Mono TOP: モノクローム化
- Edge TOP: 輪郭抽出
- Blur TOP: ぼかし
- Bloom TOP: 輝くような効果

画像処理したTOPをSwitch TOPで切り替えてみる!



<img src="https://github.com/tado/geidai-ma/blob/main/img/rotate-banana-filter.png?raw=true" width="640">

[サンプルファイル](https://github.com/tado/tdexamples/blob/main/01/04_imageProcessing.toe)



### 応用 4: フィードバック

画像をフィードバックさせながら軌跡を描いていく

- Feedback TOP、Level TOP、Over TOPを組み合わせ
- パッチの接続はちょっと複雑
- 実際に作成しながら解説していきます!



<img src="https://github.com/tado/geidai-ma/raw/main/img/rotate-banana-feedback.png?raw=true" width="640">

[サンプルファイル](https://github.com/tado/tdexamples/blob/main/01/05_feedback.toe)



## 実習!

ここまでの内容を踏まえて、TOPでいろいろ試してみましょう!

- 読み込む画像を変更
- LFOで回転するだけでなく、サイズの変更や位置の移動など
- 新たな画像処理を試してみる
- 画像処理を複数適用してみる
...etc.



## 本日の課題: ノイズで遊ぼう!

簡単に二次元から四次元のノイズを生成することのできるNoise TOPを使用して、試行錯誤しながら自分なりの「作品」をつくってみる。プログラムの原型は以下からダウンロードしてください。

まだ操作の基本を習得した段階ですが、まずはいろいろ試行錯誤しながら操作の基本感覚を身に付けていきましょう。その上で以下のような工夫をしてみてください。

<img src="https://yoppa.org/wp-content/uploads/2025/04/Screenshot-2025-04-17-052805-scaled.png" width="640">

[ノイズの基本サンプル](https://github.com/tado/tdexamples/blob/main/02-01_noiseBasic.toe)

### ランダム (乱数、random) とパーリンノイズ (perlin noise)

![](https://yoppa.org/wp-content/uploads/2026/04/random-noise.jpg)

Random Noise

- 各データ点はバラバラに決まり、前後の値とまったく関係がない
- 値がガタガタと激しく上下する
- 高周波成分から低周波成分まで同じくらい混ざっている（周波数が均一）


Perlin Noise

- となりの点とつながっていて、値が少しずつ変化する
- なめらかな波のような形になる
- ゆったりした変化（低い周波数）が中心で、細かいブレは少ない


### 参考資料

- [パーリンノイズ (wikipedia)](https://ja.wikipedia.org/wiki/%E3%83%91%E3%83%BC%E3%83%AA%E3%83%B3%E3%83%8E%E3%82%A4%E3%82%BA)
- [The Book of Shaders by Patricio Gonzalez Vivo & Jen Lowe](https://thebookofshaders.com/11/)

<img src="https://upload.wikimedia.org/wikipedia/commons/8/88/Perlin_noise_example.png" width="640">



### 基本: 使用されているオペレータのパラメーターを変化させてみる

- [Noise TOP](https://docs.derivative.ca/index.php?title=Noise_TOP) (noise1)
  - ノイズの細かさ
  - ノイズの複雑さ
  - ノイズの種類
- [LFO CHOP](https://docs.derivative.ca/index.php?title=LFO_CHOP) (lfo1)
  - 変化速度
  - 変化する波形の種類
- [HSV Adjust TOP](https://docs.derivative.ca/index.php?title=HSV_Adjust_TOP) (hsvadj1)
  - 色相を変えてみる (hue)
  - 彩度を変えてみる (saturation)
  - 明るさを変えてみる (brightness)

  

### 応用: オペレーターを追加してみる

- Noise TOPにNoise TOPを接続するとどうなるか?
- HSV Adjust TOPの前後に別のTOPを追加してみる
- LFO CHOPを追加して他のパラメータに参照させてみる



### ノイズを使用した作品例 1

- ノイズ+ノイズ、Bloomエフェクトの追加

<img src="https://yoppa.org/wp-content/uploads/2025/04/Screenshot-2025-04-17-060944-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/02-02_noiseAdvanced01.toe)



### ノイズを使用した作品例 2

- 万華鏡のようなパターンを生成

<img src="https://yoppa.org/wp-content/uploads/2025/04/Screenshot-2025-04-17-055046-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/02-03_noiseAdvanced02.toe)


### ノイズを使用した作品例 3

- ランダムな数値で激しく変化! - Pattern CHOP、LFO CHOP (pulse)、Hold CHOPを組合せる

<img src="https://yoppa.org/wp-content/uploads/2026/04/Screenshot-2026-04-15-at-9.32.56-scaled.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/02/04_noiseAdvanced03.toe)




## 実習!

- ノイズを使用した作品を作成してみましょう!
- 講義の最後の30分ほどで作品の発表を行います
