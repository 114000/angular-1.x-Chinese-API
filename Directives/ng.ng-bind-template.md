# `ng-bind-template`
- Angular@1.4.7
- `ng` 模块中的指令

`ngBindTemplate` 指令会将元素的文本内容赋值为它所代表的模板的插值。并不像 `ngBind` 那样，
`ngBindTemplate` 可以包含 `{{ }}` 表达式。这个指令的存在是由于一些 HTML 标签元素（如
`<title> 和 `<option>`）不能包含 `<span>` 元素

## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<ANY ng-bind-template="string">
...
</ANY>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngBindTemplate|`string`| 为了转化的 {{ 表达式 }} 的模板组 |


## 例子

试一试: 在输入框输入内容观察变化。

> index.html

``` html
<script>
angular.module('bindExample', [])
.controller('ExampleController', ['$scope', function($scope) {
  $scope.salutation = 'Hello';
  $scope.name = 'World';
}]);
</script>
<div ng-controller="ExampleController">
 <label>Salutation: <input type="text" ng-model="salutation"></label><br>
 <label>Name: <input type="text" ng-model="name"></label><br>
 <pre ng-bind-template="{{salutation}} {{name}}!"></pre>
</div>
```

> protractor.js

``` javascript
it('should check ng-bind', function() {
  var salutationElem = element(by.binding('salutation'));
  var salutationInput = element(by.model('salutation'));
  var nameInput = element(by.model('name'));

  expect(salutationElem.getText()).toBe('Hello World!');

  salutationInput.clear();
  salutationInput.sendKeys('Greetings');
  nameInput.clear();
  nameInput.sendKeys('user');

  expect(salutationElem.getText()).toBe('Greetings user!');
});
```
