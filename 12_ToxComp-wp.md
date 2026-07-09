# TouchDesigner応用編 5 - プロジェクトの構造化とGUI、Container、Widget

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-16.png" width="640">

## 今日の内容

本日のレクチャーでは、TouchDesignerのプロジェクトの構造化と、GUI（グラフィカルユーザーインターフェイス）の作成方法について解説します。

TouchDesignerのプロジェクトは、Containerというコンポーネントの階層構造として組み立てられています。今まで何気なく作業していたNetwork Editorも、実はこの階層構造の中のひとつの階層に過ぎません。プロジェクトの階層構造を理解し、Container COMPとinとoutによる入出力を活用することで、複雑になりがちなパッチを機能ごとに整理して構造化できるようになります。

さらに後半では、この構造化の知識を活かして、スライダーやボタンなどのGUIを作成する方法を学びます。UI COMPとWidget COMPを使うことで、パラメータを直感的に操作できるインターフェイスをプロジェクトに追加できます。最終的には、ムービーの切り替えやエフェクトをGUIで操作する簡易VJパッチの制作を目指します。

## プロジェクトの構造

まず、TouchDesignerのプロジェクトがどのような構造になっているのかを確認してみましょう。今まで作業していたNetwork Editorで「u」キーを押してみてください。

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-01.png" width="640">

すると、project1、perform、local から構成される画面に切り替わります。つまり、今まで作成してきたプログラムは、project1というContainer COMPの中身だったのです。project1やperformのパラメータを調整することで、ウィンドウの解像度など表示の詳細を設定することが可能です。

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-02.png" width="320"><img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-03.png" width="320">

このように、TouchDesignerのプロジェクトはContainerの階層構造になっています。Container COMPをダブルクリックするとその中の階層に入ることができ、「u」キーで上の階層へ戻ることができます。

また、作成しているプロジェクト内にさらにContainerを追加することも可能です。追加したContainerは、Max/MSPのサブパッチのような機能を果たします。inが入力、outが出力となり、Containerの外部とデータをやり取りすることができます。

### 画像を回転させるContainerを作ってみる

試しに、ムービーや画像を回転させる機能をもったContainerを作ってみましょう。入力は回転させる元画像のTOP、出力は回転させたTOPとなります。

まず、Container COMPを配置してダブルクリックして中の階層に入ります。

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-04.png" width="640">

Containerの中で、以下のようなパッチを作成します。In TOPから入力された画像を、LFO CHOPの値を参照したTransform TOPで回転させて、Out TOPから出力しています。作成が終了したら「u」キーで上の階層へ戻ります。

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-05.png" width="640">

上の階層に戻ると、作成したContainerに入力と出力の端子が追加されているはずです。Containerに画像や動画を入力すると、回転されて出力されます。

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-06.png" width="640">

### Containerの外部からパラメータをコントロールする

Containerの外部から、内部のパラメータを設定することも可能です。例えば、回転の速度を外部から変化できるようにしてみましょう。

まず、Containerの外部にConstant CHOPを配置してパラメータを表示します。Constantの値の上で右クリックして「Copy Parameter」を選択します。次にContainer内に入り、LFOのfrequencyの数値の上で右クリックして「Paste Reference」を選択します。

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-07.png" width="320"><img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-08.png" width="320">

これで、Containerの外部からパラメータをコントロールできるようになりました。Constantの値を変化させると、回転のスピードが変化します。次のセクションでは、このパッチを使用してGUIを追加していきます!

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-09.png" width="640">

参考: 今回の方法の他に、Container COMPのComponent Editorを使用する方法もあります。Component Editorを使うと、Container COMPの設定パネルの中に独自のパラメーターを追加することが可能です。こちらは実際に作成しながら解説します。

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-10.png" width="640">

## GUIを作成する - 基本編

先程のプログラムを元にして、回転速度を変更するGUI（スライダー）を追加してみましょう。

まず、Constant CHOPを削除し、代わりにContainer COMPを追加して、ラベルを「ui」にします。パラメータのChildrenのAlignを「Top to Bottom」に設定します。この設定によって、uiの中に配置したWidgetが上から下へ自動的にレイアウトされるようになります。

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-11.png" width="640">

uiをダブルクリックして中の階層に入ります。パレットの UI > Basic Widget から、SliderHorzを選択して追加します。パラメータの Label > Widget Label を「Rotation Speed」に設定します。

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-12.png" width="640">

次に、スライダーの値と回転速度を関連付けます。先程と同様の手順で、sliderHorzのValuesのvalueの上で右クリックして「Copy Parameter」を選択し、container1内のLFOのFrequencyに「Paste Reference」します。

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-13.png" width="320"><img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-14.png" width="320">

F1キーでプロジェクトを再生してみましょう。画面の左上に、回転スピードを変更するGUIが追加されているはずです。スライダーを操作すると、リアルタイムに回転速度が変化します。

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-15.png" width="640">

## GUIを作成する - 応用編

基本編で学んだ内容を応用して、より実践的なGUIを作成してみましょう。ここでは「超簡易VJパッチ」として、ムービーの切り替えや各種エフェクトをGUIから操作できるパッチを作成します。詳細は配布するパッチを見ながら解説します。

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-16.png" width="640">

## 実習

配布するムービーを素材にして、GUIでいろいろと映像を変化させてみましょう。

- スピード
- エフェクト
- 合成
- ...etc.

映像の変化だけでなく、GUIのレイアウト自体もいろいろ工夫してみてください。授業の最後に発表します。

参考: Widgetに関するチュートリアル（動画）

- [TouchDesigner Widgets](https://www.youtube.com/playlist?list=PLSqkC3f_BStxA4TpF6uWj3HOHPCglRpN9)

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-17.png" width="640">
