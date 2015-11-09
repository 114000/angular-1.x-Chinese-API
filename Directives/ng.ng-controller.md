# `ng-controller`
- Angular@1.4.7
- `ng` 模块中的指令

`ngController` 会在视图上添加一个控制器类。这是 angular 如何支持
MVC（Model-View-Controller）设计模式原则的关键因素。

angular 中的 MVC 组件：


- **Model** — 模型是作用域（scope）的属性；作用域依附在 DOM 元素上，作用域的属性则可以通过绑定被访问到。
- **View** — 被渲染到视图（View）中的模板（拥有数据绑定的 HTML）
- **Controller** — `ngController` 指令指定了一个控制器（Controller）类；
这个类包含了应用（app）使用函数和值来修饰作用域背后的业务逻辑


**注意**：通过将使用 `$route` 服务，你可以使用将控制器声明到路由定义中的这种方法将控制器关联到 DOM 上。

一个常见的错误：使用 `ng-controller` 再一次将控制器声明在模板上，这会导致控制器被安放和执行两次。

## 指令信息

这个指令创建了新的作用域
这个指令的执行优先级为500级

## 用法

用作属性：

``` html
<ANY ng-controller="expression">
...
</ANY>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngController|`expression`|使用当前 `$controllerProvider` 注册的构造函数的名字，或者是当前作用域上的一个表达式解析出的构造函数的名字。<br>通过指定 `ng-controller="as propertyName"` 将控制器实例发布到一个作用域（scope）属性中。<br>如果当前的 `$controllerProvider` 配置为使用全局（通过使用 `$controllerProvider.allowGlobals()`）,这也会成为一个全局性的可访问的构造函数的名字（不推荐这么做）。|


## 例子

这是一个简单的编辑用户联系信息的表。添加，删除，清空和展示方法被生命在控制器上（见源标签）。
这些方法可以轻易地从 angular 标记中调用。任何对数据的操作都会自动映射到视图中而不需要手动去更新。

下面介绍了两种不同风格的声明方式：

一种是将方法和属性直接绑定到控制器上，像这样：`ng-controller="SettingsController1 as settings"`

一种是将 `$scope` 注入到控制器中：`ng-controller="SettingsController2"`

第二种方式在 angular 社区中更为流行，而且在文档中也更经常使用这种方式作为样板。但是，
避免使用作用域而直接将属性绑定到控制器上还是有很多优势的。

- 当多个控制器作用在一个元素上时，使用 `controller as` 让被访问模板上的控制器容易识别出来。
- 如果你将你的控制器写成类的方式，则你会更方便的访问那些作用域中和控制器代码中的属性和方法
- 由于绑定中总是有一个 `.`，所以你没有必要担心原型继承会覆盖原始代码。

这个例子展示了 `controller as` 的句法。

> index.html

``` html
<div id="ctrl-as-exmpl" ng-controller="SettingsController1 as settings">
  <label>Name: <input type="text" ng-model="settings.name"/></label>
  <button ng-click="settings.greet()">greet</button><br/>
  Contact:
  <ul>
    <li ng-repeat="contact in settings.contacts">
      <select ng-model="contact.type" aria-label="Contact method" id="select_{{$index}}">
         <option>phone</option>
         <option>email</option>
      </select>
      <input type="text" ng-model="contact.value" aria-labelledby="select_{{$index}}" />
      <button ng-click="settings.clearContact(contact)">clear</button>
      <button ng-click="settings.removeContact(contact)" aria-label="Remove">X</button>
    </li>
    <li><button ng-click="settings.addContact()">add</button></li>
 </ul>
</div>
```

> app.js

``` javascript
angular.module('controllerAsExample', [])
  .controller('SettingsController1', SettingsController1);

function SettingsController1() {
  this.name = "John Smith";
  this.contacts = [
    {type: 'phone', value: '408 555 1212'},
    {type: 'email', value: 'john.smith@example.org'} ];
}

SettingsController1.prototype.greet = function() {
  alert(this.name);
};

