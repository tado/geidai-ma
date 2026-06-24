# TouchDesigner実践 3 – オーディオビジュアル、音響の視覚化

オーディオビジュアル（Audio Visual）とは、音（Audio）と映像（Visual）を組み合わせたメディアや技術全般を指す言葉です。

メディアアートの文脈では、単に音と映像を同時に扱うのではなく、両者を緊密に連携させることで統一感のある知覚体験を生み出す芸術形式を意味します。その根源には、キネティック・アブストラクト・アートのような動きを持つ抽象芸術と、音楽やサウンドが持つ時間的・構造的要素との関係性を探求する試みがあります。

空間全体で体験するアート・インスタレーション、リアルタイムで展開されるパフォーマンス、あるいは緻密に設計された映像作品など多岐にわたります。音のリズムが映像の動きを制御したり、映像の変化が新たな音を生み出したりと、その相互作用が鑑賞者の感覚に深く働きかけます。

今回は、このようなオーディオビジュアルな表現をTouchDesignerを用いて実現することを目指します。

## オーディオビジュアル参考作品

https://youtu.be/_UA40sL06sU?si=J_a52x3dNCoygCyl
Synchromy Norman McLaren (1971)

https://youtu.be/27hiBK_c3Oc?si=Kw-Om5i2gWco6s5i
ALVA NOTO - UNIEQAV #10 UNI EDIT

https://www.youtube.com/watch?v=RK6WnfWWnec
Ryoji Ikeda - Live in London, 2023.11.8

https://www.youtube.com/watch?v=h99HUWoVyWE
New Rituals 2022 at Barbican Centre feat Ryoichi Kurokawa and Nkisi (Trailer)

## 音波 (Sound Wave) とは?

そもそも音波とは、音とは何なのか?

https://www.youtube.com/watch?v=XLfQpv2ZRPU

## 波形解析 - 基本 1: サウンドファイルの再生と波形の表示

<img src="https://i0.wp.com/yoppa.org/wp-content/uploads/2025/06/Screenshot-2025-06-05-151821.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/08/01_SoundWave.toe)

TouchDesignerでは音を数値の連なりとして捉えるため、サウンドデータの扱いにはCHOPを使用します。サウンドファイルの読み込みには**Audio File In CHOP**を使用しますが、読み込んだだけではサウンドは再生されません。**Audio Device Out CHOP**に接続することで初めて音がスピーカーに出力されます。また、音波のデータが格納されたCHOPを**CHOP to TOP**に接続すると、波形を画像として視覚的に表示することができます。

## 波形解析 - 基本 2: サウンドの音量を視覚化する

<img src="https://i0.wp.com/yoppa.org/wp-content/uploads/2025/06/Screenshot-2025-06-04-121402.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/08-02_WaveCircles.toe)

**Audio File In CHOP**で取得した音データは、一定の範囲のサンプルがまとめてバッファリングされた状態で格納されています。このバッファ内のデータを単一の数値として利用するには、**Analyze CHOP**を使用する必要があります。Analyze CHOPには様々な解析方法が用意されており、**Value of First Peak**（バッファ先頭の値）、**Average**（全サンプルの平均）、**Maximum**（最大値）、**Minimum**（最小値）、**Sum**（合計値）、**RMS Power**（移動平均平方根）などを選択できます。音の大きさをビジュアライズする際には、音量の変化を滑らかに捉えられる**RMS Power**が最も適しています。数値化してしまえば、あとは図形の大きさや色など様々なパラメータに自由に適用できます。急激なピークの変化を抑えたい場合は**Lag CHOP**でスムージングを、全体の変化をさらに滑らかにしたい場合は**Filter CHOP**（ローパスフィルタ）でフィルタリングすることができます。

## 波形解析 - 基本 3: フィルターの活用、周波数帯域に分けて視覚化

<img src="https://i0.wp.com/yoppa.org/wp-content/uploads/2025/06/Screenshot-2025-06-04-124458.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/08-03_AudioFilters.toe)

オーディオ用のフィルタを使うことで、波形から特定の周波数帯域の成分だけを抽出することができます。フィルターには主に**ローパスフィルター（LPF）**・**ハイパスフィルター（HPF）**・**バンドパスフィルター（BPF）**・**バンドストップ**の種類があり、LPFは低域成分を通過させて高域をカット、HPFは高域成分を通過させて低域をカット、BPFは特定の周波数帯域のみを通過させます。これらのフィルターを組み合わせることで、元の音を低域・中域・高域に分解し、それぞれをAnalyze CHOPのRMS Powerで数値化してビジュアライズすることができます。低域にはバスドラ、高域にはハイハットといった音楽を構成する各成分ごとの変化を個別に観察・制御できるようになります。

