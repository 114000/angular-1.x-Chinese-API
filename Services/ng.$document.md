# Angular_1.4.3 API 服务篇 $document

- `ng`模块中的服务

一个对于浏览器中的 `window.document`对象的[jQuery 或 jqLite](https://code.angularjs.org/1.4.3/docs/api/ng/function/angular.element) 封装。

## 依赖

`$window`

## 例子

> html

``` html
<div ng-controller="ExampleController">

  <p>$document title: <b ng-bind="title"></b></p>
  <p>window.document title: <b ng-bind="windowTitle"></b></p>

</div>
```

> javascript

``` javascript
angular.module('documentExample', [])

.controller('ExampleController', ['$scope', '$document',
  function($scope, $document) {

    $scope.title = $document[0].title;
    $scope.windowTitle = angular.element(window.document)[0].title;

}]);
```
