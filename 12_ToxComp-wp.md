# TouchDesigner応用編 5 - プロジェクトの構造化とGUI、Container、Widget

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-16.01.01-scaled.jpg" width="640">

## 今日の内容

本日のレクチャーでは、TouchDesignerのプロジェクトの構造化と、GUI（グラフィカルユーザーインターフェイス）の作成方法について解説します。

TouchDesignerのプロジェクトは、Containerというコンポーネントの階層構造として組み立てられています。今まで何気なく作業していたNetwork Editorも、実はこの階層構造の中のひとつの階層に過ぎません。プロジェクトの階層構造を理解し、Container COMPとinとoutによる入出力を活用することで、複雑になりがちなパッチを機能ごとに整理して構造化できるようになります。

さらに後半では、この構造化の知識を活かして、スライダーやボタンなどのGUIを作成する方法を学びます。UI COMPとWidget COMPを使うことで、パラメータを直感的に操作できるインターフェイスをプロジェクトに追加できます。最終的には、メインのウィンドウに作品の最終出力を、サブウィンドウにGUIを表示する、複数ウィンドウ構成のプロジェクトへと発展させます。

## プロジェクトの構造

まず、TouchDesignerのプロジェクトがどのような構造になっているのかを確認してみましょう。今まで作業していたNetwork Editorで「u」キーを押してみてください。

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-01.png" width="640">

すると、project1、perform、local から構成される画面に切り替わります。つまり、今まで作成してきたプログラムは、project1というContainer COMPの中身だったのです。project1やperformのパラメータを調整することで、ウィンドウの解像度など表示の詳細を設定することが可能です。

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-02.png" width="320"><img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-03.png" width="320">

## Container COMPにまとめる

このように、TouchDesignerのプロジェクトはContainerの階層構造になっています。Container COMPをダブルクリックするとその中の階層に入ることができ、「u」キーで上の階層へ戻ることができます。

また、作成しているプロジェクト内にさらにContainerを追加することも可能です。追加したContainerは、Max/MSPのサブパッチのような機能を果たします。inが入力、outが出力となり、Containerの外部とデータをやり取りすることができます。

### 既存のネットワークをContainer COMPにまとめる

このように、Container COMPを用いることでプロジェクトの階層構造を作ることができ、プロジェクトのネットワークを整理整頓することが可能になります。制作を進めていくとネットワーク上のオペレーターの数はどんどん増えていき、全体の見通しが悪くなりがちです。機能のまとまりごとに階層に分けて整理することで、パッチの構造が把握しやすくなり、あとから修正や再利用をする際にも便利です。

また、最初からContainerの中に作り込んでいくだけでなく、すでに作成したネットワークの一部を、後からまとめてContainer COMPに変換することもできます。まずは「回転するバナナ」の簡単な例で試してみましょう。

元になるプロジェクトは以下のようなものです。Movie File In TOPで読み込んだバナナの画像を、Transform TOPで回転してOut TOPへ出力しています。回転の角度は、LFO CHOPが生成する周期的な値をMath CHOPで角度の範囲にスケーリングし、Null CHOPを経由してTransform TOPのRotateパラメータにエクスポートすることで、時間とともに変化させています。この段階では、これら全てのオペレータがひとつの階層に並んで配置されています。

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-14.43.54-scaled.jpg" width="640">

次に、Container COMPにひとまとめにして入れたいオペレーターを選択します。複数のオペレーターは、ドラッグで囲むか、Shiftキーを押しながらクリックすることで選択できます。この例では、回転の処理に関わる4つのオペレーター (math1、null1_rotate、moviefilein1、transform1) を選択しています。回転のスピードの元になるLFO CHOPと、最終出力のOut TOPは、外部から操作・確認できるように選択に含めず、この階層に残しておきます。

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-14.52.40-scaled.jpg" width="640">

オペレーターを選択した状態で右クリックし、メニューから「Collapse Selected」を選択します。

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-14.45.11-scaled.jpg" width="640">

すると、選択した4つのオペレータが一つのCOMP (base1) に集約されます。このとき、外部に残したオペレーターとの接続は自動的に引き継がれ、COMPの中には入力用のInと出力用のOutが追加されます。LFO CHOPからの入力はCOMPの入力端子に、Out TOPへの出力はCOMPの出力端子に置き換わるため、ネットワーク全体の動作は変わりません。COMPをダブルクリックすると中の階層に入ることができ、まとめられたオペレーターの構成を確認できます。「u」キーで元の階層に戻ります。

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-14.45.38-scaled.jpg" width="640">

