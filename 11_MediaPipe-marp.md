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

### この講義の今後の予定

07.03: TouchDesigner応用編 4 - コンピュータービジョン: MediaPipe Pluginをつかってみる
07.10: これまでの内容のふりかえりと、課題制作に向けての相談会
07.17: 最終課題の制作、質問の受付
07.24: 最終課題の講評会

---

## 今日の内容

- TouchDesignerでコンピュータビジョン (CV) を扱う方法を学ぶ
- Googleが開発したライブラリ「MediaPipe」をTouchDesignerから利用
- 顔・手・身体の骨格などをリアルタイムで高精度に検出
- カメラに映る人の動きや表情に反応するインタラクティブ表現へ

---

![height:440](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-15.13.04.jpg)

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

---

## 例1: FaceLandmarkからSOPのメッシュ生成

5. 顔のメッシュをレンダリングするには、カメラの画角や位置をMediaPipeで出力されたメッシュの3D座標に合わせて調整する

![height:340](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-14.59.48.jpg) ![height:340](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-15.00.56.jpg)

---

## 例1: FaceLandmarkからSOPのメッシュ生成

6. 最終的に下記のように配線すると、顔のメッシュが生成される

![height:400](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-15.06.00-scaled.jpg)

---

カメラ内の顔と生成されたメッシュがピッタリと重なる

![height:440](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-14.53.49.jpg)

ダウンロード: [faceTracker](https://github.com/tado/tdexamples/blob/main/11/01_faceTracker.toe)

---

## 例2: Face Meshにノイズをマッピング

- 生成されたFace MeshはSOPのオペレーターとして扱える
- これまでと同様に様々なオペレーターを組み合わせて加工可能
- 例: Face Meshにノイズをマッピングして、顔の表面に動的な変化を加える

![height:320](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-15.12.29-scaled.jpg)

---

![height:440](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-15.13.04.jpg)

ダウンロード: [faceTrackerNoise](https://github.com/tado/tdexamples/blob/main/11/02_faceTrackerNoise.toe)

---

## 例3: Hand Trackingで手の座標を取得

- hand_tracking.toxで両手の各指の細かな座標をリアルタイムで取得可能
- 各指の関節の座標は、instance_dataからCHOPとして出力される
- この情報で3DオブジェクトをInstanceすると、手の動きに応じて操作できる

![height:300](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-15.26.35-scaled.jpg)

---

![height:440](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-15.28.15.jpg)

ダウンロード: [handTracker](https://github.com/tado/tdexamples/blob/main/11/03_handTracker.toe)

---

## 例4: ユーザーインターフェイスとしてのHand Trackingの活用

- 手の座標を利用してユーザーインターフェイスとしての操作を実現
- 両手の人差し指の先端の座標を取得 → 球体を配置し、その間を直線で結ぶ
- instance_dataから特定の指の座標のみを抽出するためにTrim CHOPを使用

![height:300](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-17.20.17-scaled.jpg)

---

![height:440](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-17.22.45.jpg)

ダウンロード: [handTrackerUI](https://github.com/tado/tdexamples/blob/main/11/04_handTrackerUI.toe)

---

## 例4 (続き): カメラ映像の歪みを制御

- 両手の親指の先の座標も取得し、生成される四角形で映像に変化をつける
- 指の座標をLens Distort TOPのパラメータに接続して歪みを制御
- オーディオ制御、パーティクル制御、インスタレーション制作などに応用可能

![height:300](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-17.28.05-scaled.jpg)

---

![height:440](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-17.28.21.jpg)

ダウンロード: [handTrackerDistort](https://github.com/tado/tdexamples/blob/main/11/05_handTrackerDistort.toe)
