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

## Container COMPにまとめる

- TouchDesignerのプロジェクトはContainerの階層構造になっている
  - Container COMPをダブルクリックで中に入る
  - 「u」キーで上の階層へ
- 作成しているProject内にさらにContainerを追加することも可能
  - サブパッチのような機能を果たす
  - inが入力
  - outが出力

---

## Container COMPにまとめる

- 試しに、ムービーや画像を回転させるContainerを作ってみる
  - 入力 : 回転させる元画像TOP
  - 出力 : 回転させたTOP
- まず、Container COMPを配置してダブルクリック

![height:380](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-04.png)

---

## Container COMPにまとめる

- 以下のようなパッチを作成
- 作成終了したら「u」で上の階層へ

![height:400](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-05.png)


---

## Container COMPにまとめる

- Container COMPを用いることでプロジェクトの階層構造を作ることができる
- プロジェクトのネットワークを整理整頓することが可能
- まずは「回転するバナナ」の簡単な例で試してみる

---

## Container COMPにまとめる

- 元になるプロジェクト
- ひとつの階層に全てのオペレータが配置されている

![height:360](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-14.43.54-scaled.jpg)

---

## Container COMPにまとめる

- Container COMPにひとまとめにして入れたいオペレーターを選択
- この例では4つのオペレーターを選択している

![height:360](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-14.52.40-scaled.jpg)

---

## Container COMPにまとめる

- 右クリックしてメニューから「Collapse Selected」を選択

![height:360](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-14.45.11-scaled.jpg)

---

## Container COMPにまとめる

- 選択したオペレータが一つのContainer COMPに集約される
- ダブルクリックすると、Container COMPの中の階層に行くことができる
- 「u」キーで元に戻る

![height:360](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-14.45.38-scaled.jpg)

---

## Container COMPにパラメーターを追加する

---

## Container COMPにパラメーターを追加する

- Container COMPのプロパティーに新規にパラメータを追加することもできる
- 数値、ボタン、色 (RGBA) など様々なデータを外部入力できるようになる
- Container COMPの「i」を右クリックして、「Costmise Component」を選択

![height:320](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-15.06.18-scaled.jpg)

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

- パラメーターを追加して、データの種類、初期値、最大値、最小値など詳細を指定していく

![height:400](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-15.08.49.jpg) ![height:320](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-15.09.19.jpg)

---

## GUIを作成する - 基本編

- uiをダブルクリック
- パレットの、UI > Basic Widget からSliderHorzを選択して追加
- パラメータのLabel > Widget Label を「Rotation Speed」に

![height:380](https://yoppa.org/wp-content/uploads/2026/07/toxcomp-12.png)

---

## GUIを作成する - 基本編

- パレットから UI > Basic Widget を選択すると様々なGUI部品を追加できる
- 例えば、SliderHorz COMPを追加して横スライダーを追加
- 生成された値を、Container COMPのパラメータに関連付けることで、GUIから操作できるようになる

![height:320](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-15.40.15-scaled.jpg)

---

## GUIを作成する - 基本編

- F1キーでプロジェクトを再生
- 画面左下にGUIが追加されている

![height:440](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-19.23.17.jpg)

---

## GUIを作成する - 応用編: 複数ウィンドウを生成する

---

## GUIを作成する - 応用編: 複数ウィンドウを生成する

- 複数ウィンドウを生成する
- メインのウィンドウにはプロジェクトの最終出力を表示、サブウィンドウにはGUIを表示
- Window COMPを2つ追加して同時に開くようにする

![height:400](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-16.00.24-scaled.jpg)

---

## GUIを作成する - 応用編: 複数ウィンドウを生成する

- Openボタンを押すと、2つのWindow COMPが同時に開く
- メインのウィンドウにはプロジェクトの最終出力を表示、サブウィンドウにはGUIを表示

![height:400](https://yoppa.org/wp-content/uploads/2026/07/Screenshot-2026-07-09-at-16.01.01-scaled.jpg)

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