# angular.bind
- Angular@1.4.7
- `ng` 模块中的函数

Returns a function which calls function fn bound to self (self becomes the this for fn). You can supply optional args that are prebound to the function. This feature is also known as partial application, as distinguished from function currying.

## 用法

`angular.bind(self, fn, args)`

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|self|`object`| Context which fn should be evaluated in.|
|fn|`function()`| Function to be bound.|
|args|`*`|Optional arguments to be prebound to the fn function call.|

#### 返回

`function()`	Function that wraps the fn with all the specified bindings.
