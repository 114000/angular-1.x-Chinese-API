# `ng-bind-html`

- Angular@1.4.7
- `ng` 模块中的指令

解析表达式并用一种安全的方式将得到的 HTML 插入到元素中。默认情况下，得到的 HTML 内容会使用
`$sanitize` 服务进行校验。为了可以使用 `ng-bind-html`，需要确保 `$sanitize` 是可用的，
举个例子，在你的模块依赖中引入 `ngSanitize`（它并不在 Angular 的核心代码中）。为了在你的模块中依赖
`ngSanitize`，你需要再你的应用中引入 "angular-sanitize.js" 这个文件。

当你知道某些值是安全的，你也可以跳过这个校验。想要做到这一点需要通过 `$sce.trustAsHtml` 绑定信任的值。
可以在 Strict Contextual Escaping (SCE) 下查看例子（译：在服务里的`ng.$sce`中）

**注意**：如果 `$sanitize` 服务不能使用并且绑定的值也没有被明确地信任，你会获得以个意外（而不是一个漏洞）

## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<ANY ng-bind-html="expression">
...
</ANY>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngBindHtml|`expression`| 需要解析的表达式|


## 例子

> index.html

``` html
<div ng-controller="ExampleController">
 <p ng-bind-html="myHTML"></p>
</div>
```

> script.js

``` javascript
angular.module('bindHtmlExample', ['ngSanitize'])
.controller('ExampleController', ['$scope', function($scope) {
  $scope.myHTML =
     'I am an <code>HTML</code>string with ' +
     '<a href="#">links!</a> and other <em>stuff</em>';
}]);
```

> protractor.js

``` javascript
it('should check ng-bind-html', function() {
  expect(element(by.binding('myHTML')).getText()).toBe(
      'I am an HTMLstring with links! and other stuff');
});
```
