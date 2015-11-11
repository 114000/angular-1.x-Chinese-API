# `ng-value`
- Angular@1.4.7
- `ng` 模块中的指令

将表达式解析值绑定到 `<option>` 或者 `input[radio]` 的 `value` 属性上，以便在元素被选取时，
这个元素上的 `ngModel` 可以设置绑定值。

当使用 `ngRepeat` 动态生成单选按钮时，`ngValue` 的作用会体现出来，下面有例子。

同样地，`ngValue` 也可用于生成 `select` 的 `option` 元素。然而这种情况下，`value`
只支持字符串，所以 `ngModel` 的结果要保证是字符串，`ngOptions` 指令则支付非字符串形式的 `value`。

## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<input
  [ng-value="string"]>
...
</input>
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngValue（可选）|`string`| angular 表达式, 它的值会被绑定到表单元素的 `value` 属性上。|


## 例子

> index.html

``` html
<script>
   angular.module('valueExample', [])
     .controller('ExampleController', ['$scope', function($scope) {
       $scope.names = ['pizza', 'unicorns', 'robots'];
       $scope.my = { favorite: 'unicorns' };
     }]);
</script>
 <form ng-controller="ExampleController">
   <h2>Which is your favorite?</h2>
     <label ng-repeat="name in names" for="{{name}}">
       {{name}}
       <input type="radio"
              ng-model="my.favorite"
              ng-value="name"
              id="{{name}}"
              name="favorite">
     </label>
   <div>You chose {{my.favorite}}</div>
 </form>
```

> protractor.js

``` javascript
var favorite = element(by.binding('my.favorite'));

it('should initialize to model', function() {
  expect(favorite.getText()).toContain('unicorns');
});
it('should bind the values to the inputs', function() {
  element.all(by.model('my.favorite')).get(0).click();
  expect(favorite.getText()).toContain('pizza');
});
```
