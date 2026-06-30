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

# TouchDesigner応用編 4 - コンピュータービジョン: MediaPipe Pluginをつかってみる

---

## 今日の内容

- TouchDesignerでコンピュータビジョン (CV) を扱う方法を学ぶ
- Googleが開発したライブラリ「MediaPipe」をTouchDesignerから利用
- 顔・手・身体の骨格などをリアルタイムで高精度に検出
- カメラに映る人の動きや表情に反応するインタラクティブ表現へ

---

![height:480](https://yoppa.org/wp-content/uploads/2025/07/image-8.png)

参考: [MediaPipeで遊んでみる](https://yoppa.org/mit-design4-22/14113.html)

---

## コンピュータビジョン (CV) とは

- コンピュータが画像や映像の内容を認識・理解するための技術
- 単に画像を表示するだけでなく、何が映っているかをデータとして分析
- スマートフォンの顔認証、自動運転支援、製品検査、医療画像診断など
- すでに私たちの生活や社会の様々な場面で活用されている

---

## CVを用いてできること

- **Face Tracking** : 顔を検出し、目や口の開閉、表情を読み取る
- **Hand Tracking** : 手の形や指の関節の位置をリアルタイムで追跡する
- **Pose Estimation** : 人物の骨格を検出し、姿勢や動作を分析する
- **Object Detection** : 特定の物体 (人、車、動物など) を識別し位置を特定する

---

## MediaPipeについて

![height:240](https://yoppa.org/wp-content/uploads/2025/07/image-2.png)

[MediaPipe Solutions guide](https://ai.google.dev/edge/mediapipe/solutions/guide) Google AI Edge

---

## MediaPipeの主な特徴

- Googleが開発し、オープンソースで提供されているCV向けフレームワーク
- ライブ配信やストリーミングメディアでの利用を想定 → リアルタイム性が高い
- **高いパフォーマンス** : CPUやGPUなど様々なハードウェア上で高速動作
- **高品質な学習済みモデル** : すぐに使える高性能なモデルが多数同梱
- **クロスプラットフォーム** : モバイル / PC / Web / エッジまで幅広く対応

---

## MediaPipe Vision Tasks (1)

- **Image Classification (画像分類)**
  - 画像に何が写っているかを識別し、ラベルを割り当てる
- **Object Detection (物体検出)**
  - 複数の物体を検出し、バウンディングボックスで位置を示す
- **Face Detection (顔検出)**
  - 人の顔を検出し、位置や大まかな傾きを特定する
- **Face Landmarker (顔のランドマーク検出)**
  - 目、鼻、口などの特徴点を詳細に検出、表情認識やアバター操作に応用

---

## MediaPipe Vision Tasks (2)

- **Hand Landmarker (手のランドマーク検出)**
  - 片手または両手の21個の関節点を検出、ジェスチャー認識が可能
- **Pose Landmarker (姿勢のランドマーク検出)**
  - 全身の主要な関節点 (33点) を検出し、姿勢や動きを推定
- **Image Segmentation (画像セグメンテーション)**
  - ピクセル単位で「人物」「空」「背景」などの領域に塗り分ける

---

## MediaPipe TouchDesigner Pluginについて

![height:380](https://yoppa.org/wp-content/uploads/2025/07/image-3.png)

[https://github.com/torinmb/mediapipe-touchdesigner](https://github.com/torinmb/mediapipe-touchdesigner)

---

## MediaPipe TouchDesigner Pluginの特徴

- MediaPipeをTouchDesignerで簡単に使えるようにしたカスタムコンポーネント (.tox)
- 通常はPythonスクリプトによる複雑な実装が必要
- プラグインを読み込むだけで、Face Mesh / Pose / Hand Tracking が使える
- TOP / CHOP / SOP のオペレーターとして直接扱える
- プログラミングの知識が少なくても高度なインタラクティブ作品を制作可能

---

## MediaPipe Pluginのダウンロードとインストール

- [リリースセクション](https://github.com/torinmb/mediapipe-touchdesigner/releases)から最新のrelease.zipをダウンロード
- Zipを展開して、MediaPipe TouchDesigner.toeファイルを開く
- すべてのコンポーネントは /toxes フォルダに、メインは MediaPipe.tox
- ドロップダウンからウェブカメラを選択、各モデルのオン/オフを切り替え可能

---

![height:420](https://yoppa.org/wp-content/uploads/2025/07/image-4.png)

---

## MediaPipe Pluginを使用したプロジェクト例

- このプラグインを使って、CVを活用したプロジェクトを制作してみる
- ※ サンプルファイル実行時は、releaseフォルダ内のtoxesフォルダを<br>サンプルファイルと同じ階層に配置すること

---

## 例1: FaceLandmarkからSOPのメッシュ生成

- MediaPipeのFaceLandmarkで検出した顔のランドマーク (特徴点) を利用
- TouchDesigner上で3Dメッシュをリアルタイムに生成する

1. TouchDesignerの新規ファイルを作成
2. toxesフォルダからMediaPipe.toxを配置、Detect Facial landmarks以外をOffに

---

![height:380](https://yoppa.org/wp-content/uploads/2025/07/image-6.png)

---

## 例1: FaceLandmarkからSOPのメッシュ生成

3. MediaPipe.toxの右側にface_tracking.toxを配置、さらにNull SOPをその右に
4. 下記のように配線すると、顔のメッシュが生成される

![height:280](https://yoppa.org/wp-content/uploads/2025/07/image-7.png)

ダウンロード: [faceTracker](https://github.com/tado/tdexamples/blob/main/11/01_faceTracker.toe)

---

あとは、これまでと同様にレンダリングが可能、例: ノイズマン!

![height:440](https://yoppa.org/wp-content/uploads/2025/07/image-8.png)

ダウンロード: [faceTrackerNoise](https://github.com/tado/tdexamples/blob/main/11/02_faceTrackerNoise.toe)

---

## 例2: Hand Trackingで物体を動かす

![height:280](https://yoppa.org/wp-content/uploads/2025/07/image-10.png)

- Hand Tracking (hand_tracking.tox) で両手の各指の細かな座標が全て取得可能
- インタラクティブな様々なプロジェクトに応用可能

ダウンロード: [handTracker](https://github.com/tado/tdexamples/blob/main/11/03_handTracker.toe)

---

Hand Trackingで手の座標を取得 → 人差し指の先に球体を配置

![height:440](https://yoppa.org/wp-content/uploads/2025/07/image-9.png)

ダウンロード: [handTrackerBall](https://github.com/tado/tdexamples/blob/main/11/04_handTrackerBall.toe)
