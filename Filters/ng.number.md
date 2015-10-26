# `number`
- Angular@1.4.7
- `ng` 模块中的过滤器

将数字格式化为字符串。

如果输入的是 `null` 或者 `undefined`，则会原封不动的返回。如果输入是无穷（`Infinity/-Infinity`），
会返回无穷的符号 `'∞'`。如果输入不是数字则返回一个空字符串。

## 用法

HTML 模板绑定中

``` html
{{ number_expression | number : fractionSize}}
```

JavaScript 中

``` javascript
$filter('number')(number, fractionSize)
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|number|`number`<br>`string`| 需要格式化的数字 |
|fractionSize(可选)|`number`<br>`string`| 小数位的取舍量。默认为当前语言地区的最大量。|

#### 返回

`string` 数字会依据小数数位取舍，并且每三个数字会使用 `,` 分隔开。

## 例子

> index.html

``` html
<script>
  angular.module('numberFilterExample', [])
    .controller('ExampleController', ['$scope', function($scope) {
      $scope.val = 1234.56789;
    }]);
</script>
<div ng-controller="ExampleController">
  <label>Enter number: <input ng-model='val'></label><br>
  Default formatting: <span id='number-default'>{{val | number}}</span><br>
  No fractions: <span>{{val | number:0}}</span><br>
  Negative number: <span>{{-val | number:4}}</span>
</div>
```

> protractor.js

``` javascript
it('should format numbers', function() {
  expect(element(by.id('number-default')).getText()).toBe('1,234.568');
  expect(element(by.binding('val | number:0')).getText()).toBe('1,235');
  expect(element(by.binding('-val | number:4')).getText()).toBe('-1,234.5679');
});

it('should update', function() {
  element(by.model('val')).clear();
  element(by.model('val')).sendKeys('3374.333');
  expect(element(by.id('number-default')).getText()).toBe('3,374.333');
  expect(element(by.binding('val | number:0')).getText()).toBe('3,374');
  expect(element(by.binding('-val | number:4')).getText()).toBe('-3,374.3330');
});
```
