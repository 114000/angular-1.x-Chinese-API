# Angular_1.4.3 API 服务篇 $window

- `ng`模块中的服务

这是一个浏览器端 `window` 对象的引用. 由于Javascript 中的`window`是一个全局变量，它会给可测试性带来问题。在Angular中我们总会通过 `$window` 服务来引用它，因此在测试总它可以被改写、删除或模拟。

在下面的例子中，表达式可以像定义一个`ngClick`指令一样，在当前作用域被严格的评估。因此, 在这种依赖于一个全局变量表达式中，不会因为不经意的编码而带来风险。



> javascript and html

``` javascript
angular.module('windowExample', [])

.controller('ExampleController', ['$scope', '$window', function($scope, $window) {
  $scope.greeting = 'Hello, World!';
  $scope.doGreeting = function(greeting) {
    $window.alert(greeting);
  };
}]);
```

``` html
<div ng-controller="ExampleController">
  <input type="text" ng-model="greeting" aria-label="greeting" />
  <button ng-click="doGreeting(greeting)">ALERT</button>
</div>
```

> protractor

``` javascript
it('should display the greeting in the input box', function() {

 element(by.model('greeting')).sendKeys('Hello, E2E Tests');
 // If we click the button it will block the test runner
 // element(':button').click();
});
```
