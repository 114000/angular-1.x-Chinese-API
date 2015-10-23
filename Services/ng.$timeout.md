# Angular_1.4.3 API 服务篇 $timeout
- `ng`模块中的服务

`window.setTimeout` 的Angular封装，这个 `fn` 函数被封装成了一个 `try`/`catch` 块并且授 [`$exceptionHandler`](https://docs.angularjs.org/api/ng/service/$exceptionHandler) 服务以任何例外。

调用 `$timeout` 的返回值是一个 `promise`, 这个 `promise` 将会在延时已经结束时被解析，
超时函数（如果有的话）被执行了。

退出一个超时请求，调用 `$timeout.cancel(promise)`。如果在测试中你可以使用 `$timeout.flush()`
来同步刷新 `deferred`函数的队列。而如果你仅仅想要得到一个在一个指定延时时间之后会被解析的 `promise`，
你可以仅仅调用 `$timeout` 而不传入 `fn` 函数。

## 用法

`$timeout([fn], [delay], [invokeApply], [Pass]);`

##### *参数*


| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|fn*(可选)* |`function()=`|已经将在延时之后被执行的函数。|
|delay*(可选)*|`number` |以毫秒计的延时时间。*(默认值: 0)*|
|invokeApply*(可选)*|`boolean` |如果设置为 `false`则跳过模型的脏值检测，否则将在 `fn` 内调用 `$apply` 块。*(默认值: true)*|
|Pass*(可选)*|`*`| 函数执行的附加参数 |

##### *返回*
`Promise` - `promise` 将会在超时达成后被解析，它的值会被 `fn` 的返回值解析。

## 方法

`cancel([promise]);` - 取消一个与 `promise` 相关联的任务。这个结果会导致，`promise`会被拒绝解析。

##### *参数*

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|promise *(可选)*|`promise`|`$timeout` 函数返回的 `promise`|



##### *返回*

`boolean` - 如果任务没有被执行就被成功取消了，则会返回 `true`。
