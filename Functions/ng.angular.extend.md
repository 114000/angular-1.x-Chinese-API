# `angular.extend`
- Angular@1.4.7
- `ng` 模块中的函数


使用复制源对象的枚举属性到 `dst` 对象上的方式来扩展目标对象（`dst`），你可以指定多个源对象，
如果你想保留原来的对象，你可以传入一个空对象作为目标对象：`var object = angular.extend({}, object1, object2)`

**注意**：`angular.extend` 不支持递归合并（深拷贝）。可以使用 `angular.merge` 实现。

## 用法

`angular.extend(dst, src)`

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|dst|`object`| 目标对象。|
|src|`object`| 源对象。|

#### 返回

`object`	dst.
