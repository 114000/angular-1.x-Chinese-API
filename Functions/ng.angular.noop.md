# `angular.noop`
- Angular@1.4.7
- `ng` 模块中的函数

不执行任何操作的函数，这个函数用于函数式编程方式。

``` javascript
function foo(callback) {
  var result = calculateResult();
  (callback || angular.noop)(result);
}
```

## 用法

`angular.noop()`
