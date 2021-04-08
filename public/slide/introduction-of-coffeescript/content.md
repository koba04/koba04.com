## Introduction of CoffeeScript

[@koba04](https://twitter.com/koba04)

\#社内勉強会 2013/04/08



## 話すこと

* CoffeeScriptとは
* 書き方
* 使うべき？



## CoffeeScriptとは



### CoffeeScriptとは

    CoffeeScript はプログラミング言語のひとつである。コードはJavaScript のコードに変換される。
    Ruby や Python、Haskell から影響を受けたシンタックスシュガーの導入により、JavaScript に比べ簡潔さと可読性を向上させたほか、配列内包 (Array comprehensions) やパターンマッチといった機能を追加している。
    CoffeeScript により、パフォーマンスを下げることなく、より短いコードでプログラムを記述することができる (JavaScript に比べ 1/3 程度の行数が削減できる)。 2011年3月16日から一時、CoffeeScript は GitHub でもっともウォッチされているプロジェクトであった。

[wikipediaより](http://ja.wikipedia.org/wiki/CoffeeScript)



### CoffeeScriptとは

* [http://coffeescript.org/](http://coffeescript.org/)
* ざっくり言うとJavaScriptに変換してくれるRubyとかPythonぽい言語
* Railsでも採用されている
* githubでは新しいJSのコードは全部coffeeで書く
  * [https://github.com/styleguide/javascript](https://github.com/styleguide/javascript)
* Dropboxはcoffeeで書きなおしたらしい
  * [https://tech.dropbox.com/2012/09/dropbox-dives-into-coffeescript/](https://tech.dropbox.com/2012/09/dropbox-dives-into-coffeescript/)



### 学ぶ

* 公式のサンプルをざっと眺めて
  * [http://coffeescript.org/](http://coffeescript.org/)
* The Little Book on CoffeeScriptの日本語訳をありがたく読んで
  * [http://minghai.github.com/library/coffeescript/](http://minghai.github.com/library/coffeescript/)
* js2coffeeで色々試してみる
  * [http://js2coffee.org/](http://js2coffee.org/)
* Spineのソース読んでみる(やってないけど...)
  * [https://github.com/spine/spine](https://github.com/spine/spine)



# 書き方
公式読むの面倒な人向け



### 変数
    # coffee
    val = "foo"
    list = [
      "foo"
      "bar"
      "baz"
    ]
    list2 = [1..10]
    obj =
      key: "val"
***
    // javascript
    var list, list2, obj, val;

    val = "foo";

    list = ["foo", "bar", "baz"];

    list2 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

    obj = {
      key: "val"
    };



### Alias

    is              # ===
    isnt            # !==
    not             # !
    and             # &&
    or              # ||
    true, yes, on   # true
    false, no, off  # false
    @, this         # this
    of              # in
    in              # 配列にふくまれるか if some in list ...



### 文字列

    url = "http://#{ server.host }/path/#main"
    longText = "むかしむかし
    あるところに..."
    longText = """
               むかしむかし
               あるところに
               """
***
    var longText, url;
    url = "http://" + server.host + "/path/#main";
    longText = "むかしむかし     あるところに...";
    longText = " \nむかしむかし \nあるところに ";



### 正規表現

    someRegex = /^[0-9]+:[A-Z]+:[a-z]+$/
    # perlのmオプションみたいな感じ
    someRegex = ///
      ^       # 最初
      [0-9]+: # 数字
      [A-Z]+: # 大文字アルファベット
      [a-z]+  # 小文字アルファベット
      $       # 最後
    ///
***
    var someRegex;
    someRegex = /^[0-9]+:[A-Z]+:[a-z]+$/;
    someRegex = /^[0-9]+:[A-Z]+:[a-z]+$/;



### コメント

    # このコメントはJSには出力されない
    ###
      このコメントはJSに出力される
    ###
***
    /*
      このコメントはJSに出力される
    */



### クラス

    class Animal
      constructor:(opt) ->
        {@name, @age} = opt
      say: ->
        console.log @name
***
    var Animal;
    Animal = (function() {
      function Animal(opt) {
        this.name = opt.name, this.age = opt.age;
      }
      Animal.prototype.say = function() {
        return console.log(this.name);
      };
      return Animal;
    })();



### クラス(継承)

    class Dog extends Animal
      say: ->
        console.log "bow wow"
        # Animalのdog
        super()
***
    var Dog,
      __hasProp = {}.hasOwnProperty,
      __extends = function(child, parent) { for (var key in parent) { if (__hasProp.call(parent, key)) child[key] = parent[key]; } function ctor() { this.constructor = child; } ctor.prototype = parent.prototype; child.prototype = new ctor(); child.__super__ = parent.prototype; return child; };

    Dog = (function(_super) {
      __extends(Dog, _super);
      function Dog() {
        return Dog.__super__.constructor.apply(this, arguments);
      }
      Dog.prototype.say = function() {
        console.log("bow wow");
        return Dog.__super__.say.call(this);
      };
      return Dog;
    })(Animal);



### クラス(使う)

    taro = new Dog name:"taro", age:2
    taro.say()

    # :: で prototype関数を使いたい時に
    fakeDog =
      name: "cat"
    Dog::say.call fakeDog
***
    var fakeDog, taro;
    taro = new Dog({
      name: "taro",
      age: 2
    });
    taro.say();

    fakeDog = {
      name: "cat"
    };
    Dog.prototype.say.call(fakeDog);



### 関数

    sum = (a, b) ->
      a + b

    hoge = (a, b, opt...) ->
      return

    do ->

    # =>はメソッド内でcallback使う時とかに
    class Foo
      constructor: (name, el) ->
        @name = name
        $(el).on "click", (e) =>
          $(e.target).text(@name)
***
    var Foo, hoge, sum,
      __slice = [].slice;

    sum = function(a, b) {
      return a + b;
    };

    hoge = function() {
      var a, b, opt;
      a = arguments[0], b = arguments[1], opt = 3 <= arguments.length ? __slice.call(arguments, 2) : [];
    };

    (function() {})();

    Foo = (function() {
      function Foo(name, el) {
        var _this = this;
        this.name = name;
        $(el).on("click", function(e) {
          return $(e.target).text(_this.name);
        });
      }
      return Foo;
    })();



### 条件分岐

    if hoge and foo
      console.log "hoge and foo"

    return if isError

    name = if params.name then params.name else defaults.name

    return 0 < hoge < 100

    res = switch score
      when score > 90 then "great"
      when score > 80 then "ok"
      else                 "no"

***
    var name, res;
    if (hoge && foo) {
      console.log("hoge and foo");
    }

    if (isError) {
      return;
    }

    name = params.name ? params.name : defaults.name;

    return (0 < hoge && hoge < 100);

    res = (function() {
      switch (score) {
        case score > 90:
          return "great";
        case score > 80:
          return "ok";
        default:
          return "no";
      }
    })();



### Loop (Array)

    for i in [1..10]
      console.log i
    # 同じ
    console.log i for i in [1..10]

    even = for i in [1..10] when i % 2 is 0
      i

    for i in [1..10]
      do (i) ->
        console.log i
***
    var even, i, _fn, _i, _j, _k;
    for (i = _i = 1; _i <= 10; i = ++_i) {
      console.log(i);
    }

    for (i = _j = 1; _j <= 10; i = ++_j) {
      console.log(i);
    }

    even = (function() {
      var _k, _results;
      _results = [];
      for (i = _k = 1; _k <= 10; i = ++_k) {
        if (i % 2 === 0) {
          _results.push(i);
        }
      }
      return _results;
    })();

    _fn = function(i) {
      return console.log(i);
    };
    for (i = _k = 1; _k <= 10; i = ++_k) {
      _fn(i);
    }



### Loop (Object)

    obj =
      id: 10
    console.log "#{key} is #{val * 10}" for own key, val of obj
***
    var key, obj, val,
      __hasProp = {}.hasOwnProperty;

    obj = {
      id: 10
    };

    for (key in obj) {
      if (!__hasProp.call(obj, key)) continue;
      val = obj[key];
      console.log("" + key + " is " + (val * 10));
    }



### Array

    list = [1..10]
    copy = list[..]
    copy = (i for i in list)[1..2]
    [i, j] = (i for i in list)[1..2]
    [first, other..., last] = "hoge,foo,baz,zzz".split ","
***
    var copy, first, i, j, last, list, other, _i, _ref, _ref1,
      __slice = [].slice;
    list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

    copy = list.slice(0);

    copy = ((function() {
      var _i, _len, _results;
      _results = [];
      for (_i = 0, _len = list.length; _i < _len; _i++) {
        i = list[_i];
        _results.push(i);
      }
      return _results;
    })()).slice(1, 3);

    _ref = ((function() {
      var _i, _len, _results;
      _results = [];
      for (_i = 0, _len = list.length; _i < _len; _i++) {
        i = list[_i];
        _results.push(i);
      }
      return _results;
    })()).slice(1, 3), i = _ref[0], j = _ref[1];

    _ref1 = "hoge,foo,baz,zzz".split(","), first = _ref1[0], other = 3 <= _ref1.length ? __slice.call(_ref1, 1, _i = _ref1.length - 1) : (_i = 1, []), last = _ref1[_i++];



### ?

    hoge.foo()?.baz?()
    name = params.name?.name
***
    var name, _ref, _ref1;

    if ((_ref = hoge.foo()) != null) {
      if (typeof _ref.baz === "function") {
        _ref.baz();
      }
    }

    name = (_ref1 = params.name) != null ? _ref1.name : void 0;



### use JavaScript

    `
      obj = { name: "javascript" };
      alert(obj.name);;;
    `
***
      obj = { name: "javascript" };
      alert(obj.name);;;
    ;



# でどうなのよ



### 雑感

* **気持よく書ける**(ただし人による)
* JavaScriptの学習コスト+CoffeeScriptの学習コストがある
  * 結局JavaScriptからは逃れられない
* コンパイルが必要なのはどうせgruntとか使うだろうし気にならない
* PerlならJavaScriptそのまま書くほうが感覚的には近い
  * ruby書きたくなる



### ご利用は計画的に

* 出力されたJavaScriptがあるとはいえ影響は大きいので、プロジェクトで使う場合は関係者の合意が得る必要があると思う
* プロジェクトで使うのは今の状態だと メリット ***<*** デメリットかなぁという印象だけど、大規模に書く場合でそのことで保守性があがるならアリかも..
  * 大規模なJavaScriptはどっちにしろ他の人読めない...
* ライブラリで使うのはちょっと微妙かも



## おしまい

[http://koba04.com/slide/introduction-of-coffeescript/](http://koba04.com/slide/introduction-of-coffeescript/)

