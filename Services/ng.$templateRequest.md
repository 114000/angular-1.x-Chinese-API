# Angular_1.4.3 API 服务篇 $templateRequest

- [ng](https://docs.angularjs.org/api/ng)模块中的服务

`$templateRequest` 服务

`$templateRequest` 服务使用 `$http` 服务下载模板，并在之后进行安全检测。如果成功，
则内容会被存储到 `$templateCache` 中。如果HTTP请求失败或者该请求的响应数据为空，
会抛出一个编译（`$compile`）错误（这个行为可以通过将函数的第二个参数设置为 `true` 来阻止）。

需要注意的是，`$templateCache` 的内容是可以信任的，因此当 `tpl` 是以字符串形式出现的时候，
或者通过 `$templateCache` 作为入口时，是可以不必去调用 `$sce.getTrustedUrl(tpl)` 来验证的。

## 用法

`$templateRequest(tpl, [ignoreRequestError]);`

##### *参数*

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|tpl|`string` `TrustedResourceUrl`| HTTP 请求的模板路径(URL) |
|ignoreRequestError *(可选)*|`boolean`|是否忽略请求失败或者模板为空的情况|

##### *返回*

`promise`	- 一个表示通过给定的路径请求得到的 HTTP 相应数据的 `promise`

## 属性

`totalPendingRequests` - `number` - 所有被需求下载模板的总数
