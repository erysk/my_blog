---
title: Reactチュートリアルをやってみた
date: 2019-6-30
tags: ["React", "JavaScript"]
excerpt: ReactはFacebook製のJavaScriptライブラリです。
---

## 前置き
会社でRailsアプリのフロント部分にReactを使おうという話になりまして、  
僕自身Reactに詳しくなかったので、チュートリアルを行い予習をすることに。  
これはそのメモとか感想みたいなものを書こうかなと。  
特に内容のあるものではないと思うので悪しからず。

[Reactチュートリアル](https://ja.reactjs.org/tutorial/tutorial.html)

## 準備
まず、Web上のエディタでコードを書くか、  
ローカルに環境を作ってコードを書くか選択します。  
もちろんローカルに環境を作るのは面倒なのでWeb上を選択。

## 概要
- Reactとは
  > React はユーザーインターフェイスを構築するための、宣言型で効率的で柔軟な JavaScript ライブラリです。複雑な UI を、「コンポーネント」と呼ばれる小さく独立した部品から組み立てることができます。

```javascript
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}
```

- `ShoppingList`はReactコンポーネントのクラス
- コンポーネントはprops(propertiesの略)というパラメータを受け取る
- `render`メソッドでViewに表示する階層構造を返す
- `render`の戻り値であるJSX構文の中では`{}`で囲むことでどこでもJavaScriptが書ける

メモ
- `extends`は、そのクラスが右辺の子クラスであることを示す
  - 今回だと`ShoppingList`は`React.Component`の子クラスである

## スターターコードの確認

スターターコードは以下の通り

```javascript
class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {/* TODO */}
      </button>
    );
  }
}

class Board extends React.Component {
  renderSquare(i) {
    return <Square />;
  }

  render() {
    const status = 'Next player: X';

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}

class Game extends React.Component {
  render() {
    return (
      <div className="game">
        <div className="game-board">
          <Board />
        </div>
        <div className="game-info">
          <div>{/* status */}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
}

// ========================================

ReactDOM.render(
  <Game />,
  document.getElementById('root')
);
```

順に見てみる

```javascript
class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {/* TODO */}
      </button>
    );
  }
}
```

- マス1つを形成するコンポーネント
  - `<button className="square"></button>`でボタン要素を形成している

```javascript
class Board extends React.Component {
  renderSquare(i) {
    return <Square />;
  }

  render() {
    const status = 'Next player: X';

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```

- マスを9つ組み合わせてボードを形成するコンポーネント

  ```javascript
  renderSquare(i) {
    return <Square />;
  }
  ```
  - マスを1つ返すメソッド、iに固有の値を渡すことでマスを識別する
  - `{this.renderSquare(0)}`,`this`はRubyで言う所の`self`みたいなもんか

```javascript
class Game extends React.Component {
  render() {
    return (
      <div className="game">
        <div className="game-board">
          <Board />
        </div>
        <div className="game-info">
          <div>{/* status */}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
}
```

- ボードと状況をレンダリングするコンポーネント
  - 構造的には`Game`>`Board`>`Square`と入れ子になっている
  - `status`と`TODO`部分には動的に値が切り替わるものが今後入るのだろう

```javascript
ReactDOM.render(
  <Game />,
  document.getElementById('root')
);
```

- `ReactDOM.render`で実際にHTML上の該当箇所にレンダリングするのだろう
  - 内容的には`Game`コンポーネントをID`root`の場所にレンダリングするといったところか

## データをpropsで渡す
`Board`クラスを以下のように編集する
```javascript
class Board extends React.Component {
  renderSquare(i) {
    return <Square value={i} />;
  }
  ...
}
```
- `props`として`value`という名前の値を`Square`に渡す
  - Railsの`partial`で例えると以下のような感じか
  ```ruby
  render 'Square', value: i
  ```
  
渡された値を表示できるように`Square`クラスの中身を編集する
```javascript
class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {this.props.value}
      </button>
    );
  }
}
```
- `this`の使い方に慣れるのと`props`が必ずついてくるのに注意ってとこか
  - `props`はRailsで言うパラメータの`params`みたいなもんやな

## インタラクティブなコンポーネントを作る

Squareコンポーネントがクリックされた場合に"X"と表示されるようにする

手始めにクリックされたらアラートを表示するように該当する`Square`クラスに手を加える
```javascript
class Square extends React.Component {
  render() {
    return (
      <button className="square" onClick={ function() { alert('click'); }}>
        {this.props.value}
      </button>
    );
  }
}
```
- `onClick`に渡されるのは関数なので`onClick={alert('click')}`としてしまうと  
関数が読み込まれるたびに`alert`が発火するぞ。

- 以後は`function() {}`の代わりにアロー関数構文が使われる
  > タイプ量を減らして this の混乱しやすい挙動を回避するため、この例以降ではアロー関数構文をつかってイベントハンドラを記述します。
  
  - `function() { alert('click'); }`は`() => { alert('click') }`とかける
  > this の混乱しやすい挙動
  
  - `function() { alert(this.props.value); }`と書いてもエラーになるが  
  `() => { alert(this.props.value) }`と書くと期待通り動く、この辺りの話か

値を記憶し、変更された場合もそれを記憶するためには`state`を用いる

Reactコンポーネントはコンストラクタで`this.state`を設定することで、状態を持つことができる
- `this.state`はそれが定義されているコンポーネント内でプライベートと見なされる

```javascript
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }
  ...
}
```
- `props`として渡されるものに対して`this.state = {}`で設定されたキーは「状態」となり記憶される
- 普段Ruby書いてると`nil`と書きがち、`nil`ではなく`null`なのに注意だな
- nullの後にカンマが付くのもなんか気持ち悪いけどそういうもんなのかな
> JavaScript のクラスでは、サブクラスのコンストラクタを定義する際は常に`super`を呼ぶ必要があります

実際にクリックで"X"が表示されるようにする
```javascript
class Square extends React.Component {
  ...
  render() {
    return (
      <button
      className="square"
      onClick={() => { this.setState({ value: 'X' }) }}
      >
        {this.state.value}
      </button>
    );
  }
}
```
- `this.setState`を呼び出される度に再レンダリングされる
  - 再レンダリングされた時には`this.state.value`は"X"になっているので画面上には"X"が表示される
> setState をコンポーネント内で呼び出すと、React はその内部の子コンポーネントも自動的に更新します。

## ゲームを完成させる
comming soon...
