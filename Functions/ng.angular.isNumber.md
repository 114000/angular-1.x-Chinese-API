# `angular.isNumber`
- Angular@1.4.7
- `ng` 模块中的函数

判断值是否是一个数值。

包括‘特殊’的数值 `NaN`，`+Infinity`，`-Infinity`

如果你希望排除这些，可以使用原生的 `isFinite` 方法。


## 用法

`angular.isNumber(value)`

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|value|`*`| 检测值|


#### 返回

`boolean`	如果 `value` 是一个数值 ，则返回 `true`
