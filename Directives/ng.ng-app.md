# `ng-app`
- Angular@1.4.7
- `ng` 模块中的指令

使用这个指令来自动启动（auto-bootstrap） 一个 AngularJS 应用。`ngApp`
指令指定了一个应用的根元素并且通常被置于距离页面根元素比较近的地方。例如，放在 `<body>` 或者
`<html>` 标签上。

每一个 HTML 文档只能自动启动一个 AngularJS 应用。页面中第一个被找到的存放 `ngApp`
的标签会被定义为这个应用的根元素来自动启动这个应用。要是想在一个 HTML 文档中启动多个应用，
则你必须使用 `angular.bootstrap` 来手动启动他们。而且 AngularJS 之间不能互相嵌套。

你可以指定一个 AngularJS 模块用作一个应用的根模块。当应用启动时，这个模块将会被载入到 `$injector`
(注射器)。它应该包含了应用所需的代码，或者它依赖的其他模块包含了这些代码。可以查看 `angular.module`
来获取更多信息。

下面的例子里，如果 `ngApp` 指令没有被设定到 `html` 元素上，则文档就不会执行编译。 `AppController`
就不会被实例化出来，因此 `{{ a + b }}` 也不会被解析成 `3`。


`ngApp` 是最简单、最常用的方式来启动一个应用。

> index.html

``` html
<div ng-controller="ngAppDemoController">
  I can add: {{a}} + {{b}} =  {{ a+b }}
</div>
```

> script.js

``` javascript
angular.module('ngAppDemo', []).controller('ngAppDemoController', function($scope) {
  $scope.a = 1;
  $scope.b = 2;
});
```

如果使用 `ngStrictDi`（严格的依赖注入），会像下面这样：

> index.html

``` html
<div ng-app="ngAppStrictDemo" ng-strict-di>
    <div ng-controller="GoodController1">
        I can add: {{a}} + {{b}} =  {{ a+b }}

        <p>这里通过显式的标注形式渲染（render）出了结果，说明了控制器实例化成功了。</p>
    </div>

    <div ng-controller="GoodController2">
        Name: <input ng-model="name"><br />
        Hello, {{name}}!

        <p>这里通过显式的标注形式渲染（render）出了结果，说明了控制器实例化成功了。</p>
    </div>

    <div ng-controller="BadController">
        I can add: {{a}} + {{b}} =  {{ a+b }}

        <p>这个控制器不能被实例化是由于使用了自动地函数标注（即严格模式下会失效的一种标注）
          。因此这一部分的内容没有被插值渲染出来，而且还会在你浏览器的控制台报出一个错误。
        </p>
    </div>
</div>
```

> script.js

``` javascript
angular.module('ngAppStrictDemo', [])
// BadController 实例化失败,
// 是由于使用了自动地函数标注儿没有使用明确的标注
// 译： 如果不使用 `ngStrictDi` 这种注入方式也是可以的，只不过代码压缩的时候需要进一步处理。
.controller('BadController', function($scope) {
  $scope.a = 1;
  $scope.b = 2;
})
// 不像 BadController, GoodController1 and GoodController2 并不会实例化失败，
// 因为分别使用了明确数组形式和 注入（`$inject`）属性的方式
using the array style and $inject property, respectively.
.controller('GoodController1', ['$scope', function($scope) {
  $scope.a = 1;
  $scope.b = 2;
}])
.controller('GoodController2', GoodController2);
function GoodController2($scope) {
  $scope.name = "World";
}
GoodController2.$inject = ['$scope'];
```

> style.css

``` css
div[ng-controller] {
    margin-bottom: 1em; padding: .5em; border: 1px solid; border-radius: 4px;
}
div[ng-controller^=Good] {
    border-color: #d6e9c6; background-color: #dff0d8; color: #3c763d;
}
div[ng-controller^=Bad] {
    border-color: #ebccd1; background-color: #f2dede; color: #a94442;
    margin-bottom: 0;
}
```

## 指令信息

这个指令的执行优先级为0级

## 用法

以元素属性的形式使用:

``` html
<ANY
  ng-app="angular.Module"
  [ng-strict-di="boolean"]>
...
</ANY>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|`ng-app`|`angular.Module`| 一个可以加载的应用模块名|
|`ngStrictDi` （可选）|`boolean`| 如果这个属性被设置在了应用的元素上，则注入模式将被设置为严格依赖注入的模式。

这意味着没用使用显示函数标注的应用将不能成功的调用函数（而且这种形式也不利于压缩），
严格的方式也可以作为依赖注入（Dependency Injection）的描述指引和有助于追查错误的根源的调试信息。
