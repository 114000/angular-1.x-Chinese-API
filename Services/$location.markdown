---
layout: post
title: Angular_1.4.3 API 服务篇 $location
desc: '`$location` 服务格式化了浏览器地址栏中的 URL (基于 `window.location` 对象)，
并让它在你的应用中可以使用。改变地址栏中的 URL 将反馈到 `$location` 服务中而且改变 `$location`
也同样会反馈到浏览器地址栏中。'
categories: jekyll update
tags:
- API
- AngularJS
---


# $location

- $locationProvider
- ng 模块中的服务

`$location` 服务格式化了浏览器地址栏中的 URL (基于 `window.location` 对象)，
并让它在你的应用中可以使用。改变地址栏中的 URL 将反馈到 `$location` 服务中而且改变 `$location`
也同样会反馈到浏览器地址栏中。


`$location` 服务：

- 暴露了浏览器地址栏中的当前 URL， 所以你可以：
  * 观察监听 URL.
  * 修改 URL.
- 与浏览器的 URL 保持同步，当用户：
  * 修改地址栏
  * 点击了前进或者后退按钮(或点击了一个历史链接).
  * 页面中点击了一个链接
- 用一组方法表示了 URL 对象 (`protocol`, `host`, `port`, `path`, `search`, `hash`).

更多信息请参见开发者文档：使用 `$location`

---

## 依赖

`$rootElement`

---

## 方法

### 1.`absUrl()`

这个方法仅用于获取

``` javascript
// 地址栏中的 URL http://example.com/#/some/path?foo=bar&baz=xoxo

var absUrl = $location.absUrl();

// => sting : "http://example.com/#/some/path?foo=bar&baz=xoxo"

```
**返回**

`string` - 返回带有所有根据 `RFC 3986` 编码的部分的完整 URL


### 2.`url([url])`

这个方法既可以获取也可以设置


