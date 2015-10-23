# Angular_1.4.3 API 服务篇 $log

- `$logProvider`
- `ng` 模块中的服务

简单的打印日志的服务。默认实现安全的写入信息到浏览器的控制台（如果存在的话）。

这个服务最主要的目的是简化调试和排除故障。

默认的设置是打印调试信息。你可以通过 `ng.$logProvider#debugEnabled` 来设置。

## 依赖
`$window`

## 方法

`log();` - 打印日志消息

`info();` - 打印信息消息

`warn();` - 打印警告消息

`error();` - 打印错误消息

`debug();` - 打印调试消息

## 例子

> html

``` html
<div ng-controller="LogController">

  <p>
    Reload this page with open console,
    enter text and hit the log button...
</p>
  <label>Message:
  <input type="text" ng-model="message" /></label>

  <button ng-click="$log.log(message)">log</button>
  <button ng-click="$log.warn(message)">warn</button>
  <button ng-click="$log.info(message)">info</button>
  <button ng-click="$log.error(message)">error</button>
  <button ng-click="$log.debug(message)">debug</button>

</div>
```

> javascript

``` javascript
angular.module('logExample', [])
.controller('LogController', ['$scope', '$log', function($scope, $log) {

  $scope.$log = $log;
  $scope.message = 'Hello World!';

}]);
```
