---
layout: post
title: Angular_1.4.3 API 服务篇 $sceDelegate
desc: '`$sceDelegate` 是一个基于 `$sce` 为 AngularJS 提供SCE 的服务。'
categories: jekyll update
tags:
- API
- AngularJS
---

# Angular_1.4.3 API 服务篇 $sceDelegate

- $sceDelegateProvider
- ng模块中的服务

`$sceDelegate` 是一个基于 `$sce` 为 AngularJS 提供SCE 的服务。

通常情况下，你可以自定义 SCE 来初始化或者覆盖 `$sceDelegate` 来代替 `$sce` 服务在 AngularJS
中工作。这是因为虽然 `$sce` 提供了众多的简洁写法的方法，但你真的只需要覆盖3个核心的函数（
`trustAs`，`getTrusted` 和 `valueOf`）就可以改变他们的工作方式，原因在于 `$sce` 将这些
操作委托给 `$sceDelegate` 去做了。

调用 `$sceDelegateProvider` 来初始化这个服务。

默认的 `$sceDelegate` 实例将会制定出有一点蛋疼的规则，虽然你可以在应用启动后覆盖它来改变 `$sce`
的行为，但是常见的解决办法是调用 `$sceDelegateProvider`， 而不是通过设置你自己的白名单和黑名单
来使用信任的 URL 加载模板这类 AngularJS 资源。详请参见
`$sceDelegateProvider.resourceUrlWhitelist`和 `$sceDelegateProvider.resourceUrlBlacklist`

---

## 用法

`$sceDelegate()`

---

## 方法

### 1.`trustAs(type, value)`

返回一个受Angular信任的在规定的严格的上下文环境中逸出使用（如 `ng-bind-html`，`ng-include`，任何的 `src` 属性的插值，任何 `dom` 事件绑定的属性插值如`onclick`等）使用所提供的值。了解 * $sce 关于启用严格的语境转义。


**参数**

| 参数 | 形式 | 具体 |
|:---|:---:|:---|
|type|`string`|	指定使用的上下文类型。如 `url`, `resourceUrl`, `html`, `js` 或 `css`.|
|value|`*`|被认为是可信/安全的值。|

**返回**

`*`	在那些Angular期望的得到的 `$sce.trustAs()` 返回值的地方提供值

### 2.`valueOf(value)`

如果传入的参数已经被之前调用过的 `$sceDelegate.trustAs` 那么则返回已经被传入过
`$sceDelegate.trustAs` 的值。如果参数不是一个被 `$sceDelegate.trustAs` 返回的值，则返回它自己。

**参数**

| 参数 | 形式 | 具体 |
|:-----|:----:|:-----|
|value|`*`| 先前调用 `$sceDelegate.trustAs` 的结果，或者任何其他的值。|


**返回**

`*` 在那些Angular期望的得到的 `$sce.trustAs()` 返回值的地方提供值。如果值是这样一个调用的结果，
那么它将是最初提供给 `$sceDelegate.trustAs` 的值。否则返回的值不变。

### 3.`getTrusted(type, maybeTrusted)`

获取一个 `$sceDelegate.trustAs` 调用的结果，并在查询的上下文类型是创建类型的超集的情况下返回最初提供的值。
如果不满足这个条件，则抛出一个异常。

**参数**

| 参数 | 形式 | 具体 |
|:-----|:----:|:-----|
|type|`string`|	这个值被用于何种类型的上下文|
|maybeTrusted|`*`| 先前调用 `$sceDelegate.trustAs` 的结果。|

**返回**
`*` -	在上下文合法的情况下，返回的是最先提供给 `$sceDelegate.trustAs` 的值。否则，将抛出一个异常
