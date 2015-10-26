# `orderBy`
- Angular@1.4.7
- `ng` 模块中的过滤器

使用表达式的谓词为数组排序。对字符串使用字母顺序进行排序，数字则按数字的大小排序。

**注意**：如果你发现数字不是按照你的预期进行排序的，请确定他们被保存的实际形式确实为数字而不是字符串。

## 用法

HTML 模板绑定中

``` html
{{ orderBy_expression | orderBy : expression : reverse}}
```

JavaScript 中

``` javascript
$filter('orderBy')(array, expression, reverse)
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|array|`array`| 需要排序的数组 |
|expression|`string` <br> `function(*)` <br> `Array.<function(*)>=` <br> `Array.<string>=` |用于比较来确定元素顺序的谓词。<br>可以为以下之一：<br>- `function`：Getter 函数.这个函数返回的结果会使用 `<`，`===`，`>` 操作符进行排序。<br><br>- `string`：一个 Angular 表达式。表达式解析用来比较元素。（举个例子，表达式为 `name`，则以 `name` 属性进行排序，表达式为 `name.substr(0, 3)`，则以 `name` 属性的值的前3个字符进行排序）。一个常量表达式解析的结果会被当做属性名而被用于比较（例如："special name"就会使用他们的 `special name` 属性进行排序）。一个表达式可以通过设置 `+` 或 `-`前缀来控制排序（升序或降序）方式（例如：`+name` 或 `-name`）。如果没有提供属性（例如：'+'； 译：仅仅规定了升序）则会用数组元素本身来进行排序。<br><br>- `Array`: 一个函数、字符串谓词组成的数组，第一个谓词用于排序，当两项相等时，则用下一个谓词排序。如果不传入谓词或者为空则默认为 `'+'`|
|reverse(可选)|`boolean`| 翻转数组的顺序|

#### 返回

`string` 经过排序数组的复制品

## 例子

下面的例子展示了一个简单的 `ngRepeat`，数据通过 `age` 来降序排列（谓词设置成了 `-age`）。
翻转（reverse）没有设置，默认不生效（false）

> index.html

``` html
<script>
  angular.module('orderByExample', [])
    .controller('ExampleController', ['$scope', function($scope) {
      $scope.friends =
          [{name:'John', phone:'555-1212', age:10},
           {name:'Mary', phone:'555-9876', age:19},
           {name:'Mike', phone:'555-4321', age:21},
           {name:'Adam', phone:'555-5678', age:35},
           {name:'Julie', phone:'555-8765', age:29}];
    }]);
</script>
<div ng-controller="ExampleController">
  <table class="friend">
    <tr>
      <th>Name</th>
      <th>Phone Number</th>
      <th>Age</th>
    </tr>
    <tr ng-repeat="friend in friends | orderBy:'-age'">
      <td>{{friend.name}}</td>
      <td>{{friend.phone}}</td>
      <td>{{friend.age}}</td>
    </tr>
  </table>
</div>
```

谓词和翻转参数可以通过作用域（scope）属性动态地控制。下面的例子将展示。

> index.html

``` html
<script>
  angular.module('orderByExample', [])
    .controller('ExampleController', ['$scope', function($scope) {
      $scope.friends =
          [{name:'John', phone:'555-1212', age:10},
           {name:'Mary', phone:'555-9876', age:19},
           {name:'Mike', phone:'555-4321', age:21},
           {name:'Adam', phone:'555-5678', age:35},
           {name:'Julie', phone:'555-8765', age:29}];
      $scope.predicate = 'age';
      $scope.reverse = true;
      $scope.order = function(predicate) {
        $scope.reverse = ($scope.predicate === predicate) ? !$scope.reverse : false;
        $scope.predicate = predicate;
      };
    }]);
</script>
<style type="text/css">
  .sortorder:after {
    content: '\25b2';
  }
  .sortorder.reverse:after {
    content: '\25bc';
  }
</style>
<div ng-controller="ExampleController">
  <pre>Sorting predicate = {{predicate}}; reverse = {{reverse}}</pre>
  <hr/>
  [ <a href="" ng-click="predicate=''">unsorted</a> ]
  <table class="friend">
    <tr>
      <th>
        <a href="" ng-click="order('name')">Name</a>
        <span class="sortorder" ng-show="predicate === 'name'" ng-class="{reverse:reverse}"></span>
      </th>
      <th>
        <a href="" ng-click="order('phone')">Phone Number</a>
        <span class="sortorder" ng-show="predicate === 'phone'" ng-class="{reverse:reverse}"></span>
      </th>
      <th>
        <a href="" ng-click="order('age')">Age</a>
        <span class="sortorder" ng-show="predicate === 'age'" ng-class="{reverse:reverse}"></span>
      </th>
    </tr>
    <tr ng-repeat="friend in friends | orderBy:predicate:reverse">
      <td>{{friend.name}}</td>
      <td>{{friend.phone}}</td>
      <td>{{friend.age}}</td>
    </tr>
  </table>
</div>
```

当然也可以手动地调用 `orderBy`过滤器。注入 `$filter` 服务，使用如 `$filter('orderBy')` 这样来检索排序的过滤器，
然后调用 `$filter('orderBy')`返回的结果，并传入参数。

> index.html

``` html
<div ng-controller="ExampleController">
  <table class="friend">
    <tr>
      <th><a href="" ng-click="reverse=false;order('name', false)">Name</a>
        (<a href="" ng-click="order('-name',false)">^</a>)</th>
      <th><a href="" ng-click="reverse=!reverse;order('phone', reverse)">Phone Number</a></th>
      <th><a href="" ng-click="reverse=!reverse;order('age',reverse)">Age</a></th>
    </tr>
    <tr ng-repeat="friend in friends">
      <td>{{friend.name}}</td>
      <td>{{friend.phone}}</td>
      <td>{{friend.age}}</td>
    </tr>
  </table>
</div>
```

> script.js

``` javascript
angular.module('orderByExample', [])
.controller('ExampleController', ['$scope', '$filter', function($scope, $filter) {
  var orderBy = $filter('orderBy');
  $scope.friends = [
    { name: 'John',    phone: '555-1212',    age: 10 },
    { name: 'Mary',    phone: '555-9876',    age: 19 },
    { name: 'Mike',    phone: '555-4321',    age: 21 },
    { name: 'Adam',    phone: '555-5678',    age: 35 },
    { name: 'Julie',   phone: '555-8765',    age: 29 }
  ];
  $scope.order = function(predicate, reverse) {
    $scope.friends = orderBy($scope.friends, predicate, reverse);
  };
  $scope.order('-age',false);
}]);
```
