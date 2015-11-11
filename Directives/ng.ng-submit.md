# `ng-submit`
- Angular@1.4.7
- `ng` 模块中的指令

可以将 angular 表达式绑定到 `onsubmit` 事件上。

此外,如果表没有包含 `action` `data-action` 或者 `x-action` 属性时，这个指令会阻止默认行为（表<form> 会向服务器发送请求并重载当前页面）

``` html
注意：小心同时使用 `ngClick` 和 `ngSubmit`，这可能引起“重复提交”。
查看 `form` 指令文档关于 `ngSubmit` 何时会被触发部分的解释。
```


## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<form
  ng-submit="expression">
...
</form>
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngSubmit|`expression`| 需要解析的表达式（事件对象可以通过 $event 获得）|


## 例子

> index.html

``` html
<script>
  angular.module('submitExample', [])
    .controller('ExampleController', ['$scope', function($scope) {
      $scope.list = [];
      $scope.text = 'hello';
      $scope.submit = function() {
        if ($scope.text) {
          $scope.list.push(this.text);
          $scope.text = '';
        }
      };
    }]);
</script>
<form ng-submit="submit()" ng-controller="ExampleController">
  Enter text and hit enter:
  <input type="text" ng-model="text" name="text" />
  <input type="submit" id="submit" value="Submit" />
  <pre>list={{list}}</pre>
</form>
```

> protractor.js

``` javascript
it('should check ng-submit', function() {
  expect(element(by.binding('list')).getText()).toBe('list=[]');
  element(by.css('#submit')).click();
  expect(element(by.binding('list')).getText()).toContain('hello');
  expect(element(by.model('text')).getAttribute('value')).toBe('');
});
it('should ignore empty strings', function() {
  expect(element(by.binding('list')).getText()).toBe('list=[]');
  element(by.css('#submit')).click();
  element(by.css('#submit')).click();
  expect(element(by.binding('list')).getText()).toContain('hello');
 });
```
