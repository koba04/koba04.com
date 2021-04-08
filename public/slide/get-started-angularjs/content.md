## Angular.js使ってみた

[@koba04](https://twitter.com/koba04)

\#社内勉強会 2013/07/16



## 話すこと

* Angular.jsとは
* 今回試してみた構成
* Tips



## Angular.jsとは



### Angular.jsとは

[http://angularjs.org/](http://angularjs.org/)

* HTML enhanced for web apps!
* View(HTML)とModel(JavaScript Obejct)の結びつけが簡単に出来る
* RoutingやHTTP周りをサポートしてくれるモジュールも含まれてる
* とにかく色々含まれてる(Backbone.js比較)
* DI



### 今回使ったVersion

* 1.0.7 (Stable)を使用
  * [http://code.angularjs.org/1.0.7/docs/api](http://code.angularjs.org/1.0.7/docs/api)
* 1.1.5 (Unstable)ではngSwipeが入ってたり$animatorがあったり使いやすくなってそう
  * [http://code.angularjs.org/1.1.5/docs/api](http://code.angularjs.org/1.1.5/docs/api)



### 使ってみて(++)

* APIが豊富に用意されていてやりたいことが簡単に出来る
  * 反面、APIにないようなことをしようすると...
* ng-repeatを始め、ng-*のdirectivesが強力なのでDOM操作書かなくてよくて楽
* 公式HPにサンプルコードと合わせてテストも書いてあるのはわかりやすい(code > english)



### 使ってみて(--)

* チュートリアルな内容は簡単にできるけど、実際にアプリを作ろうとすると途端に学ぶことが多くなる印象
  * scope周りなど
* 日本語の情報は少ない
* 公式ドキュメントがもう少し整備されてると嬉しい....
  * [Tutorial](http://docs.angularjs.org/tutorial)にあったり[DeveloperGuide](http://docs.angularjs.org/guide/)にあったりそもそも書いてなかったり...



### 超簡単な使い方

$scopeに変数入れるとDOMで使える

    <html ng-app>
      <head>
        <s cript src="lib/angular/angular.js"></ script>
        <s cript>
          function HogeCtrl($scope) {
            $scope.frameworks = ["Backbone.js", "Angular.js", "Ember.js"];
          }
        </ script>
      </head>
      <body ng-controller="HogeCtrl">
        <ul>
          <li ng-repeat="framework in frameworks">{{framework}}</li>
        </ul>
      </body>
    </html>

* [http://docs.angularjs.org/tutorial/step_02](http://docs.angularjs.org/tutorial/step_02)



### DI

依存性をControllerやDirectives、Service毎に定義して使う

    myapp.controller "Hoge", ["$scope", "$http", "myAppService", ($scope, $http, myAppService) ->
      # この中で$scope, $http, myAppServiceを使用することが出来るようになる
      $http.get("/path/to/api").success (data, status) ->
        $scope.hoge = data.hoge
      myAppService.foo()
    ]



### よく使う便利なもの(ngRepeat)

繰り返し処理を簡単に書ける

    # 数値画像を並べる例( nums = [1..3] )
    <img ng-repeat="num in nums" ng-src="/img/num_{{ num }}{{ $last && '_last' || '' }}.png" />

    # こんな感じになる
    <img src="/img/num_1.png" />
    <img src="/img/num_2.png" />
    <img src="/img/num_3_last.png" />

    # $lastの他にも$first, $middle, $indexという特殊変数が使える

[http://docs.angularjs.org/api/ng.directive:ngRepeat](http://docs.angularjs.org/api/ng.directive:ngRepeat)



### よく使う便利なもの(ngShow)

表示の出し分け

    # 出し分ける場合は条件を反転させたり(ng-hideもある)
    <span ng-show="!isLogin()">ログイン</span>
    <span ng-show="isLogin()">ログアウト</span>

[http://docs.angularjs.org/api/ng.directive:ngShow](http://docs.angularjs.org/api/ng.directive:ngShow)



### よく使う便利なもの(ngClass)

クラスを値によって変更させる

    # visibleとhiddenのクラスがshowContent()の結果で出し分けられる
    <div ng-class="{ 'visible': showContent(), 'hidden': !showContent() }">
    </div>

[http://docs.angularjs.org/api/ng.directive:ngClass](http://docs.angularjs.org/api/ng.directive:ngClass)



### ngSrc, ngHrefを使う

srcやhrefの値に{{}}を使ってscopeの値を指定する

→そのままの文字列でリクエストされる

    <img ng-src="/img/{{ item.name }}.png" />
    <!-- <img src="/img/{{ item.name }}.png" /> --> # NG

    <a ng-href="/path/to/{{ item.name }}">{{ item.name }}</a>
    <!-- <a href="/path/to/{{ item.name }}">{{ item.name }}</a> --> # NG

[http://docs.angularjs.org/api/ng.directive:ngSrc](http://docs.angularjs.org/api/ng.directive:ngSrc)
[http://docs.angularjs.org/api/ng.directive:ngHref](http://docs.angularjs.org/api/ng.directive:ngHref)



### $scope

* controllerやdirectives毎に作られる
* $rootScopeまでプロトタイプチェーンのようにチェーンしている
* 「DOMのイベント」や「$httpからのレスポンス」、「$timeoutによるtimer処理」のタイミングで自動的に$scope.$applyが呼ばれてDOMの値も更新されるので、基本的には明示的に呼ぶ必要はない
* Directivesや同期的に反映させたいときには$scope.$applyを使って反映させることが出来る
* 詳細→ [http://docs.angularjs.org/guide/scope](http://docs.angularjs.org/guide/scope)



## 実際にアプリを作ってみて



## 構成



### ディレクトリ構造

    |- coffee -- app --- app.coffee             # Routingなど
    |         |       |- controllers -- ...     # Contorller
    |         |       |- directives  -- ...     # Directives
    |         |       |- services    -- ...     # Service
    |         |- spec -
    |                 |- controllers -- ...   # Contorllerのテスト
    |                 |- directives  -- ...   # Directivesのテスト
    |                 |- services    -- ...   # Serviceのテスト
    |
    | - tmpl  -- pages -- ...           # Controllerに対応するテンプレート
    |         |- directives -- ...      # Directivesに対応するテンプレート
    |         |- header.html            # 結合するHTMLのHeader
    |         |- footer.html            # 結合するHTMLのFooter



### Routing

$routeProviderを使って定義

    myapp.config ['$routeProvider', ($routeProvider) ->
      $routeProvider
        .when "/hoge/",
          controller : "Hoge"
          templateUrl: "hoge.html"
        .when "/foo/",
          controller : "Foo"
          templateUrl: "foo.html"
        .when "/",
          controller : "Top"
          templateUrl: "top.html"
        .otherwise
          controller : "Error"
          templateUrl: "error.html"

[http://docs.angularjs.org/tutorial/step_07](http://docs.angularjs.org/tutorial/step_07)



### template(ng-view)

* $routeProviderと組み合わせることでURLにあわせてng-viewをtemplateで差し替える事ができる

        <body>
          # この部分がRoutingによって書き換えられる
          <div ng-view></div>
        </body>



### template(ng-template)

* ng-templateで全ページのHTMLを書いて、それを1つのHTMLファイルにまとめてる

        # hoge.htmlに対するng-templateがないときはHTTPリクエストされる
        <s cript type="text/ng-template" id="hoge.html">
            :
            :
        </ script>
        <s cript type="text/ng-template" id="foo.html">
            :
            :
        </ script>

* こうすることでtemplateへのHTTPリクエストが発生しない



### template(concat)

* ng-template毎に別ファイルになっていて、grunt-contrib-concatで1ファイルに

        # Gruntfile.coffee
        html:
          src: [
            "tmpl/app/header.html"
            "tmpl/app/directives/**/*.html"
            "tmpl/app/pages/**/*.html"
            "tmpl/app/footer.html"
          ]
          dest: "../index.html"

* TemplateEngineによくあるinclude機能もng-includeとして存在するが未使用
  * [http://docs.angularjs.org/api/ng.directive:ngInclude](http://docs.angularjs.org/api/ng.directive:ngInclude)



### Controller

1つのRoutingに対して、1つのControllerとtemplateが対応する形になっている

    myapp.controller "Hoge", ['$scope', 'myHttp', ($scope, myHttp) ->
        # 必要に応じてHTTPリクエストなげて結果をDOM($scope)にバインド
        myHttp.get('/hoge/').success (data, status) ->
          $scope.hoge = data.hoge
          :
    ]

* Controllerをネストさせることも出来るが今回は未使用
  * HeaderやFooterに共通メニューがある時に使うとよさそう



### Directives

独自のタグやattrを定義して、

DOMが絡むようなロジックを実装出来る

    # こんな感じで使える

    # menu
    <menu />

    # $scope.itemsのデータを5件毎にpaging出来る
    # ライブラリ化して公開したい..
    <pager limit="5" list-name="items" type="xxx"></pager>

    # ng-clickだとスマートフォンで遅いのでtouchイベントにバインド
    <a href="" on-touch="someAction()">xxx</a>

* [http://docs.angularjs.org/guide/directive](http://docs.angularjs.org/guide/directive)
* [http://qiita.com/grapswiz@github/items/878432cb7e04039b06fb](http://qiita.com/grapswiz@github/items/878432cb7e04039b06fb)</small>
  * 貴重な日本語情報



### 簡単なDirectivesのサンプル

touchendにonTouchで定義された関数をバインドする例
<small>実際に使ってるのはclass適用したり色々やってる</small>

      myapp.directive('onTouch',[ () ->
        return (scope, element, attrs) ->
          element.bind('touchend', ->
            # on-touchで定義されている関数を実行してscopeに反映
            scope.$apply attrs['onTouch']
        )
      ]

      # 使う
      <a href="" on-touch="someAction()">xxx</a>



### Service

    myapp.factory 'myService', ['$http', ($http) ->
        :
    ]

* DOMが絡んでこない使いまわす系のものを定義
  * $httpを拡張した独自のHTTP Service
  * 画像のパスを管理するService
  * その他アプリのロジックなど



### Model

* 今回はいわゆるモデルは使用していない
* 使うならmodule.valueを使うのがよい？(Recommended Setupのscript.js)
  * [http://docs.angularjs.org/guide/module](http://docs.angularjs.org/guide/module)
* $resourceを使ってRestfulなモデルを作ることも出来るようだけど試していない
  * [http://docs.angularjs.org/api/ngResource.$resource](http://docs.angularjs.org/api/ngResource.$resource)



## Tips的なこと



### HTTP(Headerの付与)

$httpProviderをつかうことで

$httpを使うリクエスト全てにHTTP Headerを付与できる

    myapp.config ['$httpProvider', ($httpProvider) ->
      $httpProvider.defaults.headers.common["X-MyApp-Header"] =  "XXXXXX"
    ]

[http://docs.angularjs.org/api/ng.$http](http://docs.angularjs.org/api/ng.$http)

* configブロックの中で自分で作ったサービスは使えない？



### HTTP(Cache対策やトラッキング)

$httpを直接使わずにmyAppHttpサービスを作る

    myapp.factory 'myAppHttp', ['$http', ($http) ->

      addCustomQuery = (url) ->
        url += if /\?/.test(url) then "&" else "?"
        url += "t=#{ new Date().getTime() }&tr=#{ getTrackingCode() }"
        return url

      return {
        get: (url) ->
          $http.get addCustomQuery(url)
        post: (url, data) ->
          $http.post addCustomQuery(url), data
        put: (url, data) ->
          $http.put addCustomQuery(url), data
        delete: (url) ->
          $http.delete addCustomQuery(url)
      }
    ]



### HTTP(エラーハンドリング)

responseInterceptorsを使ってResponseのハンドリング

    myapp.factory 'myResponseInterceptors', ['$q', '$location', ($q, $location) ->
     return (promise) ->
       return promise.then(
         # success
         (response) ->
           :
        # error
        ,(response) ->
          # /error/に飛ばす
          $location.path("/error/").replace()
          return $q.reject response
      )
    ]
    $httpProvider.responseInterceptors.push('myaResponseInterceptors')



### Redirect

Angular.jsでRedirect的な動作を実現

    # myAppHttpService
    redirect: (url, params = {}) ->
      $location.path(url).search(params).replace()

    # responseInterceptors
    # 帰ってきたJSONにredirect_urlがあればredirectとして処理する
    return promise.then(
      # success
      (response) ->
        # template取得もここにくるのでそれは除外
        if typeof response.data == "object"
          if response.data.redirect_url
            # ここでは上記myAppHttpServiceは依存が循環するので使えない
            $location.path response.data.redirect_url
            $location.search response.data.params if response.data.params
            $location.replace()
            return $q.reject response
        return response



### HTTP(ローディングアニメーション)

* myAppHttpServiceでHTTPリクエスト前にローディングアニメーションを表示

〜HTTP Request〜

* responseInterceptorsで結果を受け取った時にローディングアニメーションを非表示に



### Minify対策

minifyすることで引数の名前が変更され、DIが動かないことへの対策

NG

    # minifyしなければ問題ない
    myapp.controller "Hoge", ($scope, myHttp) ->
      :

OK

    myapp.controller "Hoge", ['$scope', 'myHttp', ($scope, myHttp) ->
      :
    ]

ServiceやDirectivesをはじめ、DIで定義するところは全部

[http://qiita.com/kawaz/items/363f430d21ec729f1b7d](http://qiita.com/kawaz/items/363f430d21ec729f1b7d)



### 404対策

HTTPのレスポンスをもとに画像パスを生成するような場合は、ng-srcに関数で指定すると404にならない

    # NG controllerが呼ばれたタイミングで/img/.png へのリクエストが来る
    myapp.controller "Hoge", ['$scope', '$http', ($scope, $http) ->
      $http.get('/hoge/').success (data, status) ->
        $scope.item = data.item
    ]
    <img ng-src="/img/{{ item }}.png" />

    # OK
    myapp.controller "Hoge", ['$scope', '$http', ($scope, $http) ->
      $http.get('/hoge/').success (data, status) ->
        $scope.itemImage = ->
          "/img/#{ data.item }.png"
    ]
    <img ng-src="{{ itemImage() }}" />



### HTML Validation

* ng-xxxや独自のタグを使うのが嫌であれば、data-*を付けても動作する
  * data-ng-repeatなど



### Test

* Karma+PhantomJSを使って、Controller部分やDirectives、Serviceのロジックをテスト。
*  必要に応じてsinon.jsでstub、spy化。
  * angular.mock.module使えば出来そうだけどドキュメント...
* DOM周りのテストまではおこなっていない
  * Angular Scenarioを使うと出来るぽい



### Test(Sample)

    describe "Hoge", ->
      $httpBackend = null
      scope = null

      beforeEach module "myapp"
      beforeEach inject ($injector) ->
        $httpBackend = $injector.get '$httpBackend'
        # HTTPリクエストをMock
        $httpBackend.when('GET', /\/hoge\/\/).respond
          item: "egg"
      beforeEach inject ($controller, $rootScope) ->
        scope = $rootScope.$new()
        # Contorller呼ぶ
        $controller 'Hoge', { $scope: scope, $routeParams: { q: "hoge" } }
        # HTTPリクエストをflush(呼び出しを完了する)
        $httpBackend.flush()
      afterEach ->
        $httpBackend.verifyNoOutstandingExpectation()
        $httpBackend.verifyNoOutstandingRequest()

      it "item", ->
        expect(scope.item).toBe("egg")



### Chrome Extension

* AngularJS Batarang
[https://chrome.google.com/webstore/detail/angularjs-batarang/ighdmehidhipcmcojjgiloacoafjmpfk](https://chrome.google.com/webstore/detail/angularjs-batarang/ighdmehidhipcmcojjgiloacoafjmpfk)

* NgScopeの状態とかWatchのコストが見たり出来る
* といいながら入れてみたもののまだそんなに使ってない...



### Angularに対応したライブラリもある

* [AngularUI](http://angular-ui.github.io/)
* [Angular Kendo UI](http://kendo-labs.github.io/angular-kendo/)



### 使ってみて

* APIから結果もらってDOMに反映させるシンプルなアプリは作りやすい
* アニメーションをバリバリ入れたりフロント側にロジックがバリバリ入っているようなアプリだと辛いかも。
* 公式HPの各ページにあるDISQUSやStackOverflowやGoogleGroupに有益な情報あることが多い
* モバイル向けで使ってるけどパフォーマンスは気にならない
* 管理画面にもよさそう(使ってる)



### 使ってみて(感想)

* 全容を理解してからではなく、試行錯誤で実装していく感じだった
  * 今でもわかってない部分はいっぱいある..
* 情報があまり見つからずこの方法で果たして正しいのかという不安感...
* Backboneと比較して楽に実装したい部分もあって選んだけど、結果的に学ぶことはものすごく多かった
  * とはいえ慣れてくるし、angular.jsに任せておけることも多い
* これから個人で作るものもangular.jsでやりたいなぁと思った



## おしまい

[http://koba04.com/slide/get-started-angularjs/](http://koba04.com/slide/get-started-angularjs/)

