# `limitTo`
- Angular@1.4.7
- `ng` 模块中的过滤器

创建一个新的数组或者字符串来包含给定数量的元素。这些元素要么是从原数组（或字符串或数字）
的头部开始截取要么是从尾部截取，这由你指定的值和符号（正或负）来决定。如果输入一个数字，
则它将被转化成字符串。

## 用法

HTML 模板绑定中

``` html
{{ limitTo_expression | limitTo : limit : begin}}
```

JavaScript 中

``` javascript
$filter('limitTo')(input, limit, begin)
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|input|`number`<br>`array`<br>`string`| 需要限制的源数字，数组，字符串 |
|limit|`string`<br>`number`| 返回数组或者字符串的长度。如果截取的数字为正，会从原数组/字符串的开头进行复制。如果为负，会从结尾进行复制。截取在到达原数组/字符串长度后会被修剪。
如果 `limit` 为 `undefined`，返回值将是原数组/字符串|
|begin(可选)|`string`<br>`number`| 截取起始位置的索引值。为负值时，说明从输入的尾部开始计算偏移。默认值为0。|

#### 返回

`string` `array` 指定限制长度的一个新的子数组或者子字符串，如果输入的元素比截取数还要少，
新的子数组或子字符串也会更短。

## 例子

> index.html

``` html
<script>
  angular.module('limitToExample', [])
    .controller('ExampleController', ['$scope', function($scope) {
      $scope.numbers = [1,2,3,4,5,6,7,8,9];
      $scope.letters = "abcdefghi";
      $scope.longNumber = 2345432342;
      $scope.numLimit = 3;
      $scope.letterLimit = 3;
      $scope.longNumberLimit = 3;
    }]);
</script>
<div ng-controller="ExampleController">
  <label>
     Limit {{numbers}} to:
     <input type="number" step="1" ng-model="numLimit">
  </label>
  <p>Output numbers: {{ numbers | limitTo:numLimit }}</p>
  <label>
     Limit {{letters}} to:
     <input type="number" step="1" ng-model="letterLimit">
  </label>
  <p>Output letters: {{ letters | limitTo:letterLimit }}</p>
  <label>
     Limit {{longNumber}} to:
     <input type="number" step="1" ng-model="longNumberLimit">
  </label>
  <p>Output long number: {{ longNumber | limitTo:longNumberLimit }}</p>
</div>
```

> protractor.js

``` javascript
var numLimitInput = element(by.model('numLimit'));
var letterLimitInput = element(by.model('letterLimit'));
var longNumberLimitInput = element(by.model('longNumberLimit'));
var limitedNumbers = element(by.binding('numbers | limitTo:numLimit'));
var limitedLetters = element(by.binding('letters | limitTo:letterLimit'));
var limitedLongNumber = element(by.binding('longNumber | limitTo:longNumberLimit'));

it('should limit the number array to first three items', function() {
  expect(numLimitInput.getAttribute('value')).toBe('3');
  expect(letterLimitInput.getAttribute('value')).toBe('3');
  expect(longNumberLimitInput.getAttribute('value')).toBe('3');
  expect(limitedNumbers.getText()).toEqual('Output numbers: [1,2,3]');
  expect(limitedLetters.getText()).toEqual('Output letters: abc');
  expect(limitedLongNumber.getText()).toEqual('Output long number: 234');
});

// There is a bug in safari and protractor that doesn't like the minus key
// it('should update the output when -3 is entered', function() {
//   numLimitInput.clear();
//   numLimitInput.sendKeys('-3');
//   letterLimitInput.clear();
//   letterLimitInput.sendKeys('-3');
//   longNumberLimitInput.clear();
//   longNumberLimitInput.sendKeys('-3');
//   expect(limitedNumbers.getText()).toEqual('Output numbers: [7,8,9]');
//   expect(limitedLetters.getText()).toEqual('Output letters: ghi');
//   expect(limitedLongNumber.getText()).toEqual('Output long number: 342');
// });

it('should not exceed the maximum size of input array', function() {
  numLimitInput.clear();
  numLimitInput.sendKeys('100');
  letterLimitInput.clear();
  letterLimitInput.sendKeys('100');
  longNumberLimitInput.clear();
  longNumberLimitInput.sendKeys('100');
  expect(limitedNumbers.getText()).toEqual('Output numbers: [1,2,3,4,5,6,7,8,9]');
  expect(limitedLetters.getText()).toEqual('Output letters: abcdefghi');
  expect(limitedLongNumber.getText()).toEqual('Output long number: 2345432342');
});
```