SettingsController1.prototype.addContact = function() {
  this.contacts.push({type: 'email', value: 'yourname@example.org'});
};

SettingsController1.prototype.removeContact = function(contactToRemove) {
 var index = this.contacts.indexOf(contactToRemove);
  this.contacts.splice(index, 1);
};

SettingsController1.prototype.clearContact = function(contact) {
  contact.type = 'phone';
  contact.value = '';
};
```

> protractor.js

``` javascript
it('should check controller as', function() {
  var container = element(by.id('ctrl-as-exmpl'));
    expect(container.element(by.model('settings.name'))
      .getAttribute('value')).toBe('John Smith');

  var firstRepeat =
      container.element(by.repeater('contact in settings.contacts').row(0));
  var secondRepeat =
      container.element(by.repeater('contact in settings.contacts').row(1));

  expect(firstRepeat.element(by.model('contact.value')).getAttribute('value'))
      .toBe('408 555 1212');

  expect(secondRepeat.element(by.model('contact.value')).getAttribute('value'))
      .toBe('john.smith@example.org');

  firstRepeat.element(by.buttonText('clear')).click();

  expect(firstRepeat.element(by.model('contact.value')).getAttribute('value'))
      .toBe('');

  container.element(by.buttonText('add')).click();

  expect(container.element(by.repeater('contact in settings.contacts').row(2))
      .element(by.model('contact.value'))
      .getAttribute('value'))
      .toBe('yourname@example.org');
});
```

这个例子展示的是控制器的 “附属到 `$scope` 上” 形式。

> index.html

``` html
<div id="ctrl-exmpl" ng-controller="SettingsController2">
  <label>Name: <input type="text" ng-model="name"/></label>
  <button ng-click="greet()">greet</button><br/>
  Contact:
  <ul>
    <li ng-repeat="contact in contacts">
      <select ng-model="contact.type" id="select_{{$index}}">
         <option>phone</option>
         <option>email</option>
      </select>
      <input type="text" ng-model="contact.value" aria-labelledby="select_{{$index}}" />
      <button ng-click="clearContact(contact)">clear</button>
      <button ng-click="removeContact(contact)">X</button>
    </li>
    <li>[ <button ng-click="addContact()">add</button> ]</li>
 </ul>
</div>
```

> app.js

``` javascript
angular.module('controllerExample', [])
  .controller('SettingsController2', ['$scope', SettingsController2]);

function SettingsController2($scope) {
  $scope.name = "John Smith";
  $scope.contacts = [
    {type:'phone', value:'408 555 1212'},
    {type:'email', value:'john.smith@example.org'} ];

  $scope.greet = function() {
    alert($scope.name);
  };

  $scope.addContact = function() {
    $scope.contacts.push({type:'email',¡ value:'yourname@example.org'});
  };

  $scope.removeContact = function(contactToRemove) {
    var index = $scope.contacts.indexOf(contactToRemove);
    $scope.contacts.splice(index, 1);
  };

  $scope.clearContact = function(contact) {
    contact.type = 'phone';
    contact.value = '';
  };
}
```

> protractor.js

``` javascript
it('should check controller', function() {
  var container = element(by.id('ctrl-exmpl'));

  expect(container.element(by.model('name'))
      .getAttribute('value')).toBe('John Smith');

  var firstRepeat =
      container.element(by.repeater('contact in contacts').row(0));
  var secondRepeat =
      container.element(by.repeater('contact in contacts').row(1));

  expect(firstRepeat.element(by.model('contact.value')).getAttribute('value'))
      .toBe('408 555 1212');
  expect(secondRepeat.element(by.model('contact.value')).getAttribute('value'))
      .toBe('john.smith@example.org');

  firstRepeat.element(by.buttonText('clear')).click();

  expect(firstRepeat.element(by.model('contact.value')).getAttribute('value'))
      .toBe('');

  container.element(by.buttonText('add')).click();

  expect(container.element(by.repeater('contact in contacts').row(2))
      .element(by.model('contact.value'))
      .getAttribute('value'))
      .toBe('yourname@example.org');
});
```
