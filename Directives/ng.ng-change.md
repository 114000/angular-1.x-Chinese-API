# `ng-change`
- Angular@1.4.7
- `ng` 模块中的指令

当用户改变（onchange）表单时，解析给定的表达式。表达式会被立即解析，并不像 JavaScript 中的
onchange 事件那样仅仅会在改变结束之后触发一次。（就是通常当用户离开表中的元素或者按下回车键的时候）

`ngChange` 表达式仅会在表单产生了一个的新值，并且这个新的值被提交到模型（model）的时候被解析执行。

以下情况不会解析执行：

- 从 `$parsers` 改造管道返回的值没有变化。
- 由于模型将会置空（null）表单为无效。
- 模型由程序改变而不是由表单元素的值来改变时。

**注意**：这个指令需要 `ngModel` 作为前提。

## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<input ng-change="expression">
...
</input>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngChange|`expression`| 在表单内的值改变时，需要解析的表达式|

## 例子

> index.html

``` html
<script>
angular.module('changeExample', [])
.controller('ExampleController', ['$scope', function($scope) {
  $scope.counter = 0;
  $scope.change = function() {
    $scope.counter++;
  };
}]);
</script>
<div ng-controller="ExampleController">
  <input type="checkbox" ng-model="confirmed" ng-change="change()" id="ng-change-example1" />
  <input type="checkbox" ng-model="confirmed" id="ng-change-example2" />
  <label for="ng-change-example2">Confirmed</label><br />
  <tt>debug = {{confirmed}}</tt><br/>
  <tt>counter = {{counter}}</tt><br/>
</div>
```

> protractor.js

``` javascript
var counter = element(by.binding('counter'));
var debug = element(by.binding('confirmed'));

it('如果从视图 `view` 改变的，则解析表达式', function() {
  expect(counter.getText()).toContain('0');

  element(by.id('ng-change-example1')).click();

  expect(counter.getText()).toContain('1');
  expect(debug.getText()).toContain('true');
});

it('如果是从模型 `model` 直接改变的，则不解析表达式', function() {
  element(by.id('ng-change-example2')).click();

  expect(counter.getText()).toContain('0');
  expect(debug.getText()).toContain('true');
});
```
