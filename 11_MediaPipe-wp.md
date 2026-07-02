# TouchDesigner応用編 4 - コンピュータービジョン: MediaPipe Pluginをつかってみる

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-15.13.04.jpg" width="640">

## 今日の内容

本日のレクチャーでは、TouchDesignerでコンピュータビジョン（CV）を扱う方法について解説します。

コンピュータビジョンとは、コンピュータが画像や映像から情報を読み取り、人間の「眼」のように世界を理解するための技術分野です。この技術をTouchDesignerに応用することで、カメラに映る人の動きや表情に反応する、インタラクティブなビジュアル表現が可能になります。

今回は、そのためのツールとしてGoogleが開発した「MediaPipe」というライブラリをTouchDesignerから利用します。MediaPipeは、顔や手、身体の骨格などをリアルタイムで高精度に検出できる非常に強力なフレームワークです。専門的な知識がなくても、比較的手軽に高度な画像認識技術を扱える点が大きな魅力です。

TouchDesignerとMediaPipeを使ったCVの基本的な実装方法を学び、クリエイティブな表現の幅を広げていきましょう。

参考: [MediaPipeで遊んでみる](https://yoppa.org/mit-design4-22/14113.html)

## コンピュータビジョン (CV) とは

コンピュータビジョン（CV）は、コンピュータが人間の視覚のように、画像や映像の内容を認識し理解するための一連の技術です。単に画像を表示するだけでなく、その中に何が映っているのか、それがどのような状態かをデータとして捉え、分析します。

CVを用いることで、以下のような機能が実現できます。

- Face Tracking: 顔を検出し、目や口の開閉、表情を読み取ります。
- Hand Tracking: 手の形や指の関節の位置をリアルタイムで追跡します。
- Pose Estimation: 人物の骨格を検出し、姿勢や動作を分析します。
- Object Detection: 特定の物体（人、車、動物など）を識別し、その位置を特定します。

これらの技術は、スマートフォンの顔認証、自動車の自動運転支援システム、工場の製品検査、医療画像の診断支援など、すでに私たちの生活や社会の様々な場面で活用され、重要な役割を担っています。

## MediaPipeについて

<img src="https://yoppa.org/wp-content/uploads/2025/07/image-2.png" width="640">

[MediaPipe Solutions guide](https://ai.google.dev/edge/mediapipe/solutions/guide) Google AI Edge

MediaPipeは、Googleが開発し、オープンソースで提供しているコンピュータビジョン（CV）向けのフレームワークです。特にライブ配信やストリーミングメディアでの利用を想定して設計されており、リアルタイム性が求められるインタラクティブアート制作とも相性が良い開発環境です。

### 主な特徴

- **高いパフォーマンス**: CPUやGPUなど、様々なハードウェア上で高速に動作するよう最適化されています。これにより、PCのカメラ映像などに対して、遅延の少ないリアルタイム処理を実現します。
- **高品質な学習済みモデル**: Googleによってトレーニングされた、すぐに使える高性能なモデルが多数同梱されています。ユーザーは自身で膨大なデータを集めて機械学習モデルを構築する手間なく、最先端の認識技術を直接利用できます。
- **クロスプラットフォーム**: AndroidやiOSといったモバイル端末から、PC（Windows, macOS, Linux）、Webブラウザ、さらにはエッジデバイスまで、幅広い環境で動作するように設計されています。

### MediaPipe Vision Tasks

MediaPipeが提供するVision Task（視覚タスク）には、画像や動画の中から特定の情報を認識・分析するための、以下のような学習済みモデルが用意されています。

**Image Classification (画像分類)**

画像に何が写っているかを識別し、最も可能性の高いラベル（例:「猫」「犬」「車」）を割り当てます。一般的な物体認識に利用できます。

**Object Detection (物体検出)**

画像内にある複数の物体を検出し、それぞれの位置をバウンディングボックス（境界の四角形）で示し、ラベル付けします。人や動物、その他の特定の物体を一度に複数認識したい場合に有効です。

**Face Detection (顔検出)**

画像や動画から人の顔を検出します。顔の位置や大まかな傾きを特定することに特化しており、より詳細な顔のパーツを分析するタスクの第一段階として使われることもあります。

**Face Landmarker (顔のランドマーク検出)**

顔の輪郭、目、鼻、口などの特徴点を詳細に検出します。顔のメッシュ（トポロジー）や、視線の向き（ブレンドシェイプ）なども推定でき、表情認識やアバター操作に応用できます。

**Hand Landmarker (手のランドマーク検出)**

片手または両手の21個の関節点を検出します。これにより、手の形状やジェスチャーをリアルタイムで認識することが可能です。

**Pose Landmarker (姿勢のランドマーク検出)**

全身の主要な関節点（33点）を検出し、人物の姿勢や動きを推定します。スポーツのフォーム分析や、キャラクターアニメーションの制御などに利用されます。

**Image Segmentation (画像セグメンテーション)**

画像をピクセル単位で分析し、各ピクセルがどのカテゴリに属するかを分類します。例えば、「人物」「空」「背景」といった領域ごとに画像を塗り分けることができ、背景の差し替えなどに使われます。

## MediaPipe TouchDesigner Pluginについて

<img src="https://yoppa.org/wp-content/uploads/2025/07/image-3.png" width="640">

[https://github.com/torinmb/mediapipe-touchdesigner](https://github.com/torinmb/mediapipe-touchdesigner)

このプラグインは、GoogleのMediaPipeフレームワークをTouchDesignerで簡単に利用できるようにしたカスタムコンポーネント（.toxファイル）です。

通常、MediaPipeのような外部のライブラリをTouchDesignerで使うには、Pythonスクリプトによる複雑な実装が必要になります。しかし、このプラグインをプロジェクトに読み込むだけで、Face Mesh（顔の検出）やPose（姿勢推定）、Hand Tracking（手の追跡）といったMediaPipeの強力な機能を、TOPやCHOP、SOPのオペレーターとして直接扱えるようになります。

これにより、プログラミングの知識が豊富でなくても、リアルタイムの人物トラッキングなどを活用した高度なインタラクティブ作品を直感的に制作できます。

今回は、このTouchDesigner MediaPipe Pluginを使用して、CVを活用したTouchDesignerのプロジェクトを制作します。

### MediaPipe Pluginのダウンロードとインストール

[リリースセクション](https://github.com/torinmb/mediapipe-touchdesigner/releases)から最新のrelease.zipをダウンロードしてください。Zipファイルを展開して、中にあるMediaPipe TouchDesigner.toeファイルを開いてください。すべてのコンポーネントは/toxesフォルダに保存されています。メインコンポーネントはMediaPipe.toxです。

MediaPipeコンポーネントが読み込まれると、ドロップダウンからウェブカメラを選択できます。MediaPipeの様々なモデルのオン/オフやプレビューオーバーレイのオン/オフを切り替えられます。また、各モデルにはサブメニューが用意されており、さらにカスタマイズ可能です。

<img src="https://yoppa.org/wp-content/uploads/2025/07/image-4.png" width="640">

## TouchDesigner MediaPipe Pluginを使用したプロジェクト例

※ サンプルファイルを実行する際には、releaseフォルダ内のtoxesフォルダを、サンプルファイルと同じ階層に配置してください。

### FaceLandmarkからSOPのメッシュ生成

MediaPipeのFaceLandmark機能で検出した顔のランドマーク（特徴点）を基に、TouchDesigner上で3Dメッシュをリアルタイムに生成してみましょう。

1. TouchDesignerの新規ファイルを作成
2. toxesフォルダから、MediaPipe.toxを配置、Detect Facial landmarks以外をOffに

<img src="https://yoppa.org/wp-content/uploads/2025/07/image-6.png" width="640">

3. MediaPipe.toxの右側にface_tracking.toxを配置、さらにNull SOPをその右に
4. 下記のように配線すると、顔のメッシュが生成される

<img src="https://yoppa.org/wp-content/uploads/2025/07/image-7.png" width="640">

5. 顔のメッシュをレンダリングするには、カメラの画角や位置をMediaPipeで出力されたメッシュの3D座標に合わせて調整する必要があります。カメラの位置と画角は以下の設定にすると、顔のメッシュが正しくレンダリングされます。

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-14.59.48.jpg" width="320"><img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-15.00.56.jpg" width="320">

5. 最終的に下記のように配線すると、顔のメッシュが生成されます。カメラ内の顔と生成されたメッシュがピッタリと重なります。

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-15.06.00-scaled.jpg" width="640">

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-14.53.49.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/11/01_faceTracker.toe)


### Face Meshにノイズをマッピング

生成されたFace Meshは、SOPのオペレーターとして扱えるため、これまでと同様に様々なオペレーターを組み合わせて加工することが可能です。例えば、Face Meshにノイズをマッピングして、顔の表面に動的な変化を加えることができます。

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-15.12.29-scaled.jpg" width="640">

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-15.13.04.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/11/02_faceTrackerNoise.toe)

### Hand Trackingで手の座標を取得

hand_tracking.toxを使用すると、両手の各指の細かな座標をリアルタイムで取得することができます。これにより、手の動きやジェスチャーをインタラクティブな作品に応用することが可能です。

各指の関節の座標は、hand_tracking.toxのinstance_dataからCHOPとして出力されます。この情報を利用して3DオブジェクトをInstanceすることで、手の動きに応じてオブジェクトを操作することができます。

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-15.26.35-scaled.jpg" width="640">

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-15.28.15.jpg" width="640">


[ダウンロード](https://github.com/tado/tdexamples/blob/main/11/03_handTracker.toe)



### ユーザーインターフェイスとしてのHand Trackingの活用

Hand Trackingで取得した手の座標を利用して、ユーザーインターフェイスとしての操作を実現することも可能です。そのためには、特定の指の座標を抽出する必要があります。例えば、両手の人差し指の先端の座標を取得し、その位置に球体を配置し、さらにその間を直線で結んでみましょう。ここでは、hand_tracking.toxのinstance_dataから、特定の指の座標のみを抽出するために、Trim CHOPを使用しています。

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-17.20.17-scaled.jpg" width="640">

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-17.22.45.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/11/04_handTrackerUI.toe)

さらに、両手の親指の先の座標も取得して、生成される四角形によってカメラ入力の映像に変化をつけてみましょう。ここでは、指の座標をの情報をLens Distort TOPのパラメータに接続することで、カメラ映像の歪みを制御しています。

この手法を応用して、オーディオの制御、パーティクルの挙動の制御、さらにはインタラクティブインスタレーションの制作など、様々な表現に展開することが可能です。

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-17.28.05-scaled.jpg" width="640">

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-02-at-17.28.21.jpg" width="640">

[ダウンロード](https://github.com/tado/tdexamples/blob/main/11/05_handTrackerDistort.toe)