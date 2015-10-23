---
layout: post
title:  "Angular_1.4.3 API 服务篇 $q"
desc: "$q的灵感源自Kris Kowal的`Q`，它实现是一个`promises`/`deferred`对象。这是一个能帮助你异步地运行函数，并且在他们仍在进程中的时候使用他们的返回值（或者捕捉异常）的服务"
categories: jekyll update
tags:
- API
- AngularJS
---

# Angular_1.4.3 API 服务篇 $q

- ng模块下的服务 [`$q`官方文档](https://code.angularjs.org/1.4.3/docs/api/ng/service/$q)

> 这是一个能帮助你异步地运行函数，并且在他们仍在进程中的时候使用他们的返回值（或者捕捉异常）的服务

`$q` 的灵感源自[Kris Kowal的`Q`](https://github.com/kriskowal/q)，它实现是一个`promises`/`deferred`对象。

`$q` 可以以下面两种形式来使用:

  + 类似于Kris Kowal的 `Q` 或者jQuery实现的 `Deferred`
  + 某种程度上类似于ES6规范中的 `promises`

### $q 的构造函数

`$q` 可以作为一个构造函数，并可以像流式ES6风格的 `promise` 一样使用。它需要一个解析器函数作为第一个参数。这与源自ES6 Harmony 的原生Promise的实现方式是十分相似的。详情见 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise).
同样也支持构造风格的编程方式，但是目前还没有完全支持ES6 Harmony实现的Promise内的所有方法。

可以被这样使用：

``` javascript

// 此例中，我们假设变量`$ q`和`okToGreet`已经存在于当前作用域
// 他们可以通过依赖注入和传递参数来获取

function asyncGreet(name) {
  // 执行一些异步操作
  // 适当的时候解析(resolve)或者阻止(reject) Promise.

  return $q(function(resolve, reject) {

    setTimeout(function() {

      if (okToGreet(name)) {
        resolve('Hello, ' + name + '!');
      } else {
        reject('Greeting ' + name + ' is not allowed.');
      }

    }, 1000);
  });
}

var promise = asyncGreet('Robin Hood');

promise.then(function(greeting) {

  alert('Success: ' + greeting);

}, function(reason) {

  alert('Failed: ' + reason);

});

```


贴士：进度/通知的回调目前还未在ES6风格里得到支持。

不过，更多的传统的CommonJS的风格依然可用，并且记录如下。
[The CommonJS Promise 指南](http://wiki.commonjs.org/wiki/Promises) 描述 promise 是一个为某个代表着异步操作结果的对象提供互动的接口，而且它的执行也并不依赖于某个特定的时间节点.

从用错误处理解决问题的角度来看 `deferred` 和 `promise` 的一些API是异步编程而 `try`、`catch` 和 `throw` 关键字是同步编程.

``` javascript
// 此例中，我们假设变量`$ q`和`okToGreet`已经存在于当前作用域
// 他们可以通过依赖注入和传递参数来获取

function asyncGreet(name) {

  var deferred = $q.defer();

  setTimeout(function() {

    deferred.notify('About to greet ' + name + '.');

    if (okToGreet(name)) {
      deferred.resolve('Hello, ' + name + '!');
    } else {
      deferred.reject('Greeting ' + name + ' is not allowed.');
    }

  }, 1000);

  return deferred.promise;
}

var promise = asyncGreet('Robin Hood');

promise.then(function(greeting) {

  alert('Success: ' + greeting);

}, function(reason) {

  alert('Failed: ' + reason);

}, function(update) {

  alert('Got notification: ' + update);

});
```

开始的时候这样做的好处不是十分明显的，我们为什么要增加额外的复杂度？看起来十分麻烦。这样做的好处是源自 `promise` 和 `deferred` API文档确立的，详述请看[链接](https://github.com/kriskowal/uncommonjs/blob/master/promises/specification.md).
此外，`promise` API 允许构造是因为使用传统的回调形式十分困难。更多相关请参考[Q 文档](https://github.com/kriskowal/q) 特别是关于 `promise` 的串行和并行连接的部分.

### Deferred API

一个新的 `deferred` 实例可以通过调用 `$q.defer()` 来创建。

这样暴露出了相关 `promise` 实例，而且API可以用来广播成功或失败的完成，以及任务的状态。

##### *方法*

* `resolve(value)` – 用 `value` 来解析返回的 `promise`. 如果 `value` 是通过 `$q.reject` 返回的拒绝构造，那么这个 `promise` 也将被拒绝.
* `reject(reason)` – 以 `reason` 拒绝返回的 `promise`. 这等同于由 `$q.reject` 拒绝构造来解析它。
* `notify(value)` - 在`promise`执行的状态下提供更新. 因此在`promise`尚未被解析或是拒绝之前会调用很多次.

##### *属性*

* `promise – {Promise}` – 与 `deferred` 相关联的 `promise` 对象.

### Promise API

当一个新的 `deferred` 实例被创建出来，并且可以通过使用 `deferred.promise` 检索出来，那么实际上一个新的 `promise` 实例也被创建出来了.
`promise` 对象的作用是，允许相关部分获得当 `deferred` 任务完成时的结果。

##### *方法*

1. `then(successCallback, errorCallback, notifyCallback)` – 不管`promise` 已经被解析（拒绝）还是将要发生这些。一但结果为可知的，`then`都会调用一个成功或者失败的异步回调函数. 这些回调函数被调用是只会被传递一个单独的参数: 成功结果或是失败被拒绝的原因。此外Additionally, the 在 `promise` 被解析（拒绝）之前，为了提供了一个进度指示 通知回调（`notify`）可以被调用0到若干次。
这个方法返回了一个根据成功 （或失败） 回调函数返回结果来解析的新的 `promise`
（除非返回的结果是一个`promise`, 而这个`promise`在这种情况下已经由存在于[`promise`链](http://www.html5rocks.com/en/tutorials/es6/promises/#toc-promises-queues)的 `promise` 的 `value` 解析）。 依然会通过 `notifyCallback` 方法的返回值发布通知。`promise` 不能由这个 `notifyCallback` 方法解析或者拒绝。

2. `catch(errorCallback)` - `promise.then(null, errorCallback)` 的简写

3. `finally(callback, notifyCallback)` – 虽然允许你观测一个 `promise` 的实现(拒绝)，但并不会修改最终值。 这对于发布资源或者为那些需要做处理的被解析(拒绝)的 `promise` 做一些清理工作十分有用。

### 链接 Promise

由于调用`promise`下的`then`方法会返回一个源 `promise`, 这使得创建一条 `promise` 链变得很容易:

``` javascript
promiseB = promiseA.then(function(result) {
  return result + 1;
});

// 当promiseA被解析之后primiseB会紧接着被解析
// 其值将是promiseA的加1的结果

```
这有可能会创建一个任意长度的链,因为一个 `promise` 可以被另一个 `promise`解析(而这个`promise`将会被近一步的推迟解析)，也有可能在这条链的任意节点暂停/推迟解析。这将 实现像 `$http` 的相应拦截器那样强有力的API成为可能.

### Kris Kowal's Q 与 $q 之间的区别

以下是他们之间最主要的区别:

* `$q` 是通过 `$rootScope.Scope` 集成的，在Angular中作用域模型观测机制会更快的在你的 `model` 之间传播解析或拒绝，这避免不必要的浏览器重绘导致的UI闪烁.

* 尽管 `Q` 比 `$q` 有更多的特性，但是这也会带来增加更多字节的问题。虽然 `$q` 是微型的，但是它包含了大多数异步任务需要的所有重要功能.

测试部分

``` javascript
it('should simulate promise', inject(function($q, $rootScope) {

  var deferred = $q.defer();
  var promise = deferred.promise;
  var resolvedValue;

  promise.then(function(value) {

    resolvedValue = value;
  });

  expect(resolvedValue).toBeUndefined();

  // 模拟解析 promise

  deferred.resolve(123);

  // 注意！ then函数没有被同步调用.
  // 这是因为无论 promise 是被同步或是异步调用的，
  // 我们都希望它的 API 总是异步的
  expect(resolvedValue).toBeUndefined();

  // 使用$apply向 then方法传递 promise 解析.
  $rootScope.$apply();
  expect(resolvedValue).toEqual(123);

}));
```

---
## 依赖
`$rootScope`

---
## 用法

`$q(resolver);`

##### *参数*

| 参数 | 形式 | 具体 |
|:-----|:----:|:-----|
|resolver| `function(function, function)`|函数是为了响应新创建 `promise`, 函数的第一个参数是用来解析 `promise` 的函数,函数的第二个参数是用来拒绝 `promise` 的函数.|


##### *返回*

`Promise` - 是新创建出的 `promise`.

### 方法

#### 1.`defer();`

创建一个新的 `deferred` 对象来表示一个将要在未来完成的任务.

##### *返回*

`Deferred` - 是一个 `deferred` 的新实例.


#### 2.`reject(reason);`

创建一个由被指定 `reason` 的拒绝而解析的新的 `promise`.它的API将被放到 `promise` 链的前面，所以如果你正在处理这个 `promise` 链的最后一个 `promise`,你并不需要为此感到担心.

当将 `deferreds/promises` 和更为熟悉的 `try/catch/throw ` 行为相比较时, 把 `reject` 当做Javascript里的 `throw` 关键字. 同样地如果你“捕获 `catch` ” `promise` 失败回调返回的error并且你想将此 `error` 掷到由当前 `promise` 派生出的 `promise` 中，你将不得通过借由 `reject` 返回的一个拒绝构造来“重抛”这个 `error`.

``` javascript
promiseB = promiseA.then(function(result) {
  // 成功： 执行函数体内部的操作
  //       并用既有或新的结果解析 promiseB
  return result;

}, function(reason) {
  // 错误： 尽可能处理错误,
  //       并用 newPromiseOrValue 解析 promiseB,
  //       否则向promiseB转发拒绝

  if (canHandle(reason)) {
   // 处理错误并恢复
   return newPromiseOrValue;

  }
  return $q.reject(reason);
});

```

##### *参数*

| 参数 | 形式 | 具体 |
|:-----|:----:|:-----|
|reason| ` * `|	常量, 信息, 例外或一个代表拒绝原因的对象.|

##### *返回*

`Promise` 返回一个由`reason`拒绝的 `promise`.

#### 4.`when(value);`

把一个 `value` 或者一个(第三方)可以使用 `then` 的 `promise` 包装到一个 `$q promise`.这样做的好处是当你再处理一个对象时，你不用再考虑它是不是一个  `promise`,也不必考虑某个外源的 `promise` 是否是值得信任的.

##### *参数*

| 参数 | 形式 | 具体                |
|:-----|:----:|:--------------------|
|value | `*`  | value 或一个 promise|

##### *返回*

`Promise` - 返回由一个由验证过的 `value` 或一个 `promise` 包裹成的新的 `promise`.

#### 5.`resolve(value);`

[when](https://code.angularjs.org/1.4.3/docs/api/ng/service/$q#when)的别名，与ES6保持一致.

##### *参数*

| 参数 | 形式 | 具体 |
|:-----|:----:|:-----|
|value	|`*`	|  value 或一个 promise|

##### *返回*

`Promise` 返回由一个由验证过的 `value` 或一个 `promise` 包裹成的新的 `promise`.

#### 6.`all(promises);`

将多个`promise`合并成一个 `promise`,它的解析将会在所有输入的`promise`解析之后执行.

##### *参数*
| 参数 | 形式 | 具体 |
|:-----|:----:|:-----|
|promises|	`Array.<Promise>` `Object.<Promise>`	|  一个数组或者 `promises` 的哈希值.

##### *返回*

`Promise` 返回一个单独的 `promise`，它将被一组数组/哈希 `value` 解析。在这个单独的 `promise` 中每一个 `value` 都会对应到 `promise` 数组/哈希的索引中. 如果参数中的任何一个 `promise` 被拒绝，这将使由 `all` 方法返回的 `promise` 被同样的理由（上面的拒绝）拒绝掉.



本文由作者原创，翻译内容仍有欠佳之处，请大家多多指正。via  [_西瓜橘子葡萄冰](http://weibo.com/1975910825/profile?rightmod=1&wvr=6&mod=personinfo)
