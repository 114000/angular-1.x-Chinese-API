# `angular.bind`
- Angular@1.4.7
- `ng` 模块中的函数

返回一个绑定在 `self` 上的 `fn` 函数（`self` 会成为 `fn` 中的 `this`）。你可以提供一些可选的参数（args）
预先绑定到函数上。这个功能也被称为部分应用程序，或柯里话。

## 用法

`angular.bind(self, fn, args)`

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|self|`object`| `fn` 对应的上下文环境。|
|fn|`function()`| 被绑定的函数|
|args|`*`| 预先绑定到 `fn` 函数上的参数|

#### 返回

`function()`	使用指定绑定了的 `fn` 的封装
