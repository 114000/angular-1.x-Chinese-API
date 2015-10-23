---
layout: post
title: Angular_1.4.3 API 服务篇 $sce
desc: '`$sce` 是一个为 AngularJS 提供严格上下文模式 (Strict Contextual Escaping) 的服务'
categories: jekyll update
tags:
- API
- AngularJS
---

# Angular_1.4.3 API 服务篇 $sce

> `$sce` 是一个为 AngularJS 提供严格上下文模式 (Strict Contextual Escaping) 的服务


## 严格上下文模式

在Angular中，严格上下文模式 (SCE)用来将绑定的内容输出为该内容的安全值的形式。一个用法就是：
我们将 `ng-bind-html` 控制的一个随意的 html 绑定内容转化为特定形式或者SCE形式的内容。

Angular 1.2.x以后，将 `$sce` 放进了启动文件中

*注意* : 启动时，IE11以下浏览器的怪异模式是不支持Angular `SCE` 的。它允许使用 ()表达式
执行任意语法。如果你想了解更多相关。你可以将你处于标准模式的文档中的头部的 `<!doctype html>`
移除。

SCE 会给编写代码带来两点好处: a.使用默认方式确保安全, b.审查诸如XSS，点击劫持等安全漏洞做也会变得容易


以下是一个特定形式的绑定用例:

> html

``` html

<input ng-model="userHtml" aria-label="User input">
<div ng-bind-html="userHtml"></div>

```

请注意，`ng-bind-html` 是通过 `user` 绑定到 `userHtml` 控制上的。由于 SCE 失效，
这个应用将会允许用户将任意的 HTML 渲染到 DIV 上。一个更为实际的情景是，需要渲染用户评论，
博客文章的时候。（这里HTML是一个上下文环境，它渲染的由用户控制的输入会造成安全漏洞）

针对这个HTML案例，你可能会在客户端或者服务器端使用一个库，在HTML绑定到值上并渲染到文档之前矫正
它的不安全问题。  

然而你如何确保你使用的库会在你使用双向绑定时生效（或者你的服务器返回的貌似安全的的渲染）。你又将
如何保证你不会不小心删除了矫正值的界线，或者重命名了一些属性/域，但是却忘记更新了矫正值绑定。

在默认情况下保证安全，你想要确保任何绑定都是不允许的，除非你能确定一些明确的表述它是对于在对应的
上下文中使用一个值去绑定是安全的。然后你可以审核你的代码 （一个简单的 `grep` 命令就可以）仅仅是
为了保证你可以更简单的确定那些值是安全的-因为它们将从服务器端发送并被接收，通过你的库来校正等等。
你可以组织你的代码库来帮助你完成这些工作-也许仅允许具体路径下的文件来完成这个工作。 确保由代码暴露
的内部API不会将任意的值认定为安全的，这就成了一个更易于管理任务。

在 Angular `SCE` 服务的案例中，使用 `$sce.trustAs` (或者像 `$sce.trustAsHtml` 等的简洁方法)
来获得符合 SCE 或者指定的上下文环境的值。

---

## 它使如何工作的?

在特殊设置的上下文环境中。指令和代码相比于直接的值更倾向于绑定到 `$sce.getTrusted(context, value)`\
的结果上。指令相比 `$parse` 监视属性的绑定更倾向使用 `$sce.parseAs`，因为它将执行 `$sce.getTrusted`
在非常量的字符场景之后。

举个例子，`ngBindHtml` 用了 `$sce.parseAsHtml(binding expression)` 方法，以下是代码
(稍微简化了一些)

> javascript

``` javascript
var ngBindHtmlDirective = ['$sce', function($sce) {
  return function(scope, element, attr) {
    scope.$watch($sce.parseAsHtml(attr.ngBindHtml), function(value) {
      element.html(value || '');
    });
  };
}];

```

---

## 对加载文档的影响

该服务应用于 `ng-include` 指令的效果与 指令内部 `templateUrl` 指定的模板效果一样好。

默认情况下， Angular 仅仅通过应用内部一致的域或协议来加载模板。可以通过在模板的URL上调用 `$sce.getTrustedResourceUrl`
方法来实现这个需求。或者加载来自其他域或协议的模板，要么将他们加入白名单，要么将它们包装成一个
可以信任的值。

