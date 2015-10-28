# `angular.module`
- Angular@1.4.7
- `ng` 模块中的函数

`angular.module` 是一个用于创建，注册和检索 Angular 模块的全局方法。一个应用中所需要使用的所有所有模块
（angular 核心或第三方）都需要使用这种机制注册。

传入一个参数是检索一个存在的 Angular 模块（`angular.Module`），传入多个参数则是创建一个新的 Angular 模块。

## 模块

模块是服务（services），指令（directives），控制器（controllers），过滤器（filters）
和配置信息的集合。`angular.module` 被用于配置 `$injector`

```javascript
// 创建一个新的模块
var myModule = angular.module('myModule', []);

// 注册一个新服务
// 译：创建服务有很多种方式 value service factory provider 等
myModule.value('appName', 'MyCoolApp');

// 将现有的服务配置到初始化部分中。
myModule.config(['$locationProvider', function($locationProvider) {
  // 配置现有的 providers
  $locationProvider.hashPrefix('!');
}]);
```
你可以像下面这样创建注射器，并加载模块：
``` javascript
var injector = angular.injector(['ng', 'myModule'])
```

然而更多可能是你仅仅会使用 `ngApp` 或者 `angular.bootstrap` 来简化这个过程。



## 用法

`angular.module(name, [requires], [configFn])`

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|name|`string`| 创建或检索的模块的名字。|
|requires （可选）|`!Array.<string>=`| 如果给定则新建一个模块，没有传入则检索模块做进一步配置。|
|configFn （可选）|`Function=`| 模块的可选配置函数。和 Module#config() 相同。|

#### 返回

`module`	带有 `angular.module` api 的新模块
