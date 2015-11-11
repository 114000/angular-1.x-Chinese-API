# `ng-transclude`
- Angular@1.4.7
- `ng` 模块中的指令

这个指令会在父级指令的内部标记一个插入点，当父级指令重新进行模板渲染时，会将标签内原有的 DOM
安放到插入点的位置。

元素内的内容会在被插入到特定的位置前删除掉。

## 指令信息

这个指令的执行优先级为0级

## 用法

用作元素：（这个指令可以当做自定义标签使用，但是要注意IE的兼容性）
``` html
<ng-transclude>
...
</ng-transclude>
```

用作属性：

``` html
<ANY>
...
</ANY>
```

用作 CSS 类

``` html
<ANY class=""> ... </ANY>
```



## 例子

> index.html

```html
<script>
  angular.module('transcludeExample', [])
   .directive('pane', function(){
      return {
        restrict: 'E',
        transclude: true,
        scope: { title:'@' },
        template: '<div style="border: 1px solid black;">' +
                    '<div style="background-color: gray">{{title}}</div>' +
                    '<ng-transclude></ng-transclude>' +
                  '</div>'
      };
  })
  .controller('ExampleController', ['$scope', function($scope) {
    $scope.title = 'Lorem Ipsum';
    $scope.text = 'Neque porro quisquam est qui dolorem ipsum quia dolor...';
  }]);
</script>
<div ng-controller="ExampleController">
  <input ng-model="title" aria-label="title"> <br/>
  <textarea ng-model="text" aria-label="text"></textarea> <br/>
  <pane title="{{title}}">{{text}}</pane>
</div>
```

> protractor.js

``` javascript
it('should have transcluded', function() {
  var titleElement = element(by.model('title'));
  titleElement.clear();
  titleElement.sendKeys('TITLE');
  var textElement = element(by.model('text'));
  textElement.clear();
  textElement.sendKeys('TEXT');
  expect(element(by.binding('title')).getText()).toEqual('TITLE');
  expect(element(by.binding('text')).getText()).toEqual('TEXT');
});
```