## Container COMPにパラメーターを追加する

Container COMPをはじめとしたCOMPには、設定パネル (パラメーターダイアログ) に新規に独自のパラメーターを追加することもできます。数値やボタン、色 (RGBA) など様々な種類のデータを、COMPの外部から入力できるようになります。COMPにまとめた機能を、あたかもひとつのオペレーターのパラメーターのように操作できるようになるので、パッチの再利用性が大きく向上します。

カスタムパラメーターを追加するには、COMPのパラメーターダイアログの「i」アイコンを右クリックして、メニューから「Customize Component...」を選択します。

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-15.06.18-scaled.jpg" width="640">

## GUIを作成する - 基本編

先程の「回転するバナナ」のプログラムを元にして、回転速度などのパラメーターを変更するGUI（スライダー）を追加してみましょう。

まず、GUIを配置するためのContainer COMPを追加して、ラベルを「ui」にします。パラメータのChildrenのAlignを「Top to Bottom」に設定します。この設定によって、uiの中に配置したWidgetが上から下へ自動的にレイアウトされるようになります。

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-11.png" width="640">

次に、先程紹介した「Customize Component...」で開くComponent Editorを使用して、バナナを回転させるCOMPにカスタムパラメーターを追加していきます。Component Editorでは、パラメーター名を指定して、データの種類 (Float、Intなど)、初期値 (default)、最大値・最小値 (range min / range max) といった詳細を設定できます。以下の例では「Size」という名前のFloat型のパラメーターを追加しています。追加したパラメーターは、COMPのパラメーターダイアログのCustomページにスライダーとして表示され、外部から操作できるようになります。

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-15.08.49.jpg" width="320"><img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-15.09.19.jpg" width="320">

続いて、GUIの部品 (Widget) を追加します。uiをダブルクリックして中の階層に入ります。パレットの UI > Basic Widget から、SliderHorzを選択して追加します。パラメータの Label > Widget Label を「Rotation Speed」に設定します。

<img src="https://yoppa.org/wp-content/uploads/2026/07/toxcomp-12.png" width="640">

パレットの UI > Basic Widget には、横スライダーのSliderHorzの他にも、ボタンやトグル、RGBAのカラースライダーなど、様々なGUI部品が用意されています。Widgetが生成した値を、COMPに追加したカスタムパラメーターに関連付けることで、GUIからCOMPの動作を操作できるようになります。

以下の例では、回転速度用とスケール用の2つのSliderHorzと、背景色用のSlider RGBAの3つのWidgetを配置しています。それぞれのWidgetから出力された値をMath CHOPで適切な範囲にスケーリングし、Merge CHOPでひとつにまとめて、バナナを回転させるCOMP (Rotate_Banana) のカスタムパラメーター (Rotate Speed、Scale、Background Color) に関連付けています。

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-15.40.15-scaled.jpg" width="640">

F1キーでプロジェクトを再生してみましょう。画面の左下にGUIが追加されているはずです。スライダーを操作すると、回転速度、スケール、背景色がリアルタイムに変化します。

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-19.23.17.jpg" width="640">

## GUIを作成する - 応用編: 複数ウィンドウを生成する

より本格的な作品の上演を想定して、複数のウィンドウを生成する構成に発展させてみましょう。メインのウィンドウにはプロジェクトの最終出力を表示し、サブウィンドウにはGUIを表示します。このように出力と操作画面を分離することで、観客に見せる映像とオペレーションのためのGUIを別々の画面に配置できます。

複数のウィンドウを開くには、Window COMPを2つ追加して、それぞれに最終出力とGUIを割り当て、同時に開くようにします。以下の例では、Openボタン (Button COMP) とCloseボタンで、2つのWindow COMPの開閉をコントロールしています。

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-16.00.24-scaled.jpg" width="640">

Openボタンを押すと、2つのWindow COMPが同時に開きます。メインのウィンドウにはプロジェクトの最終出力が、サブウィンドウにはGUIが表示され、GUIの操作がメインウィンドウの映像にリアルタイムに反映されます。

<img src="https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-16.01.01-scaled.jpg" width="640">

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
