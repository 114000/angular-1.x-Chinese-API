# `angular.mock.inject`
- Angular@1.4.7
- `ngMock` 模块中的函数

**注意**: 为了方便访问，这个方法也被暴露在了 `window` 上。
**注意**: 这个方法**只会在**使用 jasmine 或者 mocha 运行测试时被声明。

这个注射函数将一个函数包裹进了一个可注入的函数。`inject()` 在每个测试中都创建了一个
`$incjector` 实例，之后会用于解析引用。

## 解析引用 (下划线包装)

通常，我们只会注入一个引用一次到 `beforeEach()` 块中，并在多个 `it()` 中使用。
为了达到这个目的我们必须将引用赋值给一个在 `describe()` 块中声明了的变量。
我们想要拥有一个命名的引用，这就会导致一个问题传入`inject()` 的函数中的参数会将外部的变量覆盖掉。

为了解决这个问题，被注入的参数可以使用下划线来封闭装饰，当引用名被解析的时候下划线会被注射器忽略。

举个例子：参数 `_myService_` 将会被当做 `myService` 引用来解析，由于它可以在函数体内当做 `myService`
被使用，所以之后我们可以将其赋值给外部作用域定义的变量。

``` javascript
// 在外部定义的引用变量
var myService;

// 将参数用下划线包装
beforeEach( inject( function(_myService_){
  myService = _myService_;
}));

// 在这个系列的测试中使用 myService
it('makes use of myService', function() {
  myService.doStuff();
});
```

同样可以查看 `angular.mock.module`

## 例子

一个典型的 jasmine 测试
Example of what a typical jasmine tests looks like with the inject method.

``` javascript
angular.module('myApplicationModule', [])
    .value('mode', 'app')
    .value('version', 'v1.0.1');


describe('MyApp', function() {

  // 你需要去载入你想要测试的模块
  // 默认只会加载 "ng" 模块
  beforeEach(module('myApplicationModule'));


  // inject() 用来注入所有给出函数的参数
  it('需要提供一个版本', inject(function(mode, version) {
    expect(version).toEqual('v1.0.1');
    expect(mode).toEqual('app');
  }));


  // inject 和 module 方法同样可以在 it 或 beforeEach 的内部使用
  it('需要覆盖版本并且测试新的版本被注入', function() {
    // module() 可以传入函数或字符串(模块的别名)
    module(function($provide) {
      $provide.value('version', 'overridden'); // 此处覆盖了版本
    });

    inject(function(version) {
      expect(version).toEqual('overridden');
    });
  });
});
```


## 用法

`angular.mock.inject(fns)`

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|fns|`...Function`| 任意数量的需要使用注射器注入的函数。|
