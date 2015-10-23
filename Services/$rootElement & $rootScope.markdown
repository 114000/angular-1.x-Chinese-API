---
layout: post
title: "Angular_1.4.3 API 服务篇 $rootElement & $rootScope"
desc: "简单的打印日志的服务。默认实现安全的写入信息到浏览器的控制台（如果存在的话）。"
categories: jekyll update
tags:
- API
- AngularJS
---

# Angular_1.4.3 API 服务篇 $rootElement

- `ng` 模块中的服务

`$rootScope` 是Angular应用中的根元素。它或是 `ngApp`声明处的元素， 或是传入 `Angular.bootstrap` 方法中的元素。它代表应用的根元素。它也是应用的 `$injector` 服务被暴露的位置，并能用 `$rootElement.injector()`方法检索到。

---
# Angular_1.4.3 API 服务篇 $rootScope

- `$rootScopeProvider`
- `ng` 模块中的服务

每个应用都有一个单独的根作用域。所有在这个应用的其他的作用域都是它的子代作用域。作用域通过一个监听模型变化的机制来为在视图和模型之间提供分离功能。他们还可以提供事件的冒泡/捕获和和订阅功能。详情参见`scope` 的开发文档。
