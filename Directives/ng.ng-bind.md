# `ng-bind`
- Angular@1.4.7
- `ng` 模块中的指令

`ngBind` 指令告知 Angular 使用表达式（expression）给定的 HTML 元素来替代文本内容，
并且当表达式的值改变时去更新文本内容。

通常情况下，你不会直接的使用 `ngBind` 指令，而是使用双花括号来代替，就像这样 {{ 你的表达式 }}，
这两种方法是类似的，但是后者更为简洁。

如果一个模板会在 Angular 编译完成之前，浏览器就会将它的原始状态瞬间展示出来的话，那么使用 `ngBind`
来代替 `{{ 表达式 }}` 会更好一点。因为 `ngBind` 是一个元素的属性，它会在页面加载的过程中无形地绑定到用户上。

另一种替代的解决方案是使用 `ngCloak` 指令。

## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<ANY ng-bind="expression">
...
</ANY>
```

用作 CSS 类

``` html
<ANY class="ng-bind: expression;"> ... </ANY>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngBind|`expression`| 需要解析的表达式|


## 例子

在实时预览的文本盒子内输入一个名字，下面的内容会瞬间改变。

> index.html

```html
<script>
angular.module('bindExample', [])
.controller('ExampleController', ['$scope', function($scope) {
  $scope.name = 'Whirled';
}]);
</script>
<div ng-controller="ExampleController">
  <label>Enter name: <input type="text" ng-model="name"></label><br>
  Hello <span ng-bind="name"></span>!
</div>
```

> protractor.js

``` javascript
it('should check ng-bind', function() {
  var nameInput = element(by.model('name'));

  expect(element(by.binding('name')).getText()).toBe('Whirled');
  nameInput.clear();
  nameInput.sendKeys('world');
  expect(element(by.binding('name')).getText()).toBe('world');
});
```
