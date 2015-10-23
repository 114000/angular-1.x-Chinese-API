---
layout: post
title: Angular_1.4.3 API 服务篇 $controller
desc: '`$controller` 服务负责实例化控制器。它仅仅是对 `$injector` 的简单调用，之所以提取成服务，是为了方便BC版本可以覆盖这个服务。'
categories: jekyll update
tags:
- API
- AngularJS
---

# $controller

- $controllerProvider
- ng 模块中的服务

`$controller` 服务负责实例化控制器。

它仅仅是对 `$injector` 的简单调用，之所以提取成服务，是为了方便BC版本可以覆盖这个服务。

---

## 依赖
`$injector`

---

## 用法

`$controller(constructor, locals)`

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|constructor|	`function()` <br> `string`|如果传入一个函数，那么它被认为是控制器的构造函数。否则它会被认为是用于检索所述控制器构造函数的一个字符串。<br>用法如以下步骤：<br> - 检查给定的名字是否已经通过 `$controllerProvider` 注册成为控制器<br> - 检查字符串是否会在当前作用域返回一个构造函数，如果为 `$controllerProvider#allowGlobals`，则在全局的 `window` 对象上验证 `window[constructor]` (不建议这么做)<br> - 当控制器实例在某个作用域内被发布为指定的属性时，该字符串可以使用控制器的属性语法；该作用域必须被注入到局部的参数中以确保能正常工作。|
|locals|`Object`|控制器要注入的域|

**返回**


`Object` - 给定控制器的实例
