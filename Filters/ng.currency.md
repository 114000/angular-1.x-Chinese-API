# `currency`
- Angular@1.4.7
- `ng` 模块中的过滤器

将数字格式化为货币（currency）（例如：$1,234.56）。 如果没有提供货币符号时，
默认使用当前语言环境（locale）的符号。

## 用法

HTML 模板绑定中

``` html
{{ currency_expression | currency : symbol : fractionSize}}
```

JavaScript 中

``` javascript
$filter('currency')(amount, symbol, fractionSize)
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|amount|`number`| 过滤器的输入 |
|symbol(可选)|`string`| 货币符号或者要展示的识别码 |
|fractionSize(可选)|`number`| 小数位的取舍量。默认为当前语言地区的最大量。|

#### 返回

`string` 格式化后的数值

## 例子

> index.html

``` html
<script>
  angular.module('currencyExample', [])
    .controller('ExampleController', ['$scope', function($scope) {
      $scope.amount = 1234.56;
    }]);
</script>
<div ng-controller="ExampleController">
  <input type="number" ng-model="amount" aria-label="amount"> <br>

  default currency symbol ($):
  <span id="currency-default">{{amount | currency}}</span><br>

  custom currency identifier (USD$):
  <span id="currency-custom">{{amount | currency:"USD$"}}</span>

  no fractions (0):
  <span id="currency-no-fractions">{{amount | currency:"USD$":0}}</span>
</div>
```

> protractor.js

``` javascript
it('should init with 1234.56', function() {
  expect(element(by.id('currency-default')).getText()).toBe('$1,234.56');
  expect(element(by.id('currency-custom')).getText()).toBe('USD$1,234.56');
  expect(element(by.id('currency-no-fractions')).getText()).toBe('USD$1,235');
});
it('should update', function() {
  if (browser.params.browser == 'safari') {
    // Safari 不识别减号键
    // 请看 https://github.com/angular/protractor/issues/481
    return;
  }
  element(by.model('amount')).clear();
  element(by.model('amount')).sendKeys('-1234');
  expect(element(by.id('currency-default')).getText()).toBe('-$1,234.00');
  expect(element(by.id('currency-custom')).getText()).toBe('-USD$1,234.00');
  expect(element(by.id('currency-no-fractions')).getText()).toBe('-USD$1,234');
});
```
