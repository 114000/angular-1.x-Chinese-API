# `<input>`
- Angular@1.4.7
- `ng` 模块中的指令

HTML input element control. When used together with ngModel, it provides data-binding, input state control, and validation. Input control follows HTML5 input types and polyfills the HTML5 validation behavior for older browsers.

Note: Not every feature offered is available for all input types. Specifically, data binding and event handling via ng-model is unsupported for input[file].


## 指令信息

这个指令的执行优先级为0级


## 用法

像个元素（标签）一样使用

``` html
<input
  ng-model="string"
  [name="string"]
  [required="string"]
  [ng-required="boolean"]
  [ng-minlength="number"]
  [ng-maxlength="number"]
  [ng-pattern="string"]
  [ng-change="string"]
  [ng-trim="boolean"]>
...
</input>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngModel|`string`|Assignable angular expression to data-bind to.|
|name（可选）|`string`|Property name of the form under which the control is published.|
|required（可选）|`string`|Sets required validation error key if the value is not entered.|
|ngRequired（可选）|`boolean`|Sets required attribute if set to true|
|ngMinlength（可选）|`number`|Sets minlength validation error key if the value is shorter than minlength.|
|ngMaxlength（可选）|`number`|Sets maxlength validation error key if the value is longer than maxlength. Setting the attribute to a negative or non-numeric value, allows view values of any length.|
|ngPattern（可选）|`string`|Sets pattern validation error key if the ngModel value does not match a RegExp found by evaluating the Angular expression given in the attribute value. If the expression evaluates to a RegExp object, then this is used directly. If the expression evaluates to a string, then it will be converted to a RegExp after wrapping it in ^ and $ characters. For instance, "abc" will be converted to new RegExp('^abc$').
Note: Avoid using the g flag on the RegExp, as it will cause each successive search to start at the index of the last search's match, thus not taking the whole input value into account.|
|ngChange（可选）|`string`|Angular expression to be executed when input changes due to user interaction with the input element.|
|ngTrim（可选）|`boolean`|If set to false Angular will not automatically trim the input. This parameter is ignored for input[type=password] controls, which will never trim the input.(默认值: true)|

## 例子

> index.html

``` html
<script>
angular.module('inputExample', [])
.controller('ExampleController', ['$scope', function($scope) {
  $scope.user = {name: 'guest', last: 'visitor'};
}]);
</script>
<div ng-controller="ExampleController">
  <form name="myForm">
    <label>
       User name:
       <input type="text" name="userName" ng-model="user.name" required>
    </label>
    <div role="alert">
      <span class="error" ng-show="myForm.userName.$error.required">
       Required!</span>
    </div>
    <label>
       Last name:
       <input type="text" name="lastName" ng-model="user.last"
       ng-minlength="3" ng-maxlength="10">
    </label>
    <div role="alert">
      <span class="error" ng-show="myForm.lastName.$error.minlength">
        Too short!</span>
      <span class="error" ng-show="myForm.lastName.$error.maxlength">
        Too long!</span>
    </div>
  </form>
  <hr>
  <tt>user = {{user}}</tt><br/>
  <tt>myForm.userName.$valid = {{myForm.userName.$valid}}</tt><br/>
  <tt>myForm.userName.$error = {{myForm.userName.$error}}</tt><br/>
  <tt>myForm.lastName.$valid = {{myForm.lastName.$valid}}</tt><br/>
  <tt>myForm.lastName.$error = {{myForm.lastName.$error}}</tt><br/>
  <tt>myForm.$valid = {{myForm.$valid}}</tt><br/>
  <tt>myForm.$error.required = {{!!myForm.$error.required}}</tt><br/>
  <tt>myForm.$error.minlength = {{!!myForm.$error.minlength}}</tt><br/>
  <tt>myForm.$error.maxlength = {{!!myForm.$error.maxlength}}</tt><br/>
</div>
```

> protractor.js

``` javascript
var user = element(by.exactBinding('user'));
var userNameValid = element(by.binding('myForm.userName.$valid'));
var lastNameValid = element(by.binding('myForm.lastName.$valid'));
var lastNameError = element(by.binding('myForm.lastName.$error'));
var formValid = element(by.binding('myForm.$valid'));
var userNameInput = element(by.model('user.name'));
var userLastInput = element(by.model('user.last'));

it('should initialize to model', function() {
  expect(user.getText()).toContain('{"name":"guest","last":"visitor"}');
  expect(userNameValid.getText()).toContain('true');
  expect(formValid.getText()).toContain('true');
});

it('should be invalid if empty when required', function() {
  userNameInput.clear();
  userNameInput.sendKeys('');

  expect(user.getText()).toContain('{"last":"visitor"}');
  expect(userNameValid.getText()).toContain('false');
  expect(formValid.getText()).toContain('false');
});

it('should be valid if empty when min length is set', function() {
  userLastInput.clear();
  userLastInput.sendKeys('');

  expect(user.getText()).toContain('{"name":"guest","last":""}');
  expect(lastNameValid.getText()).toContain('true');
  expect(formValid.getText()).toContain('true');
});

it('should be invalid if less than required min length', function() {
  userLastInput.clear();
  userLastInput.sendKeys('xx');

  expect(user.getText()).toContain('{"name":"guest"}');
  expect(lastNameValid.getText()).toContain('false');
  expect(lastNameError.getText()).toContain('minlength');
  expect(formValid.getText()).toContain('false');
});

it('should be invalid if longer than max length', function() {
  userLastInput.clear();
  userLastInput.sendKeys('some ridiculously long name');

  expect(user.getText()).toContain('{"name":"guest"}');
  expect(lastNameValid.getText()).toContain('false');
  expect(lastNameError.getText()).toContain('maxlength');
  expect(formValid.getText()).toContain('false');
});
```
