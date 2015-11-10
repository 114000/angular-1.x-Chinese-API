# `ng-include`
- Angular@1.4.7
- `ng` 模块中的指令

获取，编译，包含一个外部的 HTML 碎片。

默认情况下，模板 URL 被限定在与应用文件同一域和协议下。这样做是对其调用了 `$sce.getTrustedResourceUrl`
为了载入其他域或协议下的模板，你可以将它们加入你的白名单，或者将它们包装成受信任的值。参考 Angular 的
Strict Contextual Escaping.（SCE）（译：`$sce` 服务）

此外，浏览器的同源策略（Same Origin Policy）和跨域资源分享（Cross-Origin Resource Sharing ：CORS）
策略会进一步影响模板是否能成功加载，例如，`ngInclude` 不能工作在所有浏览器跨域环境的请求中，
和访问某些浏览器的 `file://`。

## 指令信息

这个指令会创建一个新的作用域
这个指令的执行优先级为400级

## 用法

以元素属性的形式使用:（这个指令可以像自定义元素一样使用，但是要注意 IE 浏览器的限制。译：详见指引中的 IE 兼容性）

``` html
<ng-include
  src="string"
  [onload="string"]
  [autoscroll="string"]>
...
</ng-include>
```

以元素属性的形式使用:

``` html
<ANY
  ng-include="string"
  [onload="string"]
  [autoscroll="string"]>
...
</ANY>
```

用作 CSS 类
``` html
<ANY class="ng-include: string; [onload: string;] [autoscroll: string;]"> ... </ANY>
```

## 动画

**enter** - 新的内容被置入浏览器时的动画
**leave** - 存在的内容被移除时的动画

进入和离开的动画会同时发生。


点击这儿了解更多关于动画中的执行步骤（`$animate`）。

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngInclude / src|`string`|解析为 URL 的 angular 表达式，如果源是一个字符串内容，确保它被单引号包裹，如：`src="'myPartialTemplate.html'"`|
|onload（可选）|`string`| 新的部分载入后表达式被解析执行。|
|autoscroll（可选）|`string`| 内容载入后是否调用 `$anchorScroll` 来滚动视图<br><br>- 如果属性未设置，禁用滚动<br>- 如果设置了属性但没有赋值，启用滚动<br>- 其他情况下在表达式解析为真值时启用滚动<br>|

## 事件

### `$includeContentRequested`

每次 `ngInclude` 内容被请求时广播（Emit）这个事件。

类型：冒泡（emit）

目标：`ngInclude` 声明的作用域

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|angularEvent|`Object`| 综合事件对象|
|src|`string`|载入内容的 URL|

### `$includeContentLoaded`

每次 `ngInclude` 内容被重载时广播（Emit）这个事件。

类型：冒泡（emit）

目标：当前 `ngInclude` 的作用域

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|angularEvent|`Object`| 综合事件对象|
|src|`string`|载入内容的 URL|

### `$includeContentError`

当一个模板的 HTTP 请求返回错误响应时广播此事件（status < 200 || status > 299）

类型：冒泡（emit）

目标：`ngInclude` 声明的作用域

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|angularEvent|`Object`|综合事件对象|
|src|`string`| 载入内容的 URL|

## 例子

> index.html

``` html
<div ng-controller="ExampleController">
  <select ng-model="template" ng-options="t.name for t in templates">
   <option value="">(blank)</option>
  </select>
  url of the template: <code>{{template.url}}</code>
  <hr/>
  <div class="slide-animate-container">
    <div class="slide-animate" ng-include="template.url"></div>
  </div>
</div>
```

> script.js

``` javascript
angular.module('includeExample', ['ngAnimate'])
.controller('ExampleController', ['$scope', function($scope) {
  $scope.templates =
    [ { name: 'template1.html', url: 'template1.html'},
      { name: 'template2.html', url: 'template2.html'} ];
  $scope.template = $scope.templates[0];
}]);
```

> template1.html

``` html
template1.html 的内容
```

> template2.html

``` html
template2.html 的内容
```


> animations.css

``` css
.slide-animate-container {
  position:relative;
  background:white;
  border:1px solid black;
  height:40px;
  overflow:hidden;
}

.slide-animate {
  padding:10px;
}

.slide-animate.ng-enter, .slide-animate.ng-leave {
  transition:all cubic-bezier(0.250, 0.460, 0.450, 0.940) 0.5s;

  position:absolute;
  top:0;
  left:0;
  right:0;
  bottom:0;
  display:block;
  padding:10px;
}

.slide-animate.ng-enter {
  top:-50px;
}
.slide-animate.ng-enter.ng-enter-active {
  top:0;
}

.slide-animate.ng-leave {
  top:0;
}
.slide-animate.ng-leave.ng-leave-active {
  top:50px;
}
```

> protractor.js

``` javascript
var templateSelect = element(by.model('template'));
var includeElem = element(by.css('[ng-include]'));

it('should load template1.html', function() {
  expect(includeElem.getText()).toMatch(/Content of template1.html/);
});

it('should load template2.html', function() {
  if (browser.params.browser == 'firefox') {
    // Firefox can't handle using selects
    // See https://github.com/angular/protractor/issues/480
    return;
  }
  templateSelect.click();
  templateSelect.all(by.css('option')).get(2).click();
  expect(includeElem.getText()).toMatch(/Content of template2.html/);
});

it('should change to blank', function() {
  if (browser.params.browser == 'firefox') {
    // Firefox can't handle using selects
    return;
  }
  templateSelect.click();
  templateSelect.all(by.css('option')).get(0).click();
  expect(includeElem.isPresent()).toBe(false);
});
```
