---
layout: post
title: Angular_1.4.3 API 服务篇 $templateCache & $templateRequest
desc: '关于模板的两个服务，缓存和请求'
categories: jekyll update
tags:
- API
- AngularJS  
---
# Angular_1.4.3 API 服务篇 $templateCache

- [ng](https://docs.angularjs.org/api/ng)模块中的服务

在模板第一次被引用时,它会被载入到模板缓存中以便快速的检索。你可以直接使用一个script标签来添加一个
模板，或者直接通过 `$templateCache` 服务来达到相同的效果。


通过 script 标签添加:

> html

``` html

<script type="text/ng-template" id="templateId.html">
  <p>This is the content of the template</p>
</script>

```
**注意**：script 标签包含的模板并不需要 `document` 的头部，但它必须是 `$rootElement` 的
后代(IE，带有 `ng-app` 属性的元素)，否则这个模板将被忽略掉。

通过`$templateCache` 服务添加:

> javascript

``` javascript
var myApp = angular.module('myApp', []);
myApp.run(function($templateCache) {
  $templateCache.put('templateId.html', 'This is the content of the template');
});
```
在检索模板之后，你就可以在你的 HTML 里轻松的使用了。

> html

``` html
<div ng-include=" 'templateId.html' "></div>
```


或者通过 Javascript 得到它 :

> javascript

``` javascript
$templateCache.get('templateId.html')
```

请看 [`$cacheFactory`](https://docs.angularjs.org/api/ng/service/$cacheFactory).

---

# Angular_1.4.3 API 服务篇 $templateRequest

- [ng](https://docs.angularjs.org/api/ng)模块中的服务

`$templateRequest` 服务

`$templateRequest` 服务使用 `$http` 服务下载模板，并在之后进行安全检测。如果成功，
则内容会被存储到 `$templateCache` 中。如果HTTP请求失败或者该请求的响应数据为空，
会抛出一个编译（`$compile`）错误（这个行为可以通过将函数的第二个参数设置为 `true` 来阻止）。

需要注意的是，`$templateCache` 的内容是可以信任的，因此当 `tpl` 是以字符串形式出现的时候，
或者通过 `$templateCache` 作为入口时，是可以不必去调用 `$sce.getTrustedUrl(tpl)` 来验证的。



---
## 用法

`$templateRequest(tpl, [ignoreRequestError]);`

##### *参数*

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|tpl|`string` `TrustedResourceUrl`| HTTP 请求的模板路径(URL) |
|ignoreRequestError *(可选)*|`boolean`|是否忽略请求失败或者模板为空的情况|

##### *返回*

`promise`	- 一个表示通过给定的路径请求得到的 HTTP 相应数据的 `promise`

---

## 属性

`totalPendingRequests` - `number` - 所有被需求下载模板的总数
