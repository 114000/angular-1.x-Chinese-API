# Angular_1.4.3 API 服务篇 $filter

- $filterProvider
- ng 模块中的服务

过滤器用来格式化数据展示给用户。

模板中的一般用法如下：

``` html

{ { expression [| filter_name[:parameter_value] ... ] }}

```

---

## 用法

`$filter(name)`


**参数**

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
|name| `string` |	想要取回的过滤器的函数名 |

**返回**

`fn` - 过滤器函数

---

## 例子

> index.html

``` html

<div ng-controller="MainCtrl">
 <h3>{{ originalText }}</h3>
 <h3>{{ filteredText }}</h3>
</div>

```

> script.js

``` javascript

angular.module('filterExample', [])
.controller('MainCtrl', function($scope, $filter) {
  $scope.originalText = 'hello'
  $scope.filteredText = $filter('uppercase')($scope.originalText)
})


```