没传入参数时返回 url (如：/path?a=b#hash)

当传入参数时则会修改 `path`, `search` 和 ·hash` ，并返回 `$location`。

``` javascript
// 地址栏中的 URL http://example.com/#/some/path?foo=bar&baz=xoxo

var url = $location.url()

// => sting : "/some/path?foo=bar&baz=xoxo"

```

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
| url *（可选）*| string| 新的 url，不需指定基础前缀（如：`'/path?a=b#hash'`）|

**返回**

`string` - url

### 3.`protocol()`

这个方法仅用于获取

返回当前 url 的协议

``` javascript
// 地址栏中的 URL http://example.com/#/some/path?foo=bar&baz=xoxo

var protocol = $location.protocol()

// => sting : "http"
```

**返回**

`string` - 返回当前 url 的协议

### 4.`host()`

这个方法仅用于获取

返回当前 URL 的域名、主机名

Note: 相比于不使用 Angular 版本的 `location.host` 返回的 `域名:端口`，`$location.host()`
 仅返回域名。

``` javascript
// 地址栏中的 URL http://example.com/#/some/path?foo=bar&baz=xoxo

var host = $location.host();

// => "example.com"


// 地址栏中的 URL http://user:password@example.com:8080/#/some/path?foo=bar&baz=xoxo

host = $location.host();

// => "example.com"


host = location.host;

// => "example.com:8080"

```

**返回**

`string` - 当前 URL 的域名、主机名

### 5.`port()`

这个方法仅用于获取

返回当前 URL 的端口号

``` javascript
// 地址栏中的 URL http://example.com/#/some/path?foo=bar&baz=xoxo

var port = $location.port()

// => number : 80
```

**返回**

`number` - 端口

### 6.`path([path])`

这个方法既可以获取也可以设置

不传入参数则返回当前 URL 的路径部分

传入参数则修改路径部分并返回 `$location`

*注意*：路径必须以斜杠开头（`/`），如果没有斜杠这个方法将会将其补充上

``` javascript
// 地址栏中的 URL http://example.com/#/some/path?foo=bar&baz=xoxo

var path = $location.path()

// => string : "/some/path"

```


**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|path *（可选）*| `string`<br>`number`| 新的路径部分

**返回**

`string` - 路径部分

### 7.`search(search, [paramValue])`

这个方法既可以获取也可以设置

当没传入参数时， 返回当前 URL 的搜索部分 (以对象的形式) 即`?`后的部分

传入参数时，将会改变搜索部分并返回 `$location`

``` javascript
// 地址栏中的 URL  http://example.com/#/some/path?foo=bar&baz=xoxo

var searchObject = $location.search()

// => object : {foo: 'bar', baz: 'xoxo'}


// set foo to 'yipee'
$location.search('foo', 'yipee')

// => $location
// $location.search() => object {foo: 'yipee', baz: 'xoxo'}

```

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|search|	`string`<br>`Object.<string>`<br>`Object.<Array.<string>>`|新的搜索部分的参数 - 字符串或哈希对象<br>当只传入一个参数时，这个方法是一个设置动作，将 `$location` 的搜索部分组件设置为一个指定的值。<br>如果传入的参数是一个包含一组值的哈希对象，那么这些值将会被作为URL 中复制的搜索部分参数被编码|
|paramValue *（可选）*|`string`<br>`Number`<br>`Array.<string>`<br>`boolean`|如果搜索部分是一个数字或者字符串，paramValue 将只覆盖一个单独的搜索部分的属性。<br>如果 paramValue 是一个数组，它会覆盖 `$location` 的通过第一个参数指定的搜索组件的属性<br>如果 paramValue 是 `null`，通过第一个参数指定的实行将会被删除<br>如果 paramValue 是 `true`，通过第一个参数指定的实行将会被添加进去，但是不会赋值也没有尾随等号|

**返回**

`Object` - 如果没传递参数则返回格式化后的搜索部分的对象，如果传递了多于一个的参数则返回
 `$location` 对象


### 8.`hash([hash])`

这个方法既可以获取也可以设置

没传入参数时返回哈希码

传入参数则修改哈希码并返回 `$location`

``` javascript
// 地址栏中的 URL http://example.com/#/some/path?foo=bar&baz=xoxo#hashValue

var hash = $location.hash();

// => string : "hashValue"
```

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|hash *（可选）*| `string`<br>`number`| 新的哈希码|

**返回**

`string` - 哈希码

### 9.`replace()`

如果调用，所有当前 `$digest` 下的对 `$location` 的修改都将会取代历史记录。而不是添加一个新的。

### 10.`state([state])`

这个方法既可以获取也可以设置

没传入参数时返回历史状态对象，当传入参数时修改历史状态对象并返回 `$location`. 这个状态对象稍后会
传递给 `pushState` 或 `replaceState`

*注意*： 这个方法仅在HTML5模式下被支持，而且只有在浏览器支持 `HTML5 历史 API` 的情况下可以使用。
（即，`pushState` 和 `replaceState`），若你想兼容更旧的浏览器（想 IE9 或 安卓4.0以下的设备），
就不要使用这个方法。

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|state *（可选）*| `object` | 可传递给 `pushState` 或 `replaceState` 的对象|

**返回**

`object` - 状态

---

## 事件

### 1.`$locationChangeStart`

在 URL 修改前向下传播

针对 URL 的修改可以通过调用事件对象中的 `preventDefault`方法来阻止。详情请参阅
`$rootScope.Scope` 来对事件对象有更详细的了解。一旦 URL 成功修改 `$locationChangeSuccess`
将会被调用。

当浏览器支持 `HTML5 History API` 并在 HTML5 模式下时 ，`newState` 和 `oldState` 参数才会被定义

**方式**: `broadcast` 向下传播

**起始**: 根作用域

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|angularEvent| `object`|合成的事件对象|
|newUrl|`string`| 新的 URL|
|oldUrl *（可选）*|`string`| 被修改之前的 URL|
|newState *（可选）*|`string`| 新的历史状态对象|
|oldState *（可选）*|`string`| 被修改之前的 URL|

### 2.`locationChangeSuccess`

URL 修改后向下传播

当浏览器支持 `HTML5 History API` 并在 HTML5 模式下时 ，`newState` 和 `oldState` 参数才会被定义


**方式**: `broadcast` 向下传播

**起始**: 根作用域

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|angularEvent| `object`|合成的事件对象|
|newUrl|`string`| 新的 URL|
|oldUrl *（可选）*|`string`| 被修改之前的 URL|
|newState *（可选）*|`string`| 新的历史状态对象|
|oldState *（可选）*|`string`| 被修改之前的 URL|