请注意：浏览器的同源策略(Same Origin Policy)和跨域资源共享（Cross-Origin Resource Sharing - CORS）
策略的应用会进一步限制模板的加载的成功。这意味着，如果没有正确的CORS策略，从一个不同的域下加载模板，
是不会兼容所有浏览器的。同样地，从 `file://` 路径下加载的文档也会有一些浏览器不支持。

---

## 这让人感觉得不偿失

需要谨记 SCE 仅可应用于插值表达式中。

如果你的表达式是恒定不变的字符。它将自动地被信任并且不需要在它们身上调用 `$sce.trustAs`
就可以工作了（记得引用 `ngSanituze` 模块）。

> example

``` html

<div ng-bind-html="'<b>implicitly trusted</b>'"></div>

```

此外，`a[href]` 和 `img[src]` 自动校正它们的路径，并不会通过 `$sce.getTrusted` 传递它们。
SCE 此处不会发挥效用.

引入 `$sceDelegate` 合理的设置默认值来允许你从你应用的域里加载模板到 `ng-include`中，而不需要
知道SCE。它会阻止从其他域加载模板或者从 https 服务的文档向 http 加载文档。你可以通过设置你自己习惯的
白名单和黑名单去匹配一些URL来改变这些。

这显著地降低了开销。相比于在应用启动一段时间之后指定安全策略它真是容易的多，花费很少的时间就拥有了
一个安全的可审计验证的更为方便的应用。

---

## 支持那些收信人的上下文形式?

