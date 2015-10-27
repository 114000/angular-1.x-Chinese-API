# `angular.identity`
- Angular@1.4.7
- `ng` 模块中的函数

一个返回它第一个参数的函数，这个函数用于函数式编程方式。

``` javascript
function transformer(transformationFn, value) {
  return (transformationFn || angular.identity)(value);
};
```

## 用法

`angular.identity(value)`

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|value|`*`| 被返回的值|

#### 返回

`*`	传入的值
