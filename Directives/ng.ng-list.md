# `ng-list`
- Angular@1.4.7
- `ng` 模块中的指令

分割字符串与数组字符串之间的文本转换。默认的分隔符是后面跟随一个空格的逗号 - 相当于 `ng-list=", "`。
你可以为 `ngList` 属性指定一个自定义的值来作为分隔符 - 例如，`ng-list=" | "`。

这个指令的行为会受到 `ngTrim` 属性的影响。

如果 `ngTrim` 设置为 `false`，分隔符旁的空白会被每个列表项遵守。这意味着指令的用户需要负责处理空白，
但仍允许你使用空白作为分隔符。如 tab 或换行字符。

反之，当切分时会忽略掉分隔符旁的空白（就算它是组合列表项时别一起加入的也不例外），
并且在列表项被添加进 `model` 之前会剥离其周围的空白。


## 带验证的例子

> app.js

``` javascript
angular.module('listExample', [])
.controller('ExampleController', ['$scope', function($scope) {
  $scope.names = ['morpheus', 'neo', 'trinity'];
}]);
```

> index.html

``` html
<form name="myForm" ng-controller="ExampleController">
 <label>List: <input name="namesInput" ng-model="names" ng-list required></label>
 <span role="alert">
   <span class="error" ng-show="myForm.namesInput.$error.required">
   Required!</span>
 </span>
 <br>
 <tt>names = {{names}}</tt><br/>
 <tt>myForm.namesInput.$valid = {{myForm.namesInput.$valid}}</tt><br/>
 <tt>myForm.namesInput.$error = {{myForm.namesInput.$error}}</tt><br/>
 <tt>myForm.$valid = {{myForm.$valid}}</tt><br/>
 <tt>myForm.$error.required = {{!!myForm.$error.required}}</tt><br/>
</form>
```

> protractor.js

``` javascript
var listInput = element(by.model('names'));
var names = element(by.exactBinding('names'));
var valid = element(by.binding('myForm.namesInput.$valid'));
var error = element(by.css('span.error'));

it('should initialize to model', function() {
  expect(names.getText()).toContain('["morpheus","neo","trinity"]');
  expect(valid.getText()).toContain('true');
  expect(error.getCssValue('display')).toBe('none');
});

it('should be invalid if empty', function() {
  listInput.clear();
  listInput.sendKeys('');

  expect(names.getText()).toContain('');
  expect(valid.getText()).toContain('false');
  expect(error.getCssValue('display')).not.toBe('none');
});
```

## Example - splitting on newline

> index.html

``` html
<textarea ng-model="list" ng-list="&#10;" ng-trim="false"></textarea>
<pre>{{ list | json }}</pre>
```

> protractor.js

``` javascript
it("should split the text by newlines", function() {
  var listInput = element(by.model('list'));
  var output = element(by.binding('list | json'));
  listInput.sendKeys('abc\ndef\nghi');
  expect(output.getText()).toContain('[\n  "abc",\n  "def",\n  "ghi"\n]');
});
```

## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<input
  [ng-list="string"]>
...
</input>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngList（可选）|`string`| 被用于分割值的分割符。|
