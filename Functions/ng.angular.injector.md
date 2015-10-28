# `angular.injector`
- Angular@1.4.7
- `ng` 模块中的函数

创建一个可用于查找服务和依赖注入的注射器对象。（请看开发向导中的依赖注入）

## 用法

`angular.injector(modules, [strictDi])`

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|modules|`Array.<string/Function>`| 模块函数或其代称的列表，请看 `angular.module`。`ng` 模块必须明确地添加进来。|
|strictDi（可选）|`boolean`|注射器是否使用严格注入模式，这种模式不允许使用参数名注解自身推断的方式<br>（默认: false）|

#### 返回

`injector`	注射器对象，详细请看 `$injector`

## 例子

典型用法：

``` javascript
// 创建一个注射器
var $injector = angular.injector(['ng']);

// 使用注射器启动你的应用
// 使用类型推断方式自动注入参数，或者使用静默注入
$injector.invoke(function($rootScope, $compile, $document) {
  $compile($document)($rootScope);
  $rootScope.$digest();
});
```

有时候你希望从外部的 Angular 访问当前运行的 Angular 应用的注射器。或者，你希望在应用启动后注入或者编译一些标记装饰器。
你可以使用在 JQuery/jqLite 元素上拓展的 `injector()` 方法来实现。请看 `angular.element`。

在下面的例子中，使用了 JQuery 将一个包含了 `ng-controller` 指令的新的 HTML 块插入了 `body` 的末尾。
然后我们编译并将其连接到当前的 Angular 作用域。

``` javascript
var $div = $('<div ng-controller="MyCtrl">{{content.label}}</div>');
$(document.body).append($div);

angular.element(document).injector().invoke(function($compile) {
  var scope = angular.element($div).scope();
  $compile($div)(scope);
});
```
