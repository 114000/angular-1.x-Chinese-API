# `angular.merge`
- Angular@1.4.7
- `ng` 模块中的函数

通过在源对象上枚举属性并复制到自己身上来达成深度扩展目标对象 `dst` 。你可以指定多个源对象，如果你想保留原始对象，你可以传入一个空对象作为目标点 `var object = angular.merge({}, object1, object2)`

不像 `extend()` 那样，`merge()` 会递归地深入到源对象的对象属性中，进行一个深度的复制。

## 用法

`angular.merge(dst, src)`

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|dst|`object`| 目标对象。|
|src|`object`| 源对象。|

#### 返回

`object`	dst.
