# TouchDesigner上級編 1 - TouchDesigner + Shader (GLSL) 入門

## 今日の内容

今回はTouchDesignerでShader（GLSL）を扱う入門編です。まずShaderとは何かを概観し、TouchDesigner上でShaderを表示する方法を学びます。その上で、今回の中心となるフラグメントシェーダー（Fragment Shader）の基本を、実際にコードを書きながら習得していきます。

## サンプルプログラム

今回の講義で使用するサンプルプログラムは以下の通りです。

1. [色を塗る](https://github.com/tado/tdexamples/blob/main/09_GLSLBasic/GLSLBasic01.toe)
2. [色の点滅](https://github.com/tado/tdexamples/blob/main/09_GLSLBasic/GLSLBasic02.toe)
3. [位置情報によるグラデーション](https://github.com/tado/tdexamples/blob/main/09_GLSLBasic/GLSLBasic03.toe)
4. [波を動かす](https://github.com/tado/tdexamples/blob/main/09_GLSLBasic/GLSLBasic04.toe)
5. [同心円の波 1](https://github.com/tado/tdexamples/blob/main/09_GLSLBasic/GLSLBasic05.toe)
6. [同心円の波 2](https://github.com/tado/tdexamples/blob/main/09_GLSLBasic/GLSLBasic06.toe)
7. [シェーダーを平面にハイトマッピング](https://github.com/tado/tdexamples/blob/main/10_GLSLAdvanced/GLSLAdvanced01.toe)
8. [シェーダーを球体にハイトマッピング](https://github.com/tado/tdexamples/blob/main/10_GLSLAdvanced/GLSLAdvanced02.toe)

## Shaderとは何か?

Shaderとは、もともと3DCGにおいてシェーディング（陰影処理）を行うコンピュータプログラムのことを指します。

<img src="https://upload.wikimedia.org/wikipedia/commons/3/3d/Phong-shading-sample_%28cropped%29.jpg" width="640">

例: Phong Shading

従来は、開発者やデザイナーはグラフィクスカード（GPU）に固定機能として実装された定形の処理しか使えませんでした（固定機能シェーダー）。しかし2000年代に入るとプログラマブル・シェーダーが登場し、それまでブラックボックスだったシェーダー自体をプログラムできるようになりました。シェーダー言語は3D描画ライブラリによって異なり、OpenGLではGLSL、Direct3DではHLSLが用いられます。TouchDesignerは描画にOpenGLを使用しているため、Shader言語としてGLSLを用い、GLSL TOPによってShaderを表示することができます。

### Shaderの種類

Shaderにはいくつかの種類があります。頂点シェーダー（Vertex Shader）は、入力された頂点を座標変換するための機能です。ジオメトリシェーダー（Geometry Shader）は、オブジェクト内の頂点の集合を加工して新しいプリミティブ図形を生成します。そしてフラグメントシェーダー（Fragment Shader）は、ピクセルを操作するためのものです。画面上の膨大なピクセル情報を、高い並列処理性能を持つGPUで実行することにより、CPUで実行するよりもはるかに高いパフォーマンスを実現します。ピクセルシェーダー（Pixel Shader）と呼ばれることもあります。

今回は主にフラグメントシェーダーを扱います。

頂点のデータの集合から画面に画像が表示されるまでの流れは以下の通りです。

<img src="https://learnopengl.com/img/getting-started/pipeline.png" width="640">

### Shaderの教材

現在、すばらしいオンライン教材の執筆が進行中です。

<img src="https://repository-images.githubusercontent.com/32267301/46578c00-9a17-11eb-81ed-6719b09490c2" width="640">

- The Book of Shaders - Patricio Gonzalez Vivo
- http://patriciogonzalezvivo.com/2015/thebookofshaders/

### Mythbusters Demo GPU versus CPU

<img src="https://orbograph.com/wp-content/uploads/2020/12/mythbusters.png" width="640">

https://youtu.be/ZrJeYFxpUyQ?si=3n-9e01FzwNxkwsR

### Shaderのバージョン

GLSLにはさまざまなバージョンがあり、少しやっかいな点でもあります。TouchDesignerはGLSL 4.60（OpenGL 4.6）に対応しており、これは2024年現在のGLSL（およびOpenGL）の最新版です。OpenGLのバージョンとGLSLのバージョン、そして`#version`ディレクティブの対応は以下の通りです。

| OpenGLバージョン | GLSLバージョン | #versionディレクティブ |
| --- | --- | --- |
| 1.5 |1.0|なし|
|2.0|1.1|#version 110|
|2.1|1.2|#version 120|
|3.0|1.3|#version 130|
|3.1|1.4|#version 140|
|3.2|1.5|#version 150|
|3.3|3.3|#version 330|

| OpenGLバージョン | GLSLバージョン | #versionディレクティブ |
| --- | --- | --- |
|4.0|4.0|#version 400|
|4.1|4.1|#version 410|
|4.2|4.2|#version 420|
|4.3|4.3|#version 430|
|4.4|4.4|#version 440|
|4.5|4.5|#version 450|
|4.6|4.6|#version 460|

## TouchDesignerでGLSLを使う

まずはテキストエディターの設定を行います。Preferences > DATs でText Editorの場所を設定し、普段使用しているエディター（VSCodeなど）を指定しておきます。

<img src="https://docs.derivative.ca/images/thumb/8/87/PreferencesDATs.png/300px-PreferencesDATs.png" width="640">

### 画面にGLSL TOPを配置

まずは画面上にGLSL TOPを配置します。すると自動的に、glsl_pixelとglsl_infoの2つのDATがセットで配置されます。glsl1_pixel DATのパラメータから「Edit」ボタンを押すと、設定したテキストエディターでGLSLを編集できるようになります。

<img src="https://yoppa.org/wp-content/uploads/2024/06/Picture1.jpg" width="640">

初期状態では、以下のフラグメントシェーダーが記述されています。このファイルを編集していきます。

```GLSL
// Example Pixel Shader
// uniform float exampleUniform;

out vec4 fragColor;

void main() {
  // vec4 color = texture(sTD2DInputs[0], vUV.st);
  vec4 color = vec4(1.0);
  fragColor = TDOutputSwizzle(color);
}
```

このコードはおおよそ以下の意味になります。

```GLSL
// ピクセルシェーダーのサンプル
// uniform float exampleUniform; ← 外部からの入力例
out vec4 fragColor; // 最終出力する場所

void main() //メイン関数
{
  // ↓ 外部テクスチャーの色の参照方法
  // vec4 color = texture(sTD2DInputs[0], vUV.st);
  
  vec4 color = vec4(1.0); //色を指定(白)
  
  //最終出力へ、TDOutputSwizzle()はMacとWinのずれを解消する関数
  fragColor = TDOutputSwizzle(color);
}
```

試しに、vec4 colorの値を `vec4(1.0, 0.0, 0.0, 1.0)` のように変化させてみましょう。何が変化するでしょうか。

```GLSL
out vec4 fragColor;

void main() {
  // vec4で色を指定 (R:1.0, G:0.0, B:0.0, A:1.0)
  vec4 color = vec4(1.0, 0.0, 0.0, 1.0);
  fragColor = TDOutputSwizzle(color);
}
```

赤い色に変化しました。これは `color = vec4(Red, Green, Blue, Alpha);` の順で色を指定しているためです。

<img src="https://yoppa.org/wp-content/uploads/2024/06/Picture2.jpg" width="640">

ダウンロード: [色を塗る](https://github.com/tado/tdexamples/blob/main/09_GLSLBasic/GLSLBasic01.toe)

ここでプログラムを読み解いてみましょう。GLSLのプログラムは、まず始めにmain関数が実行されます。最終出力は `out vec4 fragColor;` と宣言し、main関数で演算した最終的なピクセルの色の値をfragColorに出力します。変数や関数の書き方は、ほぼC / C++の書き方を踏襲しており、int、float、boolなどはC / C++と同じように使用できます。加えて、vec2、vec3、vec4など、GLSL独自の型も存在しています。

代表的なGLSLの型には以下のようなものがあります。

- vec2 : float型による2次元のベクトル
- vec3 : float型による3次元のベクトル
- vec4 : float型による4次元のベクトル
- ivec2 : int型による2次元のベクトル
- ivec3 : int型による3次元のベクトル
- ivec4 : int型による4次元のベクトル
- mat2 : 2x2要素を持つfloat型の行列
- mat3 : 3x3要素を持つfloat型の行列
- mat4 : 4x4要素を持つfloat型の行列
- Sampler1D : 1次元のテクスチャ
- Sampler2D : 2次元のテクスチャ
- Sampler3D : 3次元のテクスチャ

## Uniforms - TouchDesignerからShaderへ値を送出

TouchDesignerとGLSLの間で情報をやりとりするイメージは以下の通りです。

<img src="https://yoppa.org/wp-content/uploads/2024/06/Screenshot-2024-06-19-180942.png" width="640">

### 経過時間を送る - 色の点滅

送出したUniformsをShaderで活用してみましょう。まず始めに経過時間を送出します。TouchDesigner側では `absTime.seconds` を、GLSL側ではfloat型を使います。

GLSL TOPのパラメータのVectorsを開き、以下のパラメータを設定して経過時間をGLSLに送出します。Uniform Name 0 に `time`、value0x に `absTime.seconds` を指定します。

<img src="https://yoppa.org/wp-content/uploads/2024/06/Picture3.jpg" width="640">

時間と三角関数、そして絶対値を組み合わせて色を点滅させてみます。色が激しく点滅するはずです。

```GLSL
uniform float time;
out vec4 fragColor;

void main() {
  float r = abs(sin(time * 10.0));  //赤の点滅
  float g = abs(sin(time * 12.0));  //緑の点滅
  float b = abs(sin(time * 14.0));  //青の点滅
  vec4 color = vec4(r, g, b, 1.0);  //vec4(RGBA)で色を指定
  fragColor = TDOutputSwizzle(color);
}
```

ダウンロード: [色の点滅](https://github.com/tado/tdexamples/blob/main/09_GLSLBasic/GLSLBasic02.toe)

## ピクセルシェーダーの座標系

ピクセルシェーダーのコードは、テクスチャー上のピクセル1つ1つにプログラムが埋め込まれているイメージで捉えると理解しやすくなります。では、自分自身のピクセルの場所を知るにはどうすればよいでしょうか。そのためには `vUV.x` と `vUV.y` を参照します。座標系は左下が (0.0, 0.0)、右上が (1.0, 1.0) となります。

<img src="https://ogldev.org/www/tutorial16/txt_coords.png" width="640">

試しにグラデーションを描いてみましょう。

```GLSL
uniform float time;
out vec4 fragColor;

void main() {
  float r = vUV.x;  //x方向: 赤のグラデーション
  float g = 0.0;    //緑は0.0
  float b = vUV.y;  //y方向: 青のグラデーション
  vec4 color = vec4(r, g, b, 1.0);
  fragColor = TDOutputSwizzle(color);
}
```

なめらかなグラデーションが描けました。

<img src="https://yoppa.org/wp-content/uploads/2024/06/Picture4.jpg" width="640">

ダウンロード: [位置情報によるグラデーション](https://github.com/tado/tdexamples/blob/main/09_GLSLBasic/GLSLBasic03.toe)

### 移動する波 (座標情報 + 経過時間)

ここでは、vUV.xy による座標の情報と経過時間（time）を組み合わせてみます。sin関数に座標の情報と経過時間を与えることで、移動する波を作ってみましょう。

```GLSL
uniform float time;
out vec4 fragColor;

void main() {
  float r = sin(time * 10.0 + vUV.x * 12.0);  // 赤
  float g = sin(time * -10.0 + vUV.x * 12.0); // 緑
  float b = sin(time * 10.0 + vUV.y * 8.0);   // 青
  float a = 1.0; // 透明度

  vec4 color = vec4(r, g, b, a);
  fragColor = TDOutputSwizzle(color);
}
```

RGBのストライプが移動します。

<img src="https://yoppa.org/wp-content/uploads/2024/06/Picture5.jpg" width="640">

ダウンロード: [波を動かす](https://github.com/tado/tdexamples/blob/main/09_GLSLBasic/GLSLBasic04.toe)

### 拡がる同心円

もう少し複雑な図形として、同心円を描いてみましょう。ポイントは、中心点からの距離を計測し、その値で形を描くという考え方です。GLSLで距離を測るには `length()` 関数を使います。

```GLSL
float len = length(startPos, endPos);
```

この距離の値を使って、拡がる同心円を描きます。

```GLSL
uniform float time;
out vec4 fragColor;

void main() {
  //画面の中心からの距離を算出
  float len = length(vec2(0.5, 0.5) - vUV.xy);
  //画面中心からの距離でsin波を生成し同心円状の波に
  float br = sin(len * 120 - time * 40.0);
  //ピクセルの色に設定
  vec4 color = vec4(br, br, br, 1.0);
  fragColor = TDOutputSwizzle(color);
}
```

中心から拡がる同心円が描けました。

<img src="https://yoppa.org/wp-content/uploads/2024/06/Picture6.jpg" width="640">

ダウンロード: [同心円の波 1](https://github.com/tado/tdexamples/blob/main/09_GLSLBasic/GLSLBasic05.toe)

### 拡がる同心円 (真円バージョン)

次に、円の幅と高さを揃えて楕円ではなく真円にしてみます。vUVを画面の解像度の幅で、xとy座標の双方を割り算することで縦横の比率を揃えます。さらに、RGBそれぞれで拡がる速度を変化させてみましょう。

```GLSL
uniform float time;
uniform vec2 resolution;
out vec4 fragColor;

void main() {
  //画面の中心から(-1.0, -1.0)から(1.0, 1.0)の範囲の座標に変換し縦横の比率を揃える
  vec2 uv = (gl_FragCoord.xy * 2.0 - resolution.xy) / vec2(resolution.x, resolution.x);
  //画面中心からの距離を算出
  float len = length(uv);
  //画面中心からの距離でsin波を生成し同心円状の波に
  //RGBを少しシフトさせる
  float r = sin(len * 120 - time * 40.0);
  float g = sin(len * 120 - time * 40.0 - 1.0);
  float b = sin(len * 120 - time * 40.0 - 2.0);
  vec4 color = vec4(r, g, b, 1.0);
  fragColor = TDOutputSwizzle(color);
}
```

円が真円になりました。

<img src="https://yoppa.org/wp-content/uploads/2024/06/Picture7.jpg" width="640">

ダウンロード: [同心円の波 2](https://github.com/tado/tdexamples/blob/main/09_GLSLBasic/GLSLBasic06.toe)

## GLSL応用: 生成されたイメージを3D化

最後に応用として、同心円のアニメーションのパターンを用いて凸凹を生成してみます。これはハイトマップ（Height Map）と呼ばれる手法で、ゲームエンジンなどでも使用されています。

<img src="https://docs.unity3d.com/2020.1/Documentation/uploads/Main/StandardShaderParallaxMap.jpg" width="640">

GLSLのパターンを立体化します（詳細は実際のプログラムで解説します）。

<img src="https://yoppa.org/wp-content/uploads/2024/06/Screenshot-2024-06-20-105941.png" width="640">

ダウンロード: [シェーダーを平面にハイトマッピング](https://github.com/tado/tdexamples/blob/main/10_GLSLAdvanced/GLSLAdvanced01.toe)

さらに球体にも適用してみます。

<img src="https://yoppa.org/wp-content/uploads/2024/06/Screenshot-2024-06-20-110625.png" width="640">

ダウンロード: [シェーダーを球体にハイトマッピング](https://github.com/tado/tdexamples/blob/main/10_GLSLAdvanced/GLSLAdvanced02.toe)

## アンケート

本日の演習に参加した方は以下のアンケートに回答してください。

- [アンケート](https://forms.gle/QZadWERp6xXfVNrh6)
