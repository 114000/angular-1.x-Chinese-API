# Angular_1.4.3 API 服务篇 $anchorScroll
- [`$anchorScrollProvider`](https://code.angularjs.org/1.4.3/docs/api/ng/provider/$anchorScrollProvider)
- [ng](https://code.angularjs.org/1.4.3/docs/api/ng) 模块中的服务

当被调用的时候，页面会滚动到与元素相关联的指定的 `hash` 处，或者滚动到当前 [`$location.hash()`](https://code.angularjs.org/1.4.3/docs/api/ng/service/$location#hash) 处，是依照[HTML5 spec](http://dev.w3.org/html5/spec/Overview.html#the-indicated-part-of-the-document) 的规则制定的。


它当然也会监听 `$location.hash()` 并且无论锚点值何时变化，都会自动地滚动到相应的位置。当不需要它时，可以调用[`$anchorScrollProvider.disableAutoScrolling()`](https://code.angularjs.org/1.4.3/docs/api/ng/provider/$anchorScrollProvider#disableAutoScrolling)让它失效。

另外我们可以使用它的 [`yOffset`](https://code.angularjs.org/1.4.3/docs/api/ng/service/$anchorScroll#yOffset) 属性来指定一个垂直滚动偏移量（既可以是定值也可以是动态值）。

---
## 依赖

`$window` `$location` `$rootScope`

---
## 用法

`$anchorScroll([hash])`;

##### *参数*

| 参数 | 形式 | 具体 |
|:-----|:----:|:-----|
|`hash` *(可选)*| `string`|`hash` 将会指定元素滚动到的位置，如果省略参数，则将使用[`$location.hash()`](https://code.angularjs.org/1.4.3/docs/api/ng/service/$location#hash) 作为默认值。


---
## 属性
`yOffset`

|---|---|
|---|---|
|`number`<br><br>`function()`<br><br>`jqLite`| 如果设置了这个值，将会指定一个垂直的滚动的偏移量。这种场景经常用于在页面顶部有固定定位的元素, 如导航条，头部等（让出头部空间）。<br><br> `yOffset` 可以用多种途径指定: <br><br> - **number** : 一个固定的像素值可以使用（无单位）。<br>- **function** : 每次`$anchorScroll()`执行时这个函数都会被调用，它必须返回一个代表位移的数字（无单位像素值）。<br>**jqLite** : 一个jqLite/jQuery元素可以被指定为位移值。这个位移值会取页面的顶部到该元素底部的距离。<br><br> **注意**: 只有有元素的定位方式是固定定位时才会应该被纳入考虑之中。这个设置 在响应式的导航条/头部需要调整他们的高度亦或 根据视图来定位时很有用处。

> 为了使 `yOffset` 正确地工作，滚动必须是在文档的根节点，而不是子节点。

---

## 例子

> html

``` html
<div id="scrollArea" ng-controller="ScrollController">

  <a ng-click="gotoBottom()">Go to bottom</a>
  <a id="bottom"></a> You're at the bottom!

</div>
```

javascript

``` javascript
angular.module('anchorScrollExample', [])

.controller('ScrollController', ['$scope', '$location', '$anchorScroll',
  function ($scope, $location, $anchorScroll) {

    $scope.gotoBottom = function() {

      // 将location.hash的值设置为
      // 你想要滚动到的元素的id
      $location.hash('bottom');

      // 调用 $anchorScroll()
      $anchorScroll();

    };
  }]);
```
> css

``` css
#scrollArea {

  height: 280px;
  overflow: auto;

}

#bottom {

  display: block;
  margin-top: 2000px;

}
```
---

下面的例子将说明如何使用一个垂直滚动偏移（指定了一个固定值）关于 `$anchorScroll.yOffset` 的详情请看上方介绍

> html

```html
<div class="fixed-header" ng-controller="headerCtrl">

  <a href="" ng-click="gotoAnchor(x)" ng-repeat="x in [1,2,3,4,5]">
    Go to anchor {{x}}
  </a>

</div>
<div id="anchor{{x}}" class="anchor" ng-repeat="x in [1,2,3,4,5]">

  Anchor {{x}} of 5

</div>
```

> javascript

``` javascript
angular.module('anchorScrollOffsetExample', [])
.run(['$anchorScroll', function($anchorScroll) {

  $anchorScroll.yOffset = 50;   // 总是滚动额外的50像素

}])
.controller('headerCtrl', ['$anchorScroll', '$location', '$scope',
  function ($anchorScroll, $location, $scope) {

    $scope.gotoAnchor = function(x) {

      var newHash = 'anchor' + x;
      if ($location.hash() !== newHash) {

        // 将$location.hash设置为`newHash` and
        // $anchorScroll也将自动滚到该处

        $location.hash('anchor' + x);

      } else {

        // 显式地调用 $anchorScroll()方法 ,
        // 因为 $location.hash 并没有改变
        $anchorScroll();

      }
    };
  }
]);
```

> css

``` css
body {

  padding-top: 50px;

}

.anchor {

  border: 2px dashed DarkOrchid;
  padding: 10px 10px 200px 10px;

}

.fixed-header {

  background-color: rgba(0, 0, 0, 0.2);
  height: 50px;
  position: fixed;
  top: 0; left: 0; right: 0;

}

.fixed-header > a {

  display: inline-block;
  margin: 5px 15px;

}
```
