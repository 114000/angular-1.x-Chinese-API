# `angular.forEach`
- Angular@1.4.7
- `ng` 模块中的函数

为对象或数组中的每一个项调用一次迭代函数。迭代函数被 `iterator(value, key, obj)` 调用，
其中 `value` 为对象的一个属性的值，或者数组中的一个元素，`key` 是这个对象属性的键名，
或者为数组元素的索引，`obj` 则为对象或数组本身，可以为这个函数指定一个执行上下文环境。

值得注意的是 `.forEach` 不会遍历继承的属性，因为它会使用 `hasOwnProperty` 方法进行过滤。

不像 ES262 标准的 `Array.prototype.forEach`，会在 `obj` 中的值有 `undefined` 或是
`null` 抛出类型错误，我们只会直接返回这些值。

``` javascript
var values = {name: 'misko', gender: 'male'};
var log = [];
angular.forEach(values, function(value, key) {
  this.push(key + ': ' + value);
}, log);
expect(log).toEqual(['name: misko', 'gender: male']);
```


## 用法

`angular.forEach(obj, iterator, [context])`

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|obj|`Object`<br>`Array`| 迭代的对象。|
|iterator|`function`| 迭代函数|
|context（可选）|`Object`|这个对象会成为迭代器函数的上下文（即 `this` 的指向）|

#### 返回

`object`/`array` `obj`