## 波形解析 - 基本 4: Audio Analysis COMPを使用する

<img src="https://i0.wp.com/yoppa.org/wp-content/uploads/2025/06/Screenshot-2025-06-05-041639.jpg" width="640">

音の波形を周波数ごとに解析して値を滑らかに整える一連の処理を、より手軽に実装したい場合は**Audio Analysis COMP**が便利です。Palette > Tools > AudioAnalysis からドラッグ＆ドロップするだけで利用でき、高性能でありながら非常に手軽に扱えます。

<img src="https://i0.wp.com/yoppa.org/wp-content/uploads/2025/06/Screenshot-2025-06-05-043259.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/08-04_AudioAnalysis.toe)

これまで個別に組み上げていた解析パートをこのCOMPひとつに置き換えることができ、必要に応じてパラメーターを調整するだけで同等の機能をすっきりとしたネットワーク構成で実現できます。

## 波形解析 - 応用 1: 周波数帯域の視覚化を3Dで表現

<img src="https://i0.wp.com/yoppa.org/wp-content/uploads/2025/06/Screenshot-2025-06-05-041716.png" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/08-05_AudioFilters3D.toe)

これまで2次元の円（Circle TOP）で視覚化していたものを3Dへ拡張した例です。**ノイズで変形させた球（Sphere SOP）**を半透明に着色しながら変形させることで、音の周波数成分に応じて有機的に動く3Dオブジェクトを表現しています。2D表現に慣れてきたら、これまでの講義で学んだ内容を応用しながら、自由な発想で多種多様な3Dビジュアライゼーションに挑戦してみてください。

## 波形解析から周波数解析へ

なぜ周波数解析が必要なのでしょうか？FFT（高速フーリエ変換）は音を周波数の成分に分解する変換ですが、そもそもこの操作が必要な理由は何でしょうか？そのヒントは、私たちの耳が音を聴いている仕組みにあります。

https://www.youtube.com/watch?v=wkHPXHB3OxQ

https://youtu.be/sSgZXrdlIlM?si=eJHsRW3Oc0rgVwu4

<img src="https://i0.wp.com/yoppa.org/wp-content/uploads/2025/06/cochlea.png" width="640">

内耳の蝸牛（cochlea）は、音を周波数成分に自動的に分解しながら聴神経に伝えています。つまり私たちはそもそも音を周波数成分に分解して聴いているのです。

引用: Hearing, the cochlea, the frequency domain and Fourier's series

**FFTとIFFT**

<img src="https://i0.wp.com/yoppa.org/wp-content/uploads/2025/06/Convolution_FFT_and_IFFT.jpg" width="640">

**FFT（高速フーリエ変換）**は波形を周波数成分へと変換する演算で、逆に**IFFT（高速フーリエ逆変換）**は周波数成分を波形へと戻す演算です。

**STFT（短時間フーリエ変換）**

実際の音の解析では、信号を一定の時間窓で区切りながら逐次解析する**STFT（Short-Time Fourier Transform）**が用いられます。区切る時間の長さは**FFT Length**と呼ばれ、2の累乗のサンプル数で指定します。また、区切った信号の端部をなだらかにつなぐための**窓関数（Windowing）**も重要なパラメータです。このSTFTの解析結果を時系列でプロットしたものが**スペクトログラム（Spectrogram）**です。

<img src="https://i0.wp.com/yoppa.org/wp-content/uploads/2025/06/Spectrogram-19thC.png" width="640">

参考: 【視覚的に理解する】フーリエ変換

https://youtu.be/fGos3wrKeHY?si=9hY6VDXNkcLr11lP

## FFT視覚化基本1: スペクトラムをTOPで視覚化

<img src="https://i0.wp.com/yoppa.org/wp-content/uploads/2025/06/Screenshot-2025-06-05-060137.png" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/08-06_FFTBasic.toe)

**Audio File In CHOP**で取得した信号を**Audio Spectrum CHOP**に接続すると、FFTによる周波数解析が行われます。解析時には**FFT Size**（STFTで分割するサンプル数）、スケール（線形スケールとログスケールの切り替え）、高周波成分の強調といった様々なパラメータを調整することができます。解析結果を**CHOP to TOP**でテクスチャーに変換すると、各周波数帯域の強さがストライプ状のパターンとして浮かびあがります。

