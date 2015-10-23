---
layout: post
title: Angular_1.4.3 API 介绍
desc: '关于Angular的一些简介，译自angularjs.org'
categories: jekyll update
tags:
- API
- AngularJS
---

#Angular_1.4.3 API 介绍

欢迎来到 AngularJS API 文档网页。这些网页包含了 AngularJS 1.4.3 版本的一些参考资料。

在一个 AngularJS 的应用中，包含了各种组件的文件被组织到不同的模块中。这些组件是 directives,
services, filters, providers, 模板, 全局APIs和测试模块.

``` html

Angular 前缀 $ 和 $$:

  为了防止与你的代码发生命名冲突，Angular 公共对象的前缀名以 `$` 开头,
  而私有对象的前缀名以 `$$` 开头。请尽量避免在你的代码中使用 `$` 或 `$$` 作为你的前缀。

```

---

# Angular 模块

---

## ng (核心模块)

> 这是默认提供的模块，并包含了 AngularJS 的核心组件。

|||
|---|:---|
| Directives |	这是核心的指令(directive)集, 你可以将其应用到你的模板代码中来建立一个 AngularJS 的应用。<br>其中包含了: `ngClick`, `ngInclude`, `ngRepeat` 等等|
|Services / Factories| 这是可以供你的应用的 DI使用的核心的服务(service)集<br> 其中包含了: `$compile`, `$http`, `$location`, 等等 |
|Filters| 在模板数据被指令和表达式渲染之前，ng模块中的核心过滤器（filter）会对其做出一些转换或改变。<br>其中包含了: `filter`, `date`, `currency`, `lowercase`, `uppercase`, 等等|
| 全局 APIs |核心的全局API函数被绑定到 `angular` 对象上。这些核心的函数使有助于你在你的应用中使用底层的 Javascript 操作。<br>其中包含了: `angular.copy()`, `angular.equals()`, `angular.element()`, 等等...|


---

## ngRoute

> ngRoute 可以使你可以在你的应用实现 URL 路由 ngRoute 模块可以通过hashbang 和 HTML5 pushState 两种方式支持 URL 管理。


引入 angular-route.js 文件，并在你的应用是设置一个 `ngRoute` 依赖


|||
|---|:---|
|Services / Factories| 下面的服务是用来管理路由的:<br>- $routeParams 用来访问 URL 当前的查询字符串值<br>- $route 用来获取当前被访问路由的详细信息。<br>- $routeProvider 用来为应用注册路由|
|Directives|`ngView` 指令将会在页面中展示当前路由的模板。|

---

## ngAnimate

> 使用 ngAnimate 就可以在你的应用中实现动画效果。当ngAnimate 被引入时，许多核心的 `ng` 指令都会
在你的应用中提供动画钩子函数。动画可以使用 CSS `transition` / `animations`或者 Javascript回调函数来定义

引入 angular-animate.js 文件，并在你的应用是设置一个 ngAnimate 依赖

|||
|---|:---|
| Services / Factories | 在你的指令代码中用 `$animate`来触发动画操作|
| 以 CSS 为基础的动画 | 在 Angular 中，使用 `ngAnimate` 的 CSS 命名规则来引用 CSS 的 `transition` 或关键帧动画。一次定义，动画就总是会被 HTML模板代码中引用的 CSS 类触发。|
|以 JS 为基础的动画 |	调用 `module.animation()` 方法来注册一个 Javascript 动画，一次注册，动画就总是会被 HTML模板代码中引用的 CSS 类触发。|


---

## ngAria  *可能不是这样的囧*

> 使用 `ngAria` 注入公共的可访问属性到指令中并提升残疾人用户的体验

引入 angular-aria.js 文件，并在你的应用是设置一个 `ngAria` 依赖

|||
|---|:---|
|Services| `$aria` 服务包含了辅助方法来申请 ARIA 属性值为 `HTML.$ariaProvider`用于配置ARIA的属性。|

---

## ngResource

> 当你需要使用一个 REST API 来获取和发送数据时，你可以使用 `ngResource`

引入 angular-resource.js 文件，并在你的应用是设置一个 `ngResource` 依赖

|||
|---|:---|
|Services / Factories| `$resource` 服务用来定义 使用 REST API 通信的 RESTful 对象|

---

## ngCookies

> 在你的应用中你可以使用 `ngCookies` 模块处理 cookie 的管理。

引入 angular-cookies.js 文件，并在你的应用是设置一个 `ngCookies` 依赖

|||
|---|:---|
|Services / Factories|	下面的服务可用于cookie 的管理:<br> - `$cookie` 服务是一个浏览器 cookie 的便捷的封装，可以用来存储一些简单的数据<br> - `$cookieStore` 用于序列化存储更为复杂的数据。|

---

## ngTouch

> 当你为手机浏览器或设备开发时可以使用 `ngTouch` 模块。

引入 angular-touch.js 文件，并在你的应用是设置一个 `ngTouch` 依赖

|||
|---|:---|
|Services / Factories |	`$swipe`服务用来注册和处理移动端的 DOM 事件|
|Directives|	`ngTouch` 中有很多指令可以模拟移动端的 DOM 事件。|


---

## ngSanitize

> 在你的应用中，`ngSanitize`可以帮你安全的解析和操纵 HTML 数据

引入 angular-sanitize.js 文件，并在你的应用是设置一个 `ngSanitize` 依赖

|||
|---|:---|
|Services / Factories| `$sanitize` 服务是一个用来清理危险的HTML代码的快速且便利的途径。|
|Filters | `linky` 过滤器用于，在提供的字符串内将 URL 转化成 HTML 链接|


---

## ngMock

> 在你的单元测试中使用 `ngMock` 注入并测试 `module`, `factory`, `service`, `provider`

引入 angular-mocks.js 文件，来测试运行

|||
|---|:---|
|Services / Factories| `ngMock` 会以同步的方式拓展许多核心服务的行为来使它们拥有良好的测试性并易于管理<br>举一些例子: `$timeout`, `$interval`, `$log`, `$httpBackend`, 等等|
|Global APIs| 更有多种帮助性的函数来注入并测试模块在单元测试代码中<br>例如 `inject()`, `module()`, `dump()`, 等等|
