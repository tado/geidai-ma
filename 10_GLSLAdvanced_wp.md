# TouchDesigner上級編 2 - TouchDesigner + Shader (GLSL) 応用 GLSL MAT

## 今日の内容

今回はTouchDesignerでShader（GLSL）を扱う応用編として、**GLSL MAT**を取り上げます。前回学んだGLSL TOPによるFragment Shaderの基礎に加えて、今回はVertex Shaderの編集にも踏み込み、3Dオブジェクトに対して直接シェーダーを適用する方法を学びます。GLSL TOPでは平面的な (x, y) 座標しか扱えませんでしたが、GLSL MATを使うことで奥行き (z) の情報も参照できるようになり、立体的な質感や色の変化を伴ったリッチな映像表現が可能になります。

## サンプルプログラム

今回の講義で使用するサンプルプログラムは以下の通りです。

1. [GLSL TOP基本: 移動する波](https://github.com/tado/tdexamples/blob/main/10/01_GLSL-TOP-Basic01.toe)
2. [GLSL TOP基本: 拡がる同心円](https://github.com/tado/tdexamples/blob/main/10/02_GLSL-TOP-Basic02.toe)
3. [GLSL TOP: バンプマッピングとハイトマッピングで凸凹表現](https://github.com/tado/tdexamples/blob/main/10/03_GLSL-TOP-mapping.toe)
4. [GLSL TOP: 球面に平面的に波を描く](https://github.com/tado/tdexamples/blob/main/10/04_GLSL-TOP-Sphere.toe)
5. [GLSL MAT: 球面に立体的に波を描く](https://github.com/tado/tdexamples/blob/main/10/05_GLSL-MAT-Sphere.toe)
6. [GLSL MAT: 中心からの距離で色を変化させる](https://github.com/tado/tdexamples/blob/main/10/06_GLSL-MAT-length.toe)
7. [GLSL MAT: アルファ値を使ってピクセルの描画を制御する](https://github.com/tado/tdexamples/blob/main/10/07_GLSL-MAT-alpha.toe)
8. [Phong MATのShaderを編集してオリジナルの表現を作る - 球体](https://github.com/tado/tdexamples/blob/main/10/08_GLSL-Phong-MAT-Sphere.toe)
9. [Phong MATのShaderを編集してオリジナルの表現を作る - グリッド](https://github.com/tado/tdexamples/blob/main/10/09_GLSL-Phong-MAT-Grid.toe)

## 前回の復習 1: 移動する波

まずは前回のGLSL TOPの内容を復習します。最初の例は、時間の経過とともに移動する波のアニメーションです。ポイントは2つあります。1つは、各ピクセルが自分自身の座標を参照するための変数 vUV（vUV.x, vUV.y）を使うこと。もう1つは、時間経過の値を外部から受け取るためのuniform変数 time を使うことです。この自分自身の位置（vUV）と経過した時間（time）をsin関数に与えることで、画面上を移動していく波の模様を描き出します。

```GLSL
uniform float time; // 時間経過の値を取得するためのuniform変数
out vec4 fragColor; // 出力する色を格納する変数

void main() {
  // 自分自身の座標を参照して、sin関数で移動する波を描く
	float r = sin(vUV.x * 40 - time * 5.0) * 0.5 + 0.5;
	float g = sin(vUV.y * 40 - time * 5.0) * 0.5 + 0.5;
	float b = vUV.x; 
	// RGBを少しシフトさせる
	vec4 color = vec4(r, g, b, 1.0);
	// 生成された色を出力する
	fragColor = TDOutputSwizzle(color);
}
```

このコードを実行すると、画面を横切るように移動する波が描けました。

<img src="https://yoppa.org/wp-content/uploads/2026/06/Screenshot-2026-06-24-at-19.50.36-scaled.jpg" width="640">

ダウンロード: [GLSL TOP基本: 移動する波](https://github.com/tado/tdexamples/blob/main/10/01_GLSL-TOP-Basic01.toe)

## 前回の復習 2: 拡がる同心円

2つ目の復習は、中心から外側へと拡がっていく同心円のアニメーションです。ここでのポイントは、画面の中心点からの距離を計測し、その距離の値を使って形を描くという考え方です。GLSLには2点間の距離を測るための `length()` 関数が用意されています。

```GLSL
float len = length(startPos, endPos);
```

この `length()` で求めた中心からの距離をsin関数に渡すことで、距離に応じて波打つ同心円状の模様を生成できます。さらにRGBそれぞれの波の周期を少しずつずらすことで、色が変化していく美しい同心円を描き出します。

```GLSL
uniform float time;
uniform vec2 resolution;

out vec4 fragColor;
void main() {
  //画面の中心から(-1.0, -1.0)から(1.0, 1.0)の範囲の座標に変換し縦横の比率を揃える
  vec2 uv = (vUV.xy * 2.0 - resolution) / vec2(resolution.x, resolution.x);
  //画面中心からの距離を算出
  float len = length(uv);
  //画面中心からの距離でsin波を生成し同心円状の波に
  //RGBを少しシフトさせる
  float r = sin(len * 20 - time * 5.0) * 0.5 + 0.5;
  float g = sin(len * 21 - time * 5.0 - 0.2) * 0.5 + 0.5;
  float b = sin(len * 22 - time * 5.0 - 0.4) * 0.5 + 0.5;
  vec4 color = vec4(r, g, b, 1.0);
  fragColor = TDOutputSwizzle(color);
}
```

中心から拡がる同心円が描けました。

<img src="https://yoppa.org/wp-content/uploads/2026/06/Screenshot-2026-06-24-at-19.29.03-scaled.jpg" width="640">

ダウンロード: [GLSL TOP基本: 拡がる同心円](https://github.com/tado/tdexamples/blob/main/10/02_GLSL-TOP-Basic02.toe)

## 前回の復習 3: GLSLで生成されたイメージで凸凹を表現

3つ目の復習は、同心円のアニメーションのパターンを利用して立体的な凸凹を表現する例です。これは**ハイトマップ（Height Map）**と呼ばれる手法を用いており、ゲームエンジンなどでも広く使われている、グレースケールの明暗を高さ情報として扱うテクニックです。

<img src="https://docs.unity3d.com/2020.1/Documentation/uploads/Main/StandardShaderParallaxMap.jpg" width="640">

具体的な手順としては、まず球体をPOPでレンダリングしてPhong MATを適用し、GLSLで生成されたイメージを使って凸凹を表現します。Phong MATでは次のような設定を行います。GLSL TOPで出力されたTextureをColor Mapとしてマッピングし、同じTextureをNormal Mapに変換してバンプマッピングを行います。さらに、出力されたTextureをBlur TOPやLevel TOPで加工した上で、ハイトマップとして使用します。これらを組み合わせることで、平面のテクスチャから立体的な凹凸を持つ表現が生み出せます。

<img src="https://yoppa.org/wp-content/uploads/2026/06/Screenshot-2026-06-24-at-20.06.25-scaled.jpg" width="640">

ダウンロード: [GLSL TOP基本: バンプマッピングとハイトマッピングで凸凹表現](https://github.com/tado/tdexamples/blob/main/10/03_GLSL-TOP-mapping.toe)

## 先週の復習: ここまでのまとめ

ここまでの内容を整理しておきましょう。画面上のピクセル単位で色を計算しているのがFragment Shaderであり、画面の全てのピクセルに同じプログラムが埋め込まれているようなイメージで捉えると理解しやすくなります。最終的に出力する色は `gl_FragColor`（あるいは出力用の変数）に格納します。各ピクセルが自分自身の座標を参照するための変数が vUV（vUV.x, vUV.y）です。また、時間経過を取得するためには、uniform変数を使って外部から時間の値をシェーダーに渡す必要があります。

## 先週の復習: GLSL TOPを立体に適用するということは?

ここで、GLSL TOPを立体に適用する際の限界について考えてみます。Fragment Shaderは (vUV.x, vUV.y) で自分自身の平面的な位置を取得できますが、奥行き (z) の情報を参照することはできません。そのため、球面にGLSL TOPを適用しても、あくまで平面のテクスチャを球面に貼り付けただけの状態になってしまいます。では、奥行きの情報も参照するにはどうすればよいのでしょうか。その答えが、今回学ぶ**GLSL MAT**です。

<img src="https://yoppa.org/wp-content/uploads/2026/06/Screenshot-2026-06-24-at-22.07.59-scaled.jpg" width="640">

ダウンロード: [GLSL TOP: 球面に平面的に波を描く](https://github.com/tado/tdexamples/blob/main/10/04_GLSL-TOP-Sphere.toe)

## 本日の内容: GLSL MATを使ってみる

ここからが今回の本題です。TouchDesignerでShader（GLSL）を使う応用編として、GLSL MATを使ってみましょう。GLSL MATを使うと、これまでの (x, y) 座標だけでなく、奥行き (z) の情報も参照できるようになります。そのために、Fragment Shaderに加えてVertex Shaderも編集していきます。

## GLSL MATの基本構造

GLSL MATを配置すると、3つのDATが自動で生成されます。1つ目はVertex Shader用のDAT、2つ目はPixel Shader（Fragment Shader）用のDAT、3つ目はコンソール（エラー出力など）用のDATです。まずは、自動生成されるデフォルトのVertex ShaderとPixel Shaderのコードを解析して、基本構造を理解していきましょう。

GLSL MATのVertex Shaderの基本コードは次のようになっています。

```GLSL
void main() 
{
  // ローカル座標を取得
  vec3 pos = TDPos();
  // 変換後の座標をgl_Positionに代入
  vec4 worldSpacePos = TDDeform(pos);
  // 変換後の座標をgl_Positionに代入
  // gl_Positionが頂点の最終的な位置を出力する
  gl_Position = TDWorldToProj(worldSpacePos);
}
```

GLSL MATのFragment Shaderの基本コードは次のようになっています。

```GLSL
out vec4 oFragColor; // 出力カラー
void main() 
{
  // ピクセルが画面内に描画されるかどうかを判定する関数
  TDCheckDiscard(); 
  // 出力カラーを設定する
  vec4 color = vec4(1.0);
  // アルファ値を使ってピクセルの描画を制御する関数
  TDAlphaTest(color.a);
  // 最終的な出力カラーを設定する
  // oFragColorは、最終的に画面に描画されるピクセルの色を表す変数
  oFragColor = TDOutputSwizzle(color);
}
```

## Vertex ShaderからFragment Shaderに値を渡す

GLSL MATでは、Vertex Shaderで計算した値をFragment Shaderに渡すことができます。これにはvarying変数と呼ばれる仕組みを使用します。具体的には、Vertex Shader側の `out` 変数と、それを受け取るFragment Shader側の `in` 変数をペアにして対応させます。

例えば、ローカル座標を渡したい場合は次のように記述します。

```GLSL
// Vertex Shader側
out vec3 localPos; // ローカル座標をFragment Shaderに渡す
```

```GLSL
// Fragment Shader側
in vec3 localPos; // Vertex Shaderから渡されたローカル座標を受け取る
```

## Vertex Shaderで取得したローカル座標を使って、Fragment Shaderで色を計算する

この仕組みを利用して、Vertex Shaderで取得したローカル座標をFragment Shaderに渡し、その座標を使って色を計算してみましょう。Vertex Shader側では `TDPos()` を使ってローカル座標を取得し、それを `out` 変数に格納します。Fragment Shader側では `in` 変数でそのローカル座標を受け取り、色を計算します。これにより、これまで参照できなかった奥行き (z) の情報も使えるようになります。x, y, z のそれぞれの座標を使って r, g, b の波を生成し、時間経過とともに移動する立体的な波を描いてみましょう。

Vertex Shaderは次のようになります。

```GLSL
// ローカル座標をPixel Shaderに渡すための出力変数
out vec3 localPos;

void main() {
  // ローカル座標を取得
  vec3 pos = TDPos(); 
  // ローカル座標をPixel Shaderに渡す
  vec4 worldSpacePos = TDDeform(pos);
  // 変換後の座標をgl_Positionに代入
  gl_Position = TDWorldToProj(worldSpacePos);
  // ローカル座標をPixel Shaderに渡す
  localPos = pos;
}
```

Fragment Shaderは次のようになります。

```GLSL
uniform float time; // 時間経過の値を取得するためのuniform変数
out vec4 oFragColor; // 出力する色を格納する変数
in vec3 localPos; // ローカル座標を取得するための入力変数

void main() {
  // ピクセルが画面内に描画されるかどうかを判定する関数
  TDCheckDiscard();
  // ローカル座標を使って色を計算する
  float r = sin(localPos.x * 20 - time * 5.0) * 0.5 + 0.5;
  float g = sin(localPos.y * 20 - time * 5.0) * 0.5 + 0.5;
  float b = sin(localPos.z * 20 - time * 5.0) * 0.5 + 0.5;
  float a = 1.0;
  vec4 color = vec4(r, g, b, a);
  // アルファ値を使ってピクセルの描画を制御する関数
  TDAlphaTest(color.a);
  // 最終的な出力カラーを設定する
  oFragColor = TDOutputSwizzle(color);
}
```

x, y, z のそれぞれの座標を使って r, g, b の波を生成することで、球体の表面に沿って色が立体的に変化していくアニメーションが描画されます。

<img src="https://yoppa.org/wp-content/uploads/2026/06/Screenshot-2026-06-24-at-22.39.49-scaled.jpg" width="640">

ダウンロード: [GLSL MAT: 球面に立体的に波を描く](https://github.com/tado/tdexamples/blob/main/10/05_GLSL-MAT-Sphere.toe)

## 3D空間上の中心からの距離で色を変化させる

GLSL TOPで同心円を描いたときと同じように、GLSL MATでも `length()` 関数を使って3D空間上の中心からの距離を計算することができます。そして、その距離の値を使って色を変化させることができます。ここでは、球体（Sphere POP）をNoise POPで変形させた上で、中心からの距離に応じて色を変化させるGLSL MATを作ってみましょう。

Vertex Shaderは先ほどと同じものを使用し、変更はありません。

```GLSL
// ローカル座標をPixel Shaderに渡すための出力変数
out vec3 localPos;

void main() {
  // ローカル座標を取得
  vec3 pos = TDPos(); 
  // ローカル座標をPixel Shaderに渡す
  vec4 worldSpacePos = TDDeform(pos);
  // 変換後の座標をgl_Positionに代入
  gl_Position = TDWorldToProj(worldSpacePos);
  // ローカル座標をPixel Shaderに渡す
  localPos = pos;
}
```

Fragment Shaderでは、ローカル座標の中心からの距離を `length()` で求め、その距離を使って波を生成します。

```GLSL
uniform float time; // 時間経過の値を取得するためのuniform変数
out vec4 oFragColor; // 出力する色を格納する変数
in vec3 localPos; // ローカル座標を取得するための入力変数

void main() {
  // ピクセルが画面内に描画されるかどうかを判定する関数
  TDCheckDiscard();
  // ローカル座標の中心からの距離を計算する
  float len = length(localPos);
  // 中心からの距離を使って、r, g, b の波を生成する
  float r = sin(len * 10.0 - time * 3.0) * 0.5 + 0.5;
  float g = sin(len * 10.4 - time * 3.0) * 0.5 + 0.5;
  float b = sin(len * 10.8 - time * 3.0) * 0.5 + 0.5;
  float a = 1.0;
  vec4 color = vec4(r, g, b, a);
  // アルファ値を使ってピクセルの描画を制御する関数
  TDAlphaTest(color.a);
  // 最終的な出力カラーを設定する
  oFragColor = TDOutputSwizzle(color);
}
```

中心からの距離に応じて色が変化する、立体的な表現が得られました。

<img src="https://yoppa.org/wp-content/uploads/2026/06/Screenshot-2026-06-24-at-22.59.32-scaled.jpg" width="640">

ダウンロード: [GLSL MAT: 中心からの距離で色を変化させる](https://github.com/tado/tdexamples/blob/main/10/06_GLSL-MAT-length.toe)

## アルファ値を使って、ピクセルの描画を制御する

`TDAlphaTest()` 関数を使うことで、アルファ値を使ってピクセルの描画を制御することができます。これは、設定した閾値より小さいアルファ値を持つピクセルを描画しないようにする仕組みです。これを利用すると、面の一部が透明になって消えるような表現が可能になります。

次のFragment Shaderの例では、RGBの波に加えて、アルファ値も距離と時間を使った波で生成しています。

```GLSL
uniform float time; // 時間経過の値を取得するためのuniform変数
out vec4 oFragColor; // 出力する色を格納する変数
in vec3 localPos; // ローカル座標を取得するための入力変数

void main() {
  // ピクセルが画面内に描画されるかどうかを判定する関数
  TDCheckDiscard();
  // ローカル座標の中心からの距離を計算する
  float len = length(localPos);
  // 中心からの距離を使って、r, g, b の波を生成する
  float r = sin(len * 10.0 - time * 3.0) * 0.5 + 0.5;
  float g = sin(len * 10.4 - time * 3.0) * 0.5 + 0.5;
  float b = sin(len * 10.8 - time * 3.0) * 0.5 + 0.5;
  // アルファ値も同様に距離と時間を使って波を生成する
  float a = sin(len * 12.0 - time * 3.0);
  vec4 color = vec4(r, g, b, a);
  // アルファ値を使ってピクセルの描画を制御する関数
  TDAlphaTest(color.a);
  // 最終的な出力カラーを設定する
  oFragColor = TDOutputSwizzle(color);
}
```

このアルファ値による透明度の描画を有効にするには、GLSL MATのCommonタブで設定を行う必要があります。

<img src="https://yoppa.org/wp-content/uploads/2026/06/Screenshot-2026-06-24-at-23.06.57.jpg" width="640">

設定が完了すると、アルファ値に応じて面の一部が透明になり、虫食いのように一部が抜け落ちたような表現が可能になりました。

<img src="https://yoppa.org/wp-content/uploads/2026/06/Screenshot-2026-06-24-at-23.06.31-scaled.jpg" width="640">

ダウンロード: [GLSL MAT: アルファ値を使ってピクセルの描画を制御する](https://github.com/tado/tdexamples/blob/main/10/07_GLSL-MAT-alpha.toe)

## Phong MATのShaderを編集してオリジナルの表現を作る

これまでのGLSL MATによる描画には、陰影がつかないという課題がありました。そこで、Phong MATのShaderを編集することで、ライティングによる陰影がついた上にオリジナルの効果を加えた表現を作ってみましょう。Phong MATをGLSL MATに変換し、自動生成されたVertex ShaderとFragment Shaderを編集することで、陰影計算とオリジナルの色の変化を両立させることができます。

変換の手順は、Phong MATのRGBタブの最下部にある「Output Shader...」の横の「Output」ボタンを押すだけです。これにより、Phong MATの機能を再現したGLSL MATが生成されます。

生成されたFragment Shaderに、以下のようなコードを追加します。

```GLSL
//...(前略)...

void main() {
  
  //...(中略)...

  // --- 以下の部分を追記することで、オリジナルな効果を生みだしている ---
  float r = sin(iVert.worldSpacePos.z * 20.0) * 0.5 + 0.5;
  float g = sin(iVert.worldSpacePos.z * 22.0) * 0.5 + 0.5;
  float b = sin(iVert.worldSpacePos.z * 24.0) * 0.5 + 0.5;
  float a = sin(iVert.worldSpacePos.z * 43.0) + 0.5;

  finalColor.r = (finalColor.r + r) / 2.0;
  finalColor.g = (finalColor.g + g) / 2.0;
  finalColor.b = (finalColor.b + b) / 2.0;
  finalColor.a = a;
  // --- ここまでが追記部分 ---
  
  //...(中略)...

}

```

このように、Phongシェーディングによって計算された陰影（finalColor）に、自分で計算した色の変化を混ぜ合わせることで、立体感のある陰影とオリジナルの色彩表現を両立させることができました。

<img src="https://yoppa.org/wp-content/uploads/2026/06/Screenshot-2026-06-24-at-23.16.57.jpg" width="640">

ダウンロード: [Phong MATのShaderを編集してオリジナルの表現を作る - 球体](https://github.com/tado/tdexamples/blob/main/10/08_GLSL-Phong-MAT-Sphere.toe)

最後に、Noiseで変形させる元のPOPを、SphereからGridに変更してみると、また違った趣の面白い表現が得られます。元となる形状を変えながら、オリジナルの表現を探求してみましょう。

<img src="https://yoppa.org/wp-content/uploads/2026/06/Screenshot-2026-06-24-at-23.31.49.jpg" width="640">

ダウンロード: [Phong MATのShaderを編集してオリジナルの表現を作る - グリッド](https://github.com/tado/tdexamples/blob/main/10/09_GLSL-Phong-MAT-Grid.toe)