## FFT視覚化基本2: 左右対称にして着色

<img src="https://i0.wp.com/yoppa.org/wp-content/uploads/2025/06/Screenshot-2025-06-05-173929.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/08-07_FFTVisStripe.toe)

スペクトルのストライプに**Ramp TOP**を掛け合わせることで、周波数成分に応じたグラデーション着色が実現できます。左右のチャンネルを対称に配置することで、低域成分が中央に、高域成分が左右の周囲へと広がるシンメトリカルなビジュアルが生まれます。

## FFT視覚化基本3: 円に変換

<img src="https://i0.wp.com/yoppa.org/wp-content/uploads/2025/06/Screenshot-2025-06-05-062212.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/08-08_FFTVisCircle.toe)

**Cartesian To Polar COMP**を使用することで、デカルト座標系で表現されたストライプ状のスペクトルパターンを極座標系の円環状に変換することができます。左右のチャンネルの周波数成分が、リング状に展開された円形のビジュアルパターンとして表現されます。

## 応用: POPを使用したAudio Visual表現 1 - Audio Analysis + POP

前々回と前回に行ったPOPを使用して、オーディオビジュアル表現に挑戦してみましょう。まず始めは、Audio Analysis COMPでの解析結果をPOPで活用した例です。

<img src="https://yoppa.org/wp-content/uploads/2026/06/Screenshot-2026-06-09-at-12.27.29-scaled.jpg" width=640>

[ダウンロード](https://github.com/tado/tdexamples/blob/main/10/10_AudioAnalysis-Noise.toe)

この例では、前半にSOPで実装した、高域、中域、低域の音量でノイズで歪む球を描くプログラムをPOPに移植しています。SOPかPOPに移行したことで処理が軽くなり、よりポリゴン数を大量に使用した緻密な表現が可能となりました。

まず、Audio File In CHOPの信号を、Audio Analysys COMPを用いて周波数成分ごとのRMSに変化しているところまではSOPの例と同じです。それぞれの値をNoise POPのNoise Amplitudeに適用して歪んだ球を3つ生成しています。POPでは3D形状を構成するポイントに直接色の属性 (Color) を指定できるので、低域、中域、高域でそれぞれ別の色で塗り分けた立体を1つのGeometory COMPでレンダリングすることが可能となり、プログラムもすっきりとまとまりました。

## 応用: POPを使用したAudio Visual表現 2 - Audio Spectrum + POP

次に、FFT (Audio Spectrum CHOP) を使用して、その解析結果をPOPで表現してみましょう。

<img src="https://yoppa.org/wp-content/uploads/2026/06/Screenshot-2026-06-09-at-12.45.04-scaled.jpg" width=640>

[ダウンロード](https://github.com/tado/tdexamples/blob/main/10/11_AudioSpectrum-POP.toe)

この例では、Audio Spectrum CHOPを用いてFFT解析した情報を、CHOP to POPして、PointScaleに適用しています。その形状をCopy POPを用いて球の形態に分布させています。球体の色は、Ramp TOPを使用して作成したグラデーションんを、Loopup Texture POPを使用してそれぞれの球の色に適用しています。さらにNoise POPを用いて全体にゆらぎを持たせています。

## まとめ

今回の講義では、TouchDesignerを用いたオーディオビジュアル表現の基礎から応用までを体系的に学びました。まず音データをCHOPで扱う方法を出発点に、Analyze CHOPによる数値化、フィルターを活用した周波数帯域ごとの分解、Audio Analysis COMPによる効率的な実装へと段階的に理解を深めました。さらに、FFT（高速フーリエ変換）の原理を学び、Audio Spectrum CHOPを使ったスペクトル解析と、その結果をストライプ・シンメトリー・円形といった多彩なビジュアルへと展開する手法を習得しました。後半では、これまで学んだPOPと音響解析を組み合わせることで、GPUの高速処理を活かした3D音響ビジュアライゼーションが実現できることも確認しました。音と映像を緊密に連携させるオーディオビジュアルの世界は、ここで紹介した技術を土台に、さらに無限の表現へと広がっています。ぜひ今回学んだ要素を自由に組み合わせながら、自分だけのオーディオビジュアル表現を探求してみてください。

