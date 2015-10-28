# `angular.mock.module`
- Angular@1.4.7
- `ngMock` 模块中的函数

**注意**: 为了方便访问，这个方法也被暴露在了 `window` 上。
**注意**: 这个方法**只会在**使用 jasmine 或者 mocha 运行测试时被声明。

这个函数注册了一个模块配置代码。当 `inject` 创建注射器时，这个函数会用来收集配置信息。

用法示例可查看 `inject` （译：angular.mock.inject）

## 用法

`angular.mock.module(fns)`

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|fns|`string`<br>`function()`<br>`object`|任何数量的，表示为字符串的别名或匿名模块的初始化函数。这些模块用用于配置注射器。`ng` 和 `ngMock`
模块会自动加载。如果传入一个对象的字面量，则在这个模块中它会被注册成 value。键名成为模块名，键值成为返回值。|
