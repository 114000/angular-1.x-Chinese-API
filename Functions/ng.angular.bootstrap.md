# `angular.bootstrap`
- Angular@1.4.7
- `ng` 模块中的函数

使用这个函数来手动启动 angular 应用。

请看开发向导中的 Bootstrap

**注意**：基于 Protrator 的 E2E 测试不能使用这个函数来手动启动应用。必须使用 `ngApp` 指令。
Angular 会检测重复加载，并只允许首次加载的脚本启动，而为之后加载的脚本在控制台提出警告。
这会防止在应用中有多个 Angular 实例一同试图工作在 DOM 上而发生奇怪现象。

``` html
<!doctype html>
<html>
<body>
<div ng-controller="WelcomeController">
  {{greeting}}
</div>

<script src="angular.js"></script>
<script>
  var app = angular.module('demo', [])
  .controller('WelcomeController', function($scope) {
      $scope.greeting = 'Welcome!';
  });
  angular.bootstrap(document, ['demo']);
</script>
</body>
</html>
```


## 用法

`angular.bootstrap(element, [modules], [config])`

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|element|`DOMElement`| 代表 angular 应用的根的 DOM 元素|
|modules（可选）|`Array<String/Function/Array>=`| 一个想要在应用中引入的模块的数组。数组中的每一项应该是一个预设模块的名字，或者是一个（DI 注解，译：依赖注入）函数，它将被注射器当做一个配置块调用。请看 `angular.module`|
|config（可选）|`Object`|定义应用配置项的对象。支持一下项目：<br>- strictDi - 是应用的自动函数注解失效，有助于在压缩的代码中找到错误。默认为 `false`|

#### 返回

`auto.$injector`	返回为这个应用新创建的注射器。
