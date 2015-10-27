# `angular.isFunction`
- Angular@1.4.7
- `ng` 模块中的函数

判断值是否是一个对象。并不像 JavaScript 中 `typeof` 显示的那样。`null` 并不会被当做对象。
并且要注意 JavaScript 数组也是对象。


## 用法

`angular.isObject(value)`

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|value|`*`| 检测值|


#### 返回

`boolean`	如果 `value` 是一个对象，并且不是 `null`，则返回 `true`
