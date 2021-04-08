## JavaScript入門

[@koba04](https://twitter.com/koba04)

\#新人向け勉強会 2013/05/07



## 話すこと

* 関数とか変数とか配列とかオブジェクトとかその辺りの基本的なものをさらっと
* 知っておくべきParts
* 開発周辺の話



## JavaScriptとは



### 省略

[http://ja.wikipedia.org/wiki/JavaScript](http://ja.wikipedia.org/wiki/JavaScript)



### 使われるところ
  * Browser
    * まぁアレですね

  * Server
    * NodeとかRhinoとか

  * ネイティブよりなところ
    * Titanium Mobile
    * Unity



### Browserの上で何を書く?
  * jQueryでDOMを触ったりajaxでデータを取ってきたり
  * CanvasやCSS3でアニメーション書いたり
  ****
  * 今後サーバーはjsonを返すだけの役割に?
  * ますますJavaScript側にロジックが寄るのは間違いない
    * フレームワークやテストなど環境も整ってきている



### というわけで逃れられない言語
### それがJavaScript



### というわけで勉強する



### 変数
varは必ず付けるべし

    // 変数
    var name = "oreore";

    // 配列
    var names = ["ieyasu", "hidetada", "iemitsu"];

    // new Arrayは使わない
    names = new Array(100);
    // ???
    console.log(names.length);
    console.dir(names);

    // オブジェクト
    var programmingLanguage = {
      pl: "perl",
      js: "javascript",
      rb: "ruby",
    };

↓


### 変数
    var names = new Array(100);
    console.log(names.length); // ???
    console.dir(names);
    // 100 !!!
    // Array[100]
    // 100のundefinedの値を持つ配列が作られている




### オブジェクト

    var artist = {
      who: "my generation",
      "led-zeppelin": "heartbreaker"
    };
    artist.who;
    artist["led-zeppelin"];

    artist2 = artist;
    artist2.who = "kids are alright";
    // ??
    console.log(artist.who);

↓


### オブジェクト

    artist.who
    // kids are alright
    // オブジェクトは参照渡し



### 関数

    var play = function() {
      console.log("xxxx");
    };
    function play() {
      console.log("xxxx");
    }
    play();
    new play(); // ??

    var count = 0;
    var play = function play2() {
      console.log("xxxx");
      if ( count === 0 ) {
        ++count;
        play2();
      }
    };
    play2(); // ??

    var toArray = function() {
      return Array.prototype.slice.call(arguments);
    };

↓


### 関数

    function play() {
      console.log("xxxx");
    }
    play();
    // xxxx
    new play();
    // xxxx

    var play = function play2() {
      console.log("xxxx");
      if ( count === 0 ) {
        ++count;
        play2();
      }
    };
    play2();
    // ReferenceError: play2 is not defined



### apply, call

    var myName = function() {
      console.log(this.name);
    };
    // ??
    myName.call({ name: "hoge" });

    var sum = function(a, b) {
      console.log(a + b);
    }
    // ??
    sum(1,2);
    sum([1,2]);
    sum.apply(null,[1,2]);
    sum.call(null, 1, 2);

↓


### apply, call

    var myName = function() {
      console.log(this.name);
    };
    myName.call({ name: "hoge" });

    var sum = function(a, b) {
      console.log(a + b);
    }
    // hoge
    sum(1,2);
    // 3
    sum([1,2]);
    // 1,2undefined
    sum.apply(null,[1,2]);
    // 3
    sum.call(null, 1, 2);
    // 3



### 例外

    try {
        throw {
          name: "SomeException",
          message: "例外起きてますよー",
        };
    }
    catch (e) {
        console.log(e.name + ":" + e.message);
    }
    finally {
        console.log("finally...");
    }

    // SomeException:例外起きてますよー
    // finally...



### 正規表現

    /^hgoe$/gim
    // g Global 複数回マッチする
    // i 大文字小文字
    // m ^$が行ごとにマッチする



### 正規表現(test)

    if (/^hoge$/.test("hogehoge") ) {
      console.log("no match");
    }
    if (/^hoge$/.test("hoge") ) {
      console.log("match");
    }

    // match



### 正規表現(exec)(1)
    var html = "<html><boby><div>hogehoge</div><p>nonono</p><div>foofoo</div></body></html>";
    var match;
    if ( match = /<html>(.*<p>(.*?)<\/p>.*)<\/html>/.exec(html) ) {
      console.log("[0]:" + match[0]);
      console.log("[1]:" + match[1]);
      console.log("[2]:" + match[2]);
    }

    // [0]:<html><boby><div>hogehoge</div><p>nonono</p><div>foofoo</div></body></html>
    // [1]:<boby><div>hogehoge</div><p>nonono</p><div>foofoo</div></body>
    // [2]:nonono



### 正規表現(exec)(2)

    var html = "<html><boby><div>hogehoge</div><p>nonono</p><div>foofoo</div></body></html>";
    var regex = /<div>(.*?)<\/div>/g;
    while ( match = regex.exec(html) ) {
      console.log(match[1]);
    }

    // hogehoge
    // foofoo



### 正規表現(String#match)

    var html = "<html><boby><div>hogehoge</div><p>nonono</p><div>foofoo</div></body></html>";
    if ( match = html.match(/<html>(.*<p>(.*?)<\/p>.*)<\/html>/) ) {
        console.log("[0]:" + match[0]);
        console.log("[1]:" + match[1]);
        console.log("[2]:" + match[2]);
    }

    // [0]:<html><boby><div>hogehoge</div><p>nonono</p><div>foofoo</div></body></html>
    // [1]:<boby><div>hogehoge</div><p>nonono</p><div>foofoo</div></body>
    // [2]:nonono



### プロトタイプ(1)

    function Artist(name) {
      this.name = name;
    };
    Artist.prototype.getMembers = function() {
      console.log("return members");
    };
    Artist.play = function() {
      console.log("♪");
    };
    var who = new Artist("The Who");

    // ??
    Artist.play();
    Artist.getMembers();
    who.play();
    who.getMembers();

↓


### プロトタイプ(1)

    function Artist(name) {
      this.name = name;
    };
    Artist.prototype.getMembers = function() {
      console.log("return members");
    };
    Artist.play = function() {
      console.log("♪");
    };
    var who = new Artist("The Who");
    // ??
    Artist.play(); // ♪
    Artist.getMembers(); // ERROR!!!
    who.play(); // ERROR!!!
    who.getMembers(); // return members



### プロトタイプ(2)

    function Artist(name) {
      this.name = name;
    };
    Artist.prototype.getMembers = function() {
      console.log("return members");
    };
    Artist.play = function() {
      console.log("♪");
    };
    var who = new Artist("The Who");
    // ???
    console.log(Artist.constructor);
    console.log(Artist.prototype);
    console.log(who.constructor);
    console.log(who.prototype);

↓


### プロトタイプ(2)

    function Artist(name) {
      this.name = name;
    };
    Artist.prototype.getMembers = function() {
      console.log("return members");
    };
    Artist.play = function() {
      console.log("♪");
    };
    var who = new Artist("The Who");
    // ???
    console.log(Artist.constructor); // [Function: Function]
    console.log(Artist.prototype);   // { getMembers: [Function] }
    console.log(who.constructor);    // { [Function: Artist] play: [Function] }
    console.log(who.prototype);      // undefined



### プロトタイプ(3)

    var list = [1,2,3];
    console.log("list is " + list);
    Array.prototype.toString = function() {
      return "oops...";
    };
    console.log("list is " + list);

↓


### プロトタイプ(3)

    var list = [1,2,3];
    console.log("list is " + list);
    Array.prototype.toString = function() {
      return "oops...";
    };
    console.log("list is " + list);

    // list is 1,2,3
    // list is oops...
    // やったらダメ!!!



### 継承的なものの実装例

    var extend = function(parent, child) {
      var F = function() {};
      F.prototype = parent.prototype;
      child.prototype = new F();
      child.prototype.__super__ = parent.prototype;
      child.prototype.constructor = child;
      return child;
    };



### 継承的なものの実装例(使い方)
    var Parent = function() {
      console.log("parent constructor");
    };
    Parent.prototype.someMethod = function() { console.log("base") };

    var Child = extend(Parent, function(params) {
      this.__super__.constructor.call(this, params);
      console.log("child constructor");
      this.name = params.name;
    });
    Child.prototype.someMethod = function() {
      this.__super__.someMethod();
      console.log("child");
    };
    var c = new Child({ name: "kodomo" });
    c.someMethod();

↓


### 継承的なものの実装例(使い方)

    var c = new Child({ name: "kodomo" });
    c.someMethod();

    // parent constructor
    // child constructor
    // base
    // child



### 継承的なもの
* というわけで色々考える必要がある
* BackboneのModelなどを使っていれば隠蔽してくれているのでそんなに意識することはない
* 興味あればBackboneの実装やJavaScriptパターンを読むといいかも



## その他



### 関数スコープ

    (function(window) {
      // 即時関数でスコープを作り出す
      (function() {
        //
      })();
    })(this);

    var name = "hoge";
    (function() {
      console.log(name); // undefined!!!!
      var name = "foo";
    })();

    (function() {
      //
    }).call(this);



### +の罠

    "10" - 5  // 5
    "10" + 5  // 105!!!



### hasOwnProperty

    Object.prototype.hoge = "foo";
    // :
    band = {
      beatles: "uk",
      who: "uk",
      doors: "us"
    };
    for (name in band) {
      // if ( band.hasOwnProperty(name) ) {
        console.log(name);
      // }
    }

    // beatles
    // who
    // doors
    // hoge



### ==

    false == undefined  // false
    false == null       // false
    false == ''         // true
    false == 0          // true



### 配列の長さはキャッシュする

    for (var i=0, l=list.length; i<l; i++) {
        :
    }
    // for (var i=0; i<list.length; i++) {
    //     :
    // }



### callback style

    var listener = function(cb) {
      cb();
    };
    listener(function() {
      //
    });



### closure

    var createCounter = function() {
      var cnt = 0;
      return function() {
        ++cnt;
        console.log(cnt);
      };
    };
    var counter = createCounter();
    counter(); // 1
    counter(); // 2



### chain

    Klass.prototype.setName() = function(name) {
      this.name = name;
      return this;
    };



## 開発



### DeveloperToolsと仲良くなる

* Chrome Developer Tools
* Firebug
* Webインスペクタ(Safari)
* 開発者ツール(IE)



### テスト

* これは別途やります(やりたい)
* ただ、テストといってもどこをテストしたいのか決めるのが重要で、それによって使うライブラリも変わってくる
* 参考 [http://www.slideshare.net/teppeis/javascript-testwhywhathow](http://www.slideshare.net/teppeis/javascript-testwhywhathow)



### npm

* [https://npmjs.org/](https://npmjs.org/)
* nodeについてくるpackage manager
* package.jsonでproject毎に管理できるので気軽にpackageを入れられる
  * 不要な再発明はしない

    npm install....



### grunt.js
* [http://gruntjs.com/](http://gruntjs.com/)
* JavaScriptのビルドツール
  * もうね、必須
* minify, concat, coffeeやcompassなど各種compile...



### bower
* [http://bower.io/](http://bower.io/)
* jQueryとかBackboneとかのライブラリを管理してくれる
* bower.jsonにpackage.jsonみたいな感じでライブラリとかバージョンとか書いて「bower install」で入れられる
  * インストール先などの設定は.bowerrcに書く
* Twitter製。対応しているライブラリも増えてきている



### yeoman
* [http://yeoman.io/](http://yeoman.io/)
* gruntとかbowerとかを組み合わせてWebアプリケーションの雛形を作ってくれる
* Backbone.jsやAngular.jsなどフレームワークに応じた構成にしてくれる
* coffeeとかcompassのGruntの設定も作ってくれたり、Gruntでlivereloadなサーバー立ちあげてくれたり色々やってくれる



### compile to Javascript
  色々ある

* [CoffeeScript](http://coffeescript.org/)
* [TypeScript](http://www.typescriptlang.org/)
* [Dart](http://www.dartlang.org/)
* [JSX](http://jsx.github.io/)



### MVCフレームワーク(いっぱいある)
* [Backbone.js](http://backbonejs.org/)
  * MVCにコードを分割しやすくしてくれる。薄くてシンプル。
* [Angular.js](http://angularjs.org/)
  * 双方向データバインディングなフレームワーク。色々やってくれる。
* [Ember.js](http://emberjs.com/)
  * 触ってない
* [Flight](http://twitter.github.io/flight/)
  * Twitter製。Component単位でEventドリブンな感じらしい。ちらっとしか見てない。



### 困ったときに
* [MDN](https://developer.mozilla.org/ja/)
  * DOMやJavaScriptの仕様について確認したくなったらまず見るといいサイト。
  * 日本語だったり英語だったり。



### 本を見る
* 個人的に勉強になった本
  * [http://d.hatena.ne.jp/koba04/20130311/1362931533](http://d.hatena.ne.jp/koba04/20130311/1362931533)
* EffectiveJavaScriptはGoodPartsの極端な部分を控えめにしたいい本らしい
  * [http://www.amazon.co.jp/dp/4798131113](http://www.amazon.co.jp/dp/4798131113)



### おまけ(蛇足)



### DOMいじるのは重い

まぁ今ならtemplate使うことが多いと思うけど

    // よくない例
    var $container = $("#container");
    var $ul = $("<ul></ul>");
    $container.append($ul);
    $.each(list, function() {
        $container.append( $("<li></li>").text(this) );
    });

    // DOMの更新は最小限に
    var $container = $("#container");
    var $ul = $("<ul></ul>");
    $container.append($ul);
    $.each(list, function() {
        $ul.append( $("<li></li>").text(this) );
    });
    $container.append($ul);



## おしまい

[http://koba04.com/slide/for-javascript-begineers/](http://koba04.com/slide/for-javascript-begineers/)

