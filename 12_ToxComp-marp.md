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

# TouchDesigner応用編 5 - プロジェクトの構造化とGUI、Container、Widget

---

## 今日の内容

- プロジェクトの構造化とGUI
- TouchDesignerプロジェクトの階層構造を理解する
  - Container COMP
  - inとout
- GUI (グラフィクスユーザインタフェイス) を作る
  - UI COMP
  - Widget COMP

---

## プロジェクトの構造

---

## プロジェクトの構造

- 今まで作業していたNetwork Editorで「u」キーを押してみる

![height:300](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-01.png) ![height:300](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-02.png)

---

## プロジェクトの構造

- project1、perform、local から構成される画面に
- 今まで作成してきたプログラムは、project1というContainer COMPの中身だった
- project1やperformのパラメータを調整して表示の詳細を設定可能

![height:340](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-02.png) ![height:340](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-03.png)

---

## プロジェクトの構造

- TouchDesignerのプロジェクトはContainerの階層構造になっている
  - Container COMPをダブルクリックで中に入る
  - 「u」キーで上の階層へ
- 作成しているProject内にさらにContainerを追加することも可能
  - サブパッチのような機能を果たす
  - inが入力
  - outが出力

---

## プロジェクトの構造

- 試しに、ムービーや画像を回転させるContainerを作ってみる
  - 入力 : 回転させる元画像TOP
  - 出力 : 回転させたTOP
- まず、Container COMPを配置してダブルクリック

![height:380](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-04.png)

---

## プロジェクトの構造

- 以下のようなパッチを作成
- 作成終了したら「u」で上の階層へ

![height:400](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-05.png)

---

## プロジェクトの構造

- Containerに画像や動画を入力すると、回転されて出力される

![height:360](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-06.png)

---

## プロジェクトの構造

- Containerの外部から内部のパラメータも設定可能
  - 例えば、回転の速度を変化できるように
- まず、Containerの外部にConstant CHOPを配置してパラメータ表示
- Constantの値の上で右クリックして「Copy Parameter」を選択
- Container内のLFOのfrequencyの数値で「Paste Reference」を選択

![height:280](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-07.png) ![height:280](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-08.png)

---

## プロジェクトの構造

- Containerの外部からパラメータをコントロールできるように
- このパッチを使用してGUIを追加していきます!

![height:440](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-09.png)

---

## プロジェクトの構造

- 参考: Container COMPのComponent Editorを使用する方法
  - Container COMPの設定パネルの中にパラメーターを入れることも可能
  - 実際に作成しながら解説します

![height:400](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-10.png)

---

## GUIを作成する - 基本編

---

## GUIを作成する - 基本編

- 先程のプログラムを元に回転速度を変更するGUI (スライダー) を追加してみます
- Constant CHOPを削除し、Container COMPを追加、ラベルを「ui」に
- パラメータのChildrenのAlignを「Top to Bottom」へ

![height:380](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-11.png)

---

## GUIを作成する - 基本編

- uiをダブルクリック
- パレットの、UI > Basic Widget からSliderHorzを選択して追加
- パラメータのLabel > Widget Label を「Rotation Speed」に

![height:380](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-12.png)

---

## GUIを作成する - 基本編

- sliderHorzのValuesのvalueをCopy Parameter
- Container1内のLFOのFrequencyに、Paste Reference

![height:340](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-13.png) ![height:340](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-14.png)

---

## GUIを作成する - 基本編

- F1キーでプロジェクトを再生
- 左上に、回転スピードを変更するGUIが追加されているはず

![height:440](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-15.png)

---

## GUIを作成する - 応用編

---

## GUIを作成する - 応用編

- 超簡易VJパッチ
- ムービーの切り替え、各種エフェクトを行うことのできるGUIを作成します!
- 詳細はパッチで

![height:440](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-16.png)

---

## 実習

---

## 実習

- 配布するムービーを素材にしてGUIでいろいろ変化させてみる
  - スピード
  - エフェクト
  - 合成
  - ...etc.
- GUIのレイアウトもいろいろ工夫してみる
- 授業の最後で発表します

---

## 実習

- 参考: Widgetに関するチュートリアル
- https://www.youtube.com/playlist?list=PLSqkC3f_BStxA4TpF6uWj3HOHPCglRpN9

![height:400](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-17.png)