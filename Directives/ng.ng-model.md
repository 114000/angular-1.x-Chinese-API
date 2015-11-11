# `ng-model`
- Angular@1.4.7
- `ng` 模块中的指令

`ngModel` 指令将一个 `input`，`select`，`textarea`（或者自定义的表单控件）绑定到使用了
`NgModelController`的作用域的一个属性上，这个属性是由这个指令创建并导出的。

`ngModel` 负责：

- 将视图（view）绑定到那些需要它的指令的模型（model）上，如 `input`，`textarea`，`select`.
- 提供验证功能（例如：required, number, email, url）。
- 保存控件的状态（合法的/非法的 valid/invalid, 改变/未改变 dirty/pristine, 触摸过/未触摸
touched/untouched, 验证错误）。
- 在元素上设置包含动画的相关的 CSS 类（ng-valid, ng-invalid, ng-dirty, ng-pristine, ng-touched, ng-untouched）
- 在控件的父表（form）中注册。

**注意**：`ngModel` 将会试图绑定到当前作用域的属性上。这个属性是 `ngModel` 被赋值的表达式解析的结果，
但如果这个属性并不真实存在这个作用域中，那么它会被隐性的创建并被添加到作用域中。

对于使用 `ngModel` 的最佳实践，请看：

For best practices on using ngModel, see:

- [理解作用域（ Scopes）](https://github.com/angular/angular.js/wiki/Understanding-Scopes)

关于如何使用 `ngModel` 基础的例子，请看（译：下面的例子都在指令的 ng.input 中）：

- input
  + text
  + checkbox
  + radio
  + number
  + email
  + url
  + date
  + datetime-local
  + time
  + month
  + week
- select
- textarea

## CSS 类

下面这些 CSS 类的增删是由 `input/select/textarea` 元素所依赖的模型值的合法性（validity）所决定的。

- `ng-valid`: 模型值是合法的
- `ng-invalid`: 模型值是非法的
- `ng-valid-[key]`: 由 `$setValidity` 添加的合法键
- `ng-invalid-[key]`: 由 `$setValidity` 添加的非法键
- `ng-pristine`: 控件还没有发生过交互行为
- `ng-dirty`: 控件发生过交互行为
- `ng-touched`: 控件已经失去焦点
- `ng-untouched`: 控件还没有失去焦点
- `ng-pending`: 任何异步的验证都没有完成（$asyncValidators）

要记得，`ngAnimate` 在删除或添加这些 CSS 类时可以对它们中的每一项进行检测。

## 动画钩子

当依附在模型上的表单元素添加或删除了任意相关的 CSS 类时，模型上的动画都将被触发。这些类有：
`.ng-pristine`，`.ng-dirty`，`.ng-invalid`，`.ng-valid ` 当然也包括模型执行的任何其他的验证。
动画在 `ngModel` 被触发的方式与 `ngClass` 指令中的方式相似，这些动画可以被钩嵌于 CSS 过渡，关键帧动画，
JS 动画。

下面展示了一个简单的 CSS 过渡动画的应用，当表单元素被验证后将其渲染成非法的样式。

``` css
/* 要确保引入了 `ngAnimate` 模块以保证能钩嵌更优良的动画效果。*/

.my-input {
  transition:0.5s linear all;
  background: white;
}
.my-input.ng-invalid {
  background: red;
  color:white;
}
```

## 指令信息

这个指令的执行优先级为1级

## 用法

用作属性：

``` html
<input>
...
</input>
```

## 例子

> index.html

``` html
<script>
 angular.module('inputExample', [])
   .controller('ExampleController', ['$scope', function($scope) {
     $scope.val = '1';
   }]);
</script>
<style>
  .my-input {
    transition:all linear 0.5s;
    background: transparent;
  }
  .my-input.ng-invalid {
    color:white;
    background: red;
  }
</style>
<p id="inputDescription">
 Update input to see transitions when valid/invalid.
 Integer is a valid value.
</p>
<form name="testForm" ng-controller="ExampleController">
  <input ng-model="val" ng-pattern="/^\d+$/" name="anim" class="my-input"
         aria-describedby="inputDescription" />
</form>
```

## 绑定到属性获取器/属性设置器（Binding to a getter/setter）

有时候将 `ngModel` 绑定到 `getter/setter` 函数上是有好处的。当不为 `getter/setter`
传入参数时会返回改模型的表示，当传入唯一参数时会设置模型的内部状态，有事可以用来比较模型的内部表示
与导出到视图的模型有何不同。

``` html
最佳实践: 要保证 `getters` 的速度，因为 Angular 相较于你代码的其他部分可能会更为频繁调用 `getters`，
```

你可以为一个元素添加 `ng-model-options="{ getterSetter: true }"` 来使 `ng-model`
依附到 `getter/setter` 上。你也可以将 `ng-model-options="{ getterSetter: true }"`
添加到一个 `<form>` 标签上，使其内部的所有表单都拥有 `getter/setter`，查看 `ngModelOptions`
了解更多。

下面展示了如何使用 `getter/setter` 形式的 `ngModel`

> index.html

``` html
<div ng-controller="ExampleController">
  <form name="userForm">
    <label>Name:
      <input type="text" name="userName"
             ng-model="user.name"
             ng-model-options="{ getterSetter: true }" />
    </label>
  </form>
  <pre>user.name = <span ng-bind="user.name()"></span></pre>
</div>
```

> app.js

``` javascript
angular.module('getterSetterExample', [])
.controller('ExampleController', ['$scope', function($scope) {
  var _name = 'Brian';
  $scope.user = {
    name: function(newName) {
     // Note that newName can be undefined for two reasons:
     // 1. Because it is called as a getter and thus called with no arguments
     // 2. Because the property should actually be set to undefined. This happens e.g. if the
     //    input is invalid
     return arguments.length ? (_name = newName) : _name;
    }
  };
}]);
```
