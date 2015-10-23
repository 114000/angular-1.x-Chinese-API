# `<form>`
- Angular@1.4.7
- `ng` 模块中的指令

实例化 `FormController`(表单控制器在类型Type目录中)的指令

如果表单的 `name` 属性被指定，那么表单的 `controller`（控制器）会以 `name`
的值命名，并被发布到当前的作用域中。


## 别名: `ngForm`

在 Angular 中，表 (`form`) 是可以嵌套的。当所有的字表都有效时（`valid`），外部包含着他们的表才有效。
但是，浏览器实际上是不支持 `<form>` 元素嵌套的。因此 Angular 提供了 `ngForm` 指令来支持嵌套，
且他们的行为与 `<form>` 相同。这样你就可以嵌套着使用表了，当在表中使用了 Angular验证指令时，
这种方式会体现出很大的好处。它可以被 `ngRepeat` 指令动态地生成出来。由于你不能使用插值来动态生成表单
（`<input>`）元素的 `name` 属性。所以你不得不使用一个 `ngForm` 指令来包裹每组被循环（repeat）
出来的表单，然后再使用一个表元素来包裹这些。


## CSS 类

- `.ng-valid` 如果表是**有效的**，这个CSS类名会被添加。
- `.ng-invalid` 如果表是**无效的**，这个CSS类名会被添加。
- `.ng-pending` 如果表是**待定的**，这个CSS类名会被添加。
- `.ng-pristine` 如果表**没有修改**，这个CSS类名会被添加。
- `.ng-dirty` 如果表**修改了**，这个CSS类名会被添加。
- `.ng-submitted` 如果表**提交过了**，这个CSS类名会被添加。

`ngAnimate` 模块会检测到这些类中每一个的添加与移除


## 提交表数据并防止默认的动作 <small>([action])</small>。

由于在 Angular 设备端应用中表的任务（role）不用于传统的往返型（roundtrip）应用，理想的情况是，
不需要浏览器重载整个页面来使向服务器端提交表单，而是通过应用特定的方式触发相应的 javascript
的逻辑来处理表单的提交。

因此，Angular 阻止了默认动作(action, 表提交到服务端的动作)，除非你为 `<form>` 元素指定了一个
`action` 属性。

你可以使用以下两种方法中的任意一种来调用 javascript 方法来完成表单的提交(译：这个方法自己写)

- 在表元素上 添加 `ngSubmit` 指令
- 在第一个按钮或者 `type` 属性为 `submit` 的表单元素的上面添加 `ngClick` 指令

为了避免重复的执行处理，请确保仅仅使用 `ngSubmit` 和 `ngClick` 的其中一个。
这样做的原因是下面给出的在 HTML 规范中给出的表提交的规则：


- 如果一个表中仅仅只有一个输入框，在这个输入框上的的输入行为会触发表单的提交（`ngSubmit`）
- 如果一个表中有两个输入框并且没有按钮和提交按钮（`input[type=submit]`），则输入行为不会触发表单的提交。
- 如果一个表中有一个以上的输入框，并且有一个以上的按钮或提交按钮，
则在任意一个输入框上的输入行为都将会触发第一个按钮或者提交按钮上的单击处理程序和封闭表单上的提交处理程序。


当一个闭合的表被提交了，任何待定的 `ngModelOptions` 的改变都会立即生效。需要注意 `ngClick`
事件将会在模型（model）更新之前就会触发。所以使用 `ngSubmit` 来允许更新模型。


## 动画钩子

`ngForm` 中的动画，每当任何一个相关联的 CSS 类被添加或移除时都会触发。这些相关的类包括：
`.ng-pristine`, `.ng-dirty`, `.ng-invalid` 和 `.ng-valid` ，还有其他一些在表中使用的验证
（译：更详细的在**类型-FormController** 中）。`ngForm` 中的动画同 `ngClass` 的工作原理一致，
动画行为会被钩取使用 CSS 的 `transtion` 动画、关键帧（@keyframe）动画同样也可以使用 JS 的动画。

下面的例子展示了一个利用 CSS `transtion` 动画将表中的一个被验证过为无效的元素的特定样式渲染出来的栗子🌰。
⁄(⁄ ⁄•⁄ω⁄•⁄ ⁄)⁄


``` css
/* 确保引入了 ngAnimate 模块可以挂接到下面的动画上 */
/* 译：模块的引入,请看_公共方法_中的_ng.angular.module_ */
.my-form {
  transition:0.5s linear all;
  background: white;
}
.my-form.ng-invalid {
  background: red;
  color:white;
}
```

## 指令信息

这个指令的执行优先级为0级

## 用法

像个元素（标签）一样使用

> html

``` html
<form [name="string"]>
...
</form>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|`name` (可选)|`string`| 表的名字，如果被指定了，这个表的控制器就会被发布到关联的作用域（`scope`）中。|


### 例子

> index.html

``` html
<script>
angular.module('formExample', [])
.controller('FormController', ['$scope', function($scope) {
  $scope.userType = 'guest';
}]);
</script>
<style>
 .my-form {
   transition:all linear 0.5s;
   background: transparent;
 }
 .my-form.ng-invalid {
   background: red;
 }
</style>
<form name="myForm" ng-controller="FormController" class="my-form">
  userType: <input name="input" ng-model="userType" required>
  <span class="error" ng-show="myForm.input.$error.required">Required!</span><br>
  <code>userType = {{userType}}</code><br>
  <code>myForm.input.$valid = {{myForm.input.$valid}}</code><br>
  <code>myForm.input.$error = {{myForm.input.$error}}</code><br>
  <code>myForm.$valid = {{myForm.$valid}}</code><br>
  <code>myForm.$error.required = {{!!myForm.$error.required}}</code><br>
 </form>
```

> protractor.js

```javascript
it('should initialize to model', function() {
  var userType = element(by.binding('userType'));
  var valid = element(by.binding('myForm.input.$valid'));

  expect(userType.getText()).toContain('guest');
  expect(valid.getText()).toContain('true');
});

it('should be invalid if empty', function() {
  var userType = element(by.binding('userType'));
  var valid = element(by.binding('myForm.input.$valid'));
  var userInput = element(by.model('userType'));

  userInput.clear();
  userInput.sendKeys('');

  expect(userType.getText()).toEqual('userType =');
  expect(valid.getText()).toContain('false');
});
```
