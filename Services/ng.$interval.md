# Angular_1.4.3 API 服务篇 $interval

- `ng`模块中的服务

Angular对 `window.setInterval` 的封装。`fn` 函数将在每次延时的时候执行。一个注册的间隔函数的返回值是一个 `promise`。
这个 `promise` 将会在interval每次调用的时候得到广播，并且在计数迭代之后被解析，或者在未指定次数的情况下既执行无数次时解析。
这个通知的值将是已运行迭代的次数。如果想终端一个 interval，则调用 `$interval.cancel(promise)`。

测试中你可以使用 `$interval.flush(millis)` 设置其中的 `millis` 为毫秒时间来到达指定的时间点，
并且会触发在此过程中任何的函数。

**注意**: 用此服务创建的interval必须要在完成之后被明文销毁。特别地，在控制器的作用域和指令的元素被销毁时，
interval 也不会被自动销毁。你需要将此纳入考虑之中，确保在适合的时间退出 interval。详述之后的例子将会
介绍何时与怎样去做这些处理。

---
## 用法

`$interval(fn, delay, [count], [invokeApply], [Pass]);`

##### *参数*

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|fn|`function()`|一个将被反复调用的函数|
|delay|`number`|毫秒记的间隔时间|
|count *(可选)*|`number`|循环的次数.如果不传入或者为0，那么$interval将会无限循环. *(默认值: 0)*|
|invokeApply *(可选)*|`boolean`| 如果设置为 `true` 则跳过脏值检测，否则将在 `fn` 中执行 `$apply` *(默认值: true)*|
|Pass *(可选)*|`*`|函数执行的附加参数|


##### *返回*

`promise` - 一个每次迭代都会被通知的 `promise`。

---
## 方法

`cancel([promise]);` - 取消一个与 `promise` 相关的任务。


##### *参数*

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|promise *(可选)*|`promise`| 通过$interval 函数返回。|

##### *返回*

`boolean` - 如果任务被成功取消则返回 `true` 。

---

## 例子

> html

``` html
<div>
  <div ng-controller="ExampleController">

    <label>Date format: <input ng-model="format"></label> <hr/>
    Current time is: <span my-current-time="format"></span>
    <hr/>

    Blood 1 : <font color='red'>{{blood_1}}</font>
    Blood 2 : <font color='red'>{{blood_2}}</font>

    <button type="button" data-ng-click="fight()">Fight</button>
    <button type="button" data-ng-click="stopFight()">StopFight</button>
    <button type="button" data-ng-click="resetFight()">resetFight</button>
  </div>
</div>
```

> javascsript

``` javascript
angular.module('intervalExample', [])
.controller('ExampleController', ['$scope', '$interval',
  function($scope, $interval) {

    $scope.format = 'M/d/yy h:mm:ss a';
    $scope.blood_1 = 100;
    $scope.blood_2 = 120;

    var stop;
    $scope.fight = function() {
      // 如果$scope.fight进行中，则不会开启新的 $scope.fight
      if ( angular.isDefined(stop) ) return;

      stop = $interval(function() {
        if ($scope.blood_1 > 0 && $scope.blood_2 > 0) {
          $scope.blood_1 = $scope.blood_1 - 3;
          $scope.blood_2 = $scope.blood_2 - 4;
        } else {
          $scope.stopFight();
        }
      }, 100);
    };
    $scope.stopFight = function() {
      if (angular.isDefined(stop)) {
        $interval.cancel(stop);
        stop = undefined;
      }
    };

    $scope.resetFight = function() {
      $scope.blood_1 = 100;
      $scope.blood_2 = 120;
    };

    $scope.$on('$destroy', function() {
      // 保证interval已经被销毁
      $scope.stopFight();
    });
  }])
// 用工厂方法注册一个 'myCurrentTime' 指令。
// 我们注入 $interval 服务和源自 DI 的 dateFilter 服务。
.directive('myCurrentTime', ['$interval', 'dateFilter',
  function($interval, dateFilter) {

    // 返回指令链接的函数. (不需要编译函数)
    return function(scope, element, attrs) {
      var format,  // 数据格式
          stopTime; // 以便我们可以取消时间更新

      // 用于更新 UI
      function updateTime() {
        element.text(dateFilter(new Date(), format));
      }

      // 监视表达式，并在改变时更新UI。
      scope.$watch(attrs.myCurrentTime, function(value) {
        format = value;
        updateTime();
      });

      stopTime = $interval(updateTime, 1000);

      // 监听 DOM 销毁(去除)事件，并取消下一次的UI更新
      // 是为了在 DOM 被删除之后防止再次更新次数。
      element.on('$destroy', function() {
        $interval.cancel(stopTime);
      });
    }
  }]);
```
