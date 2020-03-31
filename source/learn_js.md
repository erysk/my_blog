---
title: RubyistがJavascriptの基礎学習をしてみる
date: 2020-3-31
tags: ["Javascript"]
excerpt: Javascript何もわからん
---

Rubyしか書けないし、Rubyしか書いてこなかったので、
それ以外の言語も触れるようになりたいなと思って、
リファレンス等を見ながら何となくでやってたところをしっかり理解しようという意図。

というか今までJavascriptよくわかってないのによく業務こなせてたな...。

雰囲気でコード書いてるのでウンコード量産してそう、すまん。

# クラス

そもそもJavascriptのRubyでいうクラスメソッドみたいなのがよくわかっていなかった。

```javascript
Example.foo();
```
みたいなやつ。
Javascriptって関数以外にメソッドも定義できるの？くらい何もわかってない。

## クラス宣言とクラス式

クラスを用意するにはクラス宣言とクラス式の2つの定義方法があるみたい。

Rubyでいうところの以下みたいなものだろうか。

```ruby
# クラス宣言にあたる記法
class Name; end

# クラス式にあたる記法
Name = Class.new
```

多分おそらくだけどクラス宣言で定義するのが一般的だと思うので、そちらに絞って学習を続ける。

ちなみにどちらもホイスティング問題というのがあるらしい。

> クラスにアクセスする前に、そのクラスを宣言する必要があります。そうしないと、ReferenceError がスローされます

## クラス本体の記述

クラス本体は[Strictモード](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Strict_mode)というもので実行されるらしい。
通常エラーにならないけど、バグや落とし穴になりそうなところでエラーが起きるようになるものみたい。

あとはちょっと高速だったり、定義予定の構文を禁止(将来の予約後となる名前の使用禁止ってこと？)したりするとか。

要は厳密なコードを書く必要がありますよ、といったところだろうか。

### constructor

> `class`によって生成されるオブジェクトの生成や初期化を行う特別なメソッドです。

Rubyでいうところの`initialize`メソッドのことかな？

各クラスに1つしか定義できず、2回以上定義されると`SyntaxError`を発生させる。

```javascript
class Name{
    constructor(){}
    constructor(){}
}
// Uncaught SyntaxError: Classes may not have a field named 'constructor'
```

```ruby
class Name
  def initialize; end
  def initialize; end
end
# SyntaxOK
```

Rubyは何度でも定義できるのでちょっと差異があるけど、同じようなものという認識でいていいかな。

```javascript
class Name{
    constructor(foo){console.log(foo)}
}
new Name('Hello Javascript')
// Hello Javascript
```

```ruby
class Name
  def initialize(foo)
    puts foo
  end
end
Name.new('Hello Ruby')
# Hello Ruby
```

戻り値の違いはあるものの同じ感じ。

### プロトタイプメソッド

サンプル
```javascript
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
  // ゲッター
  get area() {
    return this.calcArea();
  }
  // メソッド
  calcArea() {
    return this.height * this.width;
  }
}

const square = new Rectangle(10, 10);

console.log(square.area); // 100
```

まぁ、サンプルパッと出されても何もわからないのでRubyに書き直して咀嚼する。

```ruby
class Rectangle
  def initialize(height, width)
    @height = height
    @width = width
  end
  
  attr_reader :height, :width
  
  def area
    calc_area
  end
   
  def calc_area
    height * width
  end
end

square = Rectangle.new(10, 10);

print square.area # 100
```

こんな感じかな。ゲッターとある部分を単なるメソッドにしているのが意味合いを変えてしまっている気がしないでもないが...。

でもRubyのゲッターも単なるメソッドではあるので間違いではないか。

`this`をどう解釈すればいいのかわからない、Rubyの`self`ではなさそう...。

リファレンスに[メソッド定義](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Functions/Method_definitions)を参照するように書いてあるので見てみる。

