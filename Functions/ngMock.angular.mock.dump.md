# `angular.mock.dump`
- Angular@1.4.7
- `ngMock` 模块中的函数

NOTE: 这不是一个可注入的实例。仅仅是一个全局函数。

将 angular 对象（scope, elements,等）序列化成字符串的方法。用于调试。

这个方法也可以从 `window` 上得到来在控制台中展示对象。

## 用法

`angular.mock.dump(object)`

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|object|`*`|任何需要转化成字符串的对象|


#### 返回

`string`	参数序列化成的字符串。
