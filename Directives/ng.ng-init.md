# `ng-init`
- Angular@1.4.7
- `ng` 模块中的指令

`ngInit` 指令是你可以在当前作用域解析表达式。

这个指令可以随意将逻辑中非必须的东西添加进你的模板。只有少数情况下适合使用 `ngInit`，例如为
`ngRepeat` 的特定属性包装别名，你可以在下面的例子中看到；通过服务器端脚本注入数据。
除了这些少数案例，在作用域使用控制器初始化值要比 `ngInit` 好得多。

**注意**：如果在 `ngInit` 中有一个使用了过滤器的定义，请确保你使用了括号包裹他们来保证正确的优先级：

``` html
<div ng-init="test1 = ($index | toString)"></div>
```

## 指令信息

这个指令的执行优先级为450级

## 用法

用作属性：

``` html
<ANY
  ng-init="expression">
...
</ANY>
```

用作 CSS 类
``` html
<ANY class="ng-init: expression;"> ... </ANY>
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngInit|`expression`| 需要被转化的表达式|


## 例子

> index.html

``` html
<script>
  angular.module('initExample', [])
    .controller('ExampleController', ['$scope', function($scope) {
      $scope.list = [['a', 'b'], ['c', 'd']];
    }]);
</script>
<div ng-controller="ExampleController">
  <div ng-repeat="innerList in list" ng-init="outerIndex = $index">
    <div ng-repeat="value in innerList" ng-init="innerIndex = $index">
       <span class="example-init">list[ {{outerIndex}} ][ {{innerIndex}} ] = {{value}};</span>
    </div>
  </div>
</div>
```

> protractor.js

``` javascript
it('should alias index positions', function() {
  var elements = element.all(by.css('.example-init'));
  expect(elements.get(0).getText()).toBe('list[ 0 ][ 0 ] = a;');
  expect(elements.get(1).getText()).toBe('list[ 0 ][ 1 ] = b;');
  expect(elements.get(2).getText()).toBe('list[ 1 ][ 0 ] = c;');
  expect(elements.get(3).getText()).toBe('list[ 1 ][ 1 ] = d;');
});
```