...省略形のメソッド定義構文が出てきた。省略前もよくわかってないのに理解できない...。

ページ内に`getter`と`setter`のリンクがあったので、Rubyに書き換えた時の疑問を解消すべくそこを確認してみる。

### ゲッター

> `get`構文は、オブジェクトのプロパティを関数に結びつけ、プロパティが参照された時に関数が呼び出されるようにします。

まぁ当然ながら「プロパティ」や「関数」という概念すら雰囲気でやってきたのでよくわかっていない。

例のごとくサンプルをRubyに書き換えて咀嚼。

```javascript
const obj = {
  log: ['a', 'b', 'c'],
  get latest() {
    if (this.log.length == 0) {
      return undefined;
    }
    return this.log[this.log.length - 1];
  }
}

console.log(obj.latest);
// expected output: "c"
```

```ruby
obj = Class.new do
  @log = ['a', 'b', 'c']

  class << self

    attr_reader :log

    def latest
      return if log.size == 0
      
      log[log.size - 1]
    end
  end
end

print obj.latest
# expected output: "c"
```

こんな感じかな。`get`はやっぱりRubyのメソッドと同じような扱いでいいんだろうな。

### セッター

全くわからないけど、流れ的にRubyの`attr_writer`的なものかなとみる。

例のごとくサンプルをRubyに書き換えて咀嚼。

```javascript
const language = {
  set current(name) {
    this.log.push(name);
  },
  log: []
}

language.current = 'EN';
language.current = 'FA';

console.log(language.log);
// expected output: Array ["EN", "FA"]
```

```ruby
language = Class.new do
  @log = []

  class << self
  
    attr_reader :log

    def current=(name)
      @log << name
    end
   end
end

language.current = 'EN'
language.current = 'FA'

print language.log
```

こんな感じか。Rubyで書くと無理やりクラスを登場させているのでなんとなくわかりづらい感じもするが...。

これは個人のメモみたいなものなので気にせずにいこう。

話は戻ってJavascriptのゲッターとメソッドの違いを理解しないといけない。

ゲッターはRubyでいうメソッドのようなもの、処理の戻り値が返ってくる。

逆にJavascriptのメソッドをRubyのような形で呼び出すと、関数が返ってくるみたい。

```javascript
class Sample {
    method() {
        console.log("Hello Javascript")
    }
}

obj = new Sample;
obj.method;

// ƒ method() {
//     console.log("Hello Javascript")
// }
```

いまいちよくわからないけど、こういうもんなのかな。

### 静的メソッド

`static`キーワードでクラスに静的なメソッドを定義できるみたい。

> 静的メソッドは、クラスのインスタンス化なしで呼ばれ、インスタンス化されていると呼べません。

インスタンス化なしで呼ぶということは、これがRubyでいうクラスメソッドにあたるのかな。

いつもの

```javascript
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  static distance(a, b) {
    const dx = a.x - b.x;
    const dy = a.y - b.y;

    return Math.hypot(dx, dy);
  }
}

const p1 = new Point(5, 5);
const p2 = new Point(10, 10);
p1.distance; //未定義
p2.distance; //未定義

console.log(Point.distance(p1, p2)); // 7.0710678118654755
```

```ruby
class Point
  def initialize(x, y)
    @x = x
    @y = y
  end
  
  attr_reader :x, :y

  class << self
    def distance(a, b)
      dx = a.x - b.x
      dy = a.y - b.y

      Math.hypot(dx, dy)
     end
  end
end

p1 = Point.new(5, 5)
p2 = Point.new(10, 10)
# p1.distance # NoMethodError
# p2.distance # NoMethodError

print Point.distance(p1, p2) # 7.0710678118654755
```

`Math.hypot(dx, dy)`のところとか全く同じものが使えるの面白いですね。

最初に睨んだ通り、静的メソッドはRubyのクラスメソッドみたいなものでしたね。

一旦おしまい。続きはまた今度。