| 上下文 |	注意事项 |
|:---:|:---|
| `$sce.HTML` |	对于应用安全的 HTML 源。`ngBindHtml`指令使用他来绑定。如果遇到一个不安全的值并且 $sanitize 模块当前可使用，那么它将矫正这个值而不是抛出错误。|
| `$sce.CSS` |	对于应用安全的 CSS 源。当前未使用。 可以在你的指令了随意使用它。|
| `$sce.URL` |	对于作为链接安全的URL。当前未使用 (`<a href=` 和 `<img src=` 校正它们的路径并且不会构成一个SCE上下文环境。）|
| `$sce.RESOURCE_URL` |	不仅仅是对于作为链接安全的URL。并且链接引入到你应用中的内容也是安全的，例如引入 `ng-include`或是 `src`/`ngSrc` 绑定的 IMG 外的其他标签 (e.g. IFRAME, OBJECT, etc.) <br>需要注意的是 `$sce.RESOURCE_URL` 相比 `$sce.URL` 做了更为强大的声明。因此 `$sce.RESOURCE_URL` 信任的上下文要求值可以用于任何地方，而 `$sce.URL` 的则不行。
|`$sce.JS`|	可以在你的应用上下文中安全执行的 Javascript。当前未使用。可以在你的指令了随意使用它。|

---

## 格式化在资源路径白、黑名单中的项目

这些数组中的任意一个元素必须符合下面描述的其中一项：

- `self`
  + 特殊的字符串, `self` , 可用于匹配校验使用了相同协议的应用文档的相同域下的所有URL。
- 字符串 (除了特殊值 `self`)
  + 字符串符合完全标准化/资源被测试的绝对路径。(substring 匹配不够好.)
有两个通配符 - `*` 和 `**`. 所有其他字符都可以和它们匹配。
`*`: 匹配没有，或者多次出现的任意字符但不包括以下六个字符中的任意一个: `:`, `/`, `.`, `?`, `&` 和 `;`. 在白名单的设置中它使十分有用的。
`**`: 匹配没有，或者多次出现的任意字符。像这样, 他不适合匹配格式和域名。因为它将匹配太多(e.g. `http://**.example.com/` 将会匹配 `http://evil.com/?ignore=.example.com/` 这并不是我们所期望的) 它一般用于一个路径的结尾。(e.g. `http://foo.example.com/templates/**`).

正则表达式 (看下面的警告)

警告: 虽然正则表达式威力十足并且提供了极大的灵活性，他们的句法 (和所有不可避免的逃逸)使它们更难于维护。
它十分容易当某个人更新了一个复杂的表达式时十分偶然地引入一个错误（恕我直言，所有的正则表达式都应该有
良好的测试覆盖率）。例如 `.` 在正则中的用途仅在很少的情况下会被正确使用。

`A .` 字符在正则中会在匹配格式或者子域为 `a :` 或者 `字面量 .` 的时候使用。很可能不如我们所愿。
我们强烈推荐使用字符串模式，只在没有其他办法的时候使用正则表达式。

正则表达式必须是一个 `RegExp` 的实例。（即，不是一个字符串.）它匹配了整个标准化/资源被测试的绝对路径
（甚至当 `RegExp` 没有 `^` 和 `$` 的时候也是这样）。另外，出现在正则表达式中的任何标志
（诸如多行m、全局g、忽略大小写i）都会被忽略。

如果你是从其他模板引擎生成你的JavaScript（不建议如此，例如 #4006 issue），记得避免你的正则表达式
（要意识到你可能需要根据你的模板引擎和你插值的方式逃避的多个级别。）请使用您的平台的逃逸机制，
因为它可能是编码自己以前不够好。例如 Ruby 有 `Regexp.escape(str)`，Python 有 `re.escape`。
Javascript 内部则缺少一个类似的机制用于逃逸。看一看Google Closure library's
`goog.string.regExpEscape(s)`。

参阅 `$sceDelegateProvider` 的例子。

---

## 为你展示一个使用SCE的例子

> index.html

``` html

<div ng-controller="AppController as myCtrl">
  <i ng-bind-html="myCtrl.explicitlyTrustedHtml" id="explicitlyTrustedHtml"></i><br><br>
  <b>用户评论</b><br>
  默认情况下，如果在 $sanitize 可用，不是明确可信的HTML（例如 Alice的评论）会被校正。
  如果$sanitize不可用，这将导致一个错误，而不是开发。

  <div class="well">
    <div ng-repeat="userComment in myCtrl.userComments">
      <b>{{userComment.name}}</b>:
      <span ng-bind-html="userComment.htmlComment" class="htmlComment"></span>
      <br>
    </div>
  </div>
</div>
```

> script.js

``` javascript

angular.module('mySceApp', ['ngSanitize'])
.controller('AppController', ['$http', '$templateCache', '$sce',

  function($http, $templateCache, $sce) {

    var self = this;

    $http.get("test_data.json", {cache: $templateCache}).success(function(userComments) {
      self.userComments = userComments;
    });

    self.explicitlyTrustedHtml = $sce.trustAsHtml(
        '<span onmouseover="this.textContent=&quot;Explicitly trusted HTML bypasses ' +
        'sanitization.&quot;">Hover over this text.</span>');
  }]);

```

> test_data.json

``` json

[
  { "name": "Alice",
    "htmlComment":
        "<span onmouseover='this.textContent=\"PWN3D!\"'>Is <i>anyone</i> reading this?</span>"
  },
  { "name": "Bob",
    "htmlComment": "<i>Yes!</i>  Am I the only other one?"
  }
]

```

> protractor.js

``` javascript
describe('SCE doc demo', function() {
  it('should sanitize untrusted values', function() {
    expect(element.all(by.css('.htmlComment')).first().getInnerHtml())
        .toBe('<span>Is <i>anyone</i> reading this?</span>');
  });

  it('should NOT sanitize explicitly trusted values', function() {
    expect(element(by.id('explicitlyTrustedHtml')).getInnerHtml()).toBe(
        '<span onmouseover="this.textContent=&quot;Explicitly trusted HTML bypasses ' +
        'sanitization.&quot;">Hover over this text.</span>');
  });
});

```

---

## 我也以在应用开始生效时禁用 SCE 吗?

当然可以。 然而，我们强烈建议你不要这么做。SCE 将给予你非常多的安全益处和很少的代码开销。掌控一个
SCE失效的应用更为困难。不论是自己去验证安全，或者在之后的阶段启动SCE。某些情况下禁用SCE是合理的，
有很多在引入SCE前就编写了的代码并且你在迁移他们到一个模块中。

所以, 以下是如何在应用开始生效时禁用 SCE:

> javascript

``` javascript
angular.module('myAppWithSceDisabledmyApp', []).config(function($sceProvider) {
  // 应用开始生效时禁用 SCE.  仅供演示!
  // 不再新项目中使用.
  $sceProvider.enabled(false);
});
```

---

## 用法

`$sce`

---

## 方法

### 1.`isEnabled()`
返回一个布尔值来说明 SCE 是否启用了

##### *返回*


`Boolean` -  如果启用则返回`true`, 其他情况则返回 `false`，如果你想设置这个值，
你可以在模块初始化配置时使用 `$sceProvider` 来完成。

### 2.`parseAs(type, expression)`

将Angular 表达式转化成一个函数，这像是当表达式是常量时的 `$parse`。除此之外，它通过调用
`$sce.getTrusted(type, result)` 来包装这个表达式。

##### *参数*

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
| type | `string` | 输出的结果使用哪一种 SCE 上下文种类|
| expression | `string` | 用于编译的字符串表达式 |


##### *返回*

`function(context, locals)` 一个代表了编译表达式的函数:

`context` – `{object}` – 一个针对字符串形式的表达式的评估结果的对象(通常为一个 `scope` 对象).
`locals` – `{object=}` – 局部变量上下文对象, 可用于覆盖上下文范围内的值。


### 3.`trustAs(type, value)`

代理给 `$sceDelegate.trustAs`。像这样，返回一个受Angular信任的在规定的严格的上下文环境中逸出使用（
如 `ng-bind-html`，`ng-include`，任何的 `src` 属性的插值，任何 `dom` 事件绑定的属性插值如
 `onclick`等）使用所提供的值。了解 * $sce 关于启用严格的语境转义。

##### *参数*

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
| type | `string` | 指定使用的上下文类型。如 `url`, `resourceUrl`, `html`, `js` 或 `css`.|
| value | `*` | 被认为是可信/安全的值。|

##### *返回*


`*`	 在那些Angular期望的得到的 `$sce.trustAs()` 返回值的地方提供值


### 4.`trustAsHtml(value)`

简洁方法。`$sce.trustAsHtml(value)` → `$sceDelegate.trustAs($sce.HTML, value)`

##### *参数*

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
| value | `*` |trustAs 的值.|

##### *返回*

`*` 一个可以传给 `$sce.getTrustedHtml(value)` 后获得原值的对象（自定义的指令仅能接收
常量表达式或者通过 `$sce.trustAs` 返回的值）

### 5.`trustAsUrl(value)`

简洁方法。 `$sce.trustAsUrl(value)` → `$sceDelegate.trustAs($sce.URL, value)`

##### *参数*

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
| value | `*` |trustAs 的值.|

##### *返回*

`*` 一个可以传给 `$sce.getTrustedUrl(value)` 后获得原值的对象（自定义的指令仅能接收
常量表达式或者通过 `$sce.trustAs` 返回的值）

### 6.`trustAsResourceUrl(value);`
简洁方法。 `$sce.trustAsResourceUrl(value)` → `$sceDelegate.trustAs($sce.RESOURCE_URL, value)`

##### *参数*

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
| value | `*` |trustAs 的值.|

##### *返回*

`*` 一个可以传给 `$sce.getTrustedResourceUrl(value)` 后获得原值的对象（自定义的指令仅能接收
常量表达式或者通过 `$sce.trustAs` 返回的值）


### 7.`tgetTrusted(type, maybeTrusted);`

代理给 `$sceDelegate.getTrusted` 像这样, 拿到一个 `$sce.trustAs()` 调用的结果，如果查询的
上相问类型是创建类型的超类型则返回最初传入的参数。如果不成立，抛出一个例外。

##### *参数*

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
| type | `string` |指定上下文的类型.|
| maybeTrusted | `*` | 先被调用的 `$sce,trustAs` 的结果。|



##### *返回*

`*` 如果在上下文环境中是合法的返回的则是开始提供给 `$sce.trustAs` 的参数。否则抛出一个例外。


### 8.`tgetTrustedHtml(value);`

简洁方法。 `$sce.getTrustedHtml(value)` → `$sceDelegate.getTrusted($sce.HTML, value)`

##### *参数*

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
| value | `*` |传给 `$sce.getTrusted` 的值.|

##### *返回*

`*` $sce.getTrusted($sce.HTML, value)的返回值


### 9.`getTrustedCss(value);`

简洁方法。 `$sce.getTrustedCss(value)` → `$sceDelegate.getTrusted($sce.CSS, value)`

##### *参数*

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
| value | `*` |传给 `$sce.getTrusted` 的值.|

##### *返回*

`*` $sce.getTrusted($sce.CSS, value)的返回值

### 10.`getTrustedUrl(value);`

简洁方法。 `$sce.getTrustedUrl(value)` → `$sceDelegate.getTrusted($sce.URL, value)`

##### *参数*

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
| value | `*` |传给 `$sce.getTrusted` 的值.|

##### *返回*

`*`  $sce.getTrusted($sce.URL, value)的返回值

### 10.`getTrustedResourceUrl(value);`

简洁方法。 `$sce.getTrustedResourceUrl(value)` → `$sceDelegate.getTrusted($sce.RESOURCE_URL, value)`

##### *参数*

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
| value | `*` |传给 `$sce.getTrusted` 的值.|

##### *返回*

`*`  $sce.getTrusted($sce.RESOURCE_URL, value)的返回值

### 11.`getTrustedJs(value);`

简洁方法。 `$sce.getTrustedJs(value)` → `$sceDelegate.getTrusted($sce.JS, value)`

##### *参数*

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
| value | `*` |传给 `$sce.getTrusted` 的值.|


##### *返回*

`*`  $sce.getTrusted($sce.JS, value)的返回值

### 12.`parseAsHtml(expression);`

简洁方法。 `$sce.parseAsHtml(expression string)` → `$sce.parseAs($sce.HTML, value)`

##### *参数*

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
| type | `string` | 输出的结果使用哪一种 SCE 上下文种类|
| expression | `string` | 用于编译的字符串表达式 |


##### *返回*

`function(context, locals)` 一个代表了编译表达式的函数:

`context` – `{object}` – 一个针对字符串形式的表达式的评估结果的对象(通常为一个 `scope` 对象).
`locals` – `{object=}` – 局部变量上下文对象, 可用于覆盖上下文范围内的值。

### 13.`parseAsCss(expression);`

简洁方法。 `$sce.parseAsCss(value)` → `$sce.parseAs($sce.CSS, value)`

##### *参数*

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
| type | `string` | 输出的结果使用哪一种 SCE 上下文种类|
| expression | `string` | 用于编译的字符串表达式 |


##### *返回*

`function(context, locals)` 一个代表了编译表达式的函数:

`context` – `{object}` – 一个针对字符串形式的表达式的评估结果的对象(通常为一个 `scope` 对象).
`locals` – `{object=}` – 局部变量上下文对象, 可用于覆盖上下文范围内的值。

### 14.`parseAsUrl(expression);`

简洁方法。 `$sce.parseAsUrl(value)` → `$sce.parseAs($sce.URL, value)`

##### *参数*

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
| type | `string` | 输出的结果使用哪一种 SCE 上下文种类|
| expression | `string` | 用于编译的字符串表达式 |


##### *返回*

`function(context, locals)` 一个代表了编译表达式的函数:

`context` – `{object}` – 一个针对字符串形式的表达式的评估结果的对象(通常为一个 `scope` 对象).
`locals` – `{object=}` – 局部变量上下文对象, 可用于覆盖上下文范围内的值。




### 15.`parseAsResourceUrl(expression);`

简洁方法。 `$sce.parseAsResourceUrl(value)` → `$sce.parseAs($sce.RESOURCE_URL, value)`

##### *参数*

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
| type | `string` | 输出的结果使用哪一种 SCE 上下文种类|
| expression | `string` | 用于编译的字符串表达式 |


##### *返回*

`function(context, locals)` 一个代表了编译表达式的函数:

`context` – `{object}` – 一个针对字符串形式的表达式的评估结果的对象(通常为一个 `scope` 对象).
`locals` – `{object=}` – 局部变量上下文对象, 可用于覆盖上下文范围内的值。



### 16.`parseAsJs(expression);`

简洁方法。 `$sce.parseAsJs(value)` → `$sce.parseAs($sce.JS, value)`

##### *参数*

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
| type | `string` | 输出的结果使用哪一种 SCE 上下文种类|
| expression | `string` | 用于编译的字符串表达式 |


##### *返回*

`function(context, locals)` 一个代表了编译表达式的函数:

`context` – `{object}` – 一个针对字符串形式的表达式的评估结果的对象(通常为一个 `scope` 对象).
`locals` – `{object=}` – 局部变量上下文对象, 可用于覆盖上下文范围内的值。
