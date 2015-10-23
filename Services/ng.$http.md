# $http

- $httpProvider
- ng 模块中的服务

`$http` 服务是一个Angular内的有助于通过浏览器的 `XMLHttpRequest` 对象或 `JSONP` 的方式连接远程
HTTP 服务器核心服务。

对使用 `$http` 服务的单元测试的应用，请参见 [`$httpBackend`](https://code.angularjs.org/1.4.3/docs/api/ngMock/service/$httpBackend) 测试

对于更高级别的抽象化，请查看[`$resource`](https://code.angularjs.org/1.4.3/docs/api/ngResource/service/$resource)服务

`$http`的 API 是基于 `$q` 服务导出的 `deferred/promise` API.

虽然简单的用法格式与此没有多大关系，但是对于更为高级的用法它使非常重要的，它所提供的这些 API
和保障会使你更为熟悉你自己写的代码。

---

## 一般用法

`$http` 服务是一个函数，它需要单一的一个参数 - 一个配置对象 - 它被用来生成一个 HTTP 请求并返
回一个 拥有两个具体方法的 `promise`：`success` 和 `error`

``` javascript

// 简单的 GET 请求示例:
$http.get('/someUrl').
  success(function(data, status, headers, config) {
    // 当响应成功了
    // 这个回调函数会被异步调用
  }).
  error(function(data, status, headers, config) {
    // 如果发生错误或则相应返回了错误的状态吗
    // 这个回调函数会被异步调用
  });

```

``` javascript

// 简单的 POST 请求示例 (passing data) :
$http.post('/someUrl', {msg:'hello word!'}).
  success(function(data, status, headers, config) {
    // 当响应成功了
    // 这个回调函数会被异步调用
  }).
  error(function(data, status, headers, config) {
    // 如果发生错误或则相应返回了错误的状态吗
    // 这个回调函数会被异步调用
  });

```

由于调用 `$http` 函数的返回值是一个 `promise`，因此你可以使用 `then`方法注册回调函数，并且这些回调函数
将会接收唯一的参数 - 请求返回的响应对象。请查看下方关于 API 和格式信息的详细信息。

响应状态码在 200 - 299 之间会被认定为成功的状态，并在成功的回调函数被调用时将返回的结果传递给它。注意，
如果响应是一个重定向，`XMLHttpRequest` 依旧会执行，意思是 error 回调不会被响应调用。

---

## 使用 `$http` 编写单元测试

``` javascript

$httpBackend.expectGET(...);
$http.get(...);
$httpBackend.flush();

```

---

## 简写方法

简写方法也可以使用，所有的简写方法都需要传入 URL，并且需要为 `POST/PUT` 请求传入请求数据。

``` javascript

$http.get('/someUrl').success(successCallback);
$http.post('/someUrl', data).success(successCallback);

```

简写方法的完整列表:

  - `$http.get`
  - `$http.head`
  - `$http.post`
  - `$http.put`
  - `$http.delete`
  - `$http.jsonp`
  - `$http.patch`

---

## 设置 HTTP 头部信息

`$http` 服务会自动为所有的请求添加确定的 HTTP 头。这些默认的设置可以通过 `$httpProvider.defaults.headers`
组态对象来完全重置。包含着一下的组态：

- `$httpProvider.defaults.headers.common` (所有请求公用的头部):
  * `Accept: application/json, text/plain, * / *`
- `$httpProvider.defaults.headers.post`: (POST 请求的默认头部)
  * `Content-Type: application/json`
- `$httpProvider.defaults.headers.put` (PUT 请求的头部)
  * `Content-Type: application/json`


添加或者覆盖这些默认配置，只需从组态对象中添加或者删除一个属性即可。为除了 POST 或 PUT 的 HTTP 方法添加一个头部，
则只需添加一个以这个方法名字的小写形式作为 key 的新的对象即可。例如 `$httpProvider.defaults.headers.get = { 'My-Header' : 'value' }`.

这些默认的设置在运行时仍然可以通过 `$http.default` 对象实现改变，例如：

``` javascript
module.run(function($http) {
  $http.defaults.headers.common.Authorization = 'Basic YmVlcDpib29w'
})
```

此外，你可以在 `$http(config)` 调用的时候在传入 `config` 对象的时候提供一个 `header` 属性，
以此来覆盖默认配置而并不用全局地改变他们。

为了明确的删除在每一个请求的通过 `$httpProvider.default.headers` 自动添加的头部，使用 `headers`
的属性，设置需要的属性为 `undefined`。例如：

``` javascript
var req = {
 method: 'POST',
 url: 'http://example.com',
 headers: {
   'Content-Type': undefined
 },
 data: { test: 'test' }
}

$http(req).success(function(){...}).error(function(){...});

```

---

## 改造请求和响应

请求和相应均可以被改造函数改造：`transformRequest` 和 `transformResponse`。这两个属性可以
是返回改造值一个单独的函数 (`function(data, headersGetter, status)`)，或者是一个含有改造
函数的一个数组，它可以允许你向其中 `push` 或 `unshift` 一个新改造函数到你的改造链中。


### 默认的改造


`$httpProvider` 提供器和 `$http` 服务导出了 `defaults.transformRequest` 和 `defaults.transformResponse`
属性。如果以个请求没有一共它自己的改造那么以上的实行将被应用。

你可以通过向数组添加或替换改造函数来修改这些属性以扩大或者取代默认的改造。

Angular 提供了以下的默认改造。

请求改造 (`$httpProvider.defaults.transformRequest` 和 `$http.defaults.transformRequest`):

  - 如果 `request` 的组态对象的 data 属性中包含对象，那么这个对象将被 JSON 格式化。

响应改造 (`$httpProvider.defaults.transformResponse` and `$http.defaults.transformResponse`):

  - 如果检测到 `XSRF` 前缀，则过滤掉它(请看下面的 Security Considerations 部分)。
  - 如果检测到 `JSON response` ，则使用 `JSON parser` 将其反序列化。

### 覆盖每一个请求的默认改造

如果你仅仅想覆盖某一个单独的 `request/response` 的改造，那么将带有 `transformRequest` `transformResponse`
的对象传入 `$http` 中即可。


Note that if you provide these properties on the config object the default transformations will be overwritten. If you wish to augment the default transformations then you must include them in your local transformation array.

The following code demonstrates adding a new response transformation to be run after the default response transformations have been run.


``` javascript

function appendTransform(defaults, transform) {

  // We can't guarantee that the default transformation is an array
  defaults = angular.isArray(defaults) ? defaults : [defaults];

  // Append the new transformation to the defaults
  return defaults.concat(transform);
}

$http({
  url: '...',
  method: 'GET',
  transformResponse: appendTransform($http.defaults.transformResponse, function(value) {
    return doTransform(value);
  })
});

```


---

## Caching


To enable caching, set the request configuration `cache` property to `true` (to use default cache) or to a custom cache object (built with `$cacheFactory`). When the cache is enabled, $http stores the response from the server in the specified cache. The next time the same request is made, the response is served from the cache without sending a request to the server.

Note that even if the response is served from cache, delivery of the data is asynchronous in the same way that real requests are.

If there are multiple GET requests for the same URL that should be cached using the same cache, but the cache is not populated yet, only one request to the server will be made and the remaining requests will be fulfilled using the response from the first request.

You can change the default cache to a new object (built with `$cacheFactory`) by updating the `$http.defaults.cache` property. All requests who set their cache property to true will now use this cache object.

If you set the default cache to false then only requests that specify their own custom cache object will be cached.


---

## Interceptors


Before you start creating interceptors, be sure to understand the $q and deferred/promise APIs.

For purposes of global error handling, authentication, or any kind of synchronous or asynchronous pre-processing of request or postprocessing of responses, it is desirable to be able to intercept requests before they are handed to the server and responses before they are handed over to the application code that initiated these requests. The interceptors leverage the promise APIs to fulfill this need for both synchronous and asynchronous pre-processing.

The interceptors are service factories that are registered with the `$httpProvider` by adding them to the `$httpProvider.interceptors` array. The factory is called and injected with dependencies (if specified) and returns the interceptor.

There are two kinds of interceptors (and two kinds of rejection interceptors):

- `request`: interceptors get called with a http `config` object. The function is free to modify the `config` object or create a new one. The function needs to return the `config` object directly, or a promise containing the `config` or a new `config` object.
- `requestError`: interceptor gets called when a previous interceptor threw an error or resolved with a rejection.
- `response`: interceptors get called with http `response` object. The function is free to modify the `response` object or create a new one. The function needs to return the `response` object directly, or as a promise containing the `response` or a new `response` object.
- `responseError`: interceptor gets called when a previous interceptor threw an error or resolved with a rejection.


``` javascript
// register the interceptor as a service
$provide.factory('myHttpInterceptor', function($q, dependency1, dependency2) {
  return {
    // optional method
    'request': function(config) {
      // do something on success
      return config;
    },

    // optional method
   'requestError': function(rejection) {
      // do something on error
      if (canRecover(rejection)) {
        return responseOrNewPromise
      }
      return $q.reject(rejection);
    },



    // optional method
    'response': function(response) {
      // do something on success
      return response;
    },

    // optional method
   'responseError': function(rejection) {
      // do something on error
      if (canRecover(rejection)) {
        return responseOrNewPromise
      }
      return $q.reject(rejection);
    }
  };
});

$httpProvider.interceptors.push('myHttpInterceptor');


// alternatively, register the interceptor via an anonymous factory
$httpProvider.interceptors.push(function($q, dependency1, dependency2) {
  return {
   'request': function(config) {
       // same as above
    },

    'response': function(response) {
       // same as above
    }
  };
});


```

---

## Security Considerations

When designing web applications, consider security threats from:

- JSON vulnerability
- XSRF
Both server and the client must cooperate in order to eliminate these threats. Angular comes pre-configured with strategies that address these issues, but for this to work backend server cooperation is required.

### JSON Vulnerability Protection

A JSON vulnerability allows third party website to turn your JSON resource URL into JSONP request under some conditions. To counter this your server can prefix all JSON requests with following string `")]}',\n"`. Angular will automatically strip the prefix before processing it as JSON.

For example if your server needs to return:

``` javascript

['one','two']

```

which is vulnerable to attack, your server can return:

``` javascript
'……)]}',
['one','two']

```

Angular will strip the prefix, before processing the JSON.

### Cross Site Request Forgery (XSRF) Protection

XSRF is a technique by which an unauthorized site can gain your user's private data. Angular provides a mechanism to counter XSRF. When performing XHR requests, the $http service reads a token from a cookie (by default, XSRF-TOKEN) and sets it as an HTTP header (X-XSRF-TOKEN). Since only JavaScript that runs on your domain could read the cookie, your server can be assured that the XHR came from JavaScript running on your domain. The header will not be set for cross-domain requests.

To take advantage of this, your server needs to set a token in a JavaScript readable session cookie called XSRF-TOKEN on the first HTTP GET request. On subsequent XHR requests the server can verify that the cookie matches X-XSRF-TOKEN HTTP header, and therefore be sure that only JavaScript running on your domain could have sent the request. The token must be unique for each user and must be verifiable by the server (to prevent the JavaScript from making up its own tokens). We recommend that the token is a digest of your site's authentication cookie with a salt for added security.

The name of the headers can be specified using the xsrfHeaderName and xsrfCookieName properties of either $httpProvider.defaults at config-time, $http.defaults at run-time, or the per-request config object.

In order to prevent collisions in environments where multiple Angular apps share the same domain or subdomain, we recommend that each application uses unique cookie name.

---

## 依赖

`$httpBackend`
`$cacheFactory`
`$rootScope`
`$q`
`$injector`

---

## 用法

`$http(config)`

**参数**


| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|config|`object`|

Object describing the request to be made and how it should be processed. 该对象有以下属性:
  - **method** – `{string}` – HTTP 方法 (如. 'GET', 'POST', 等)
  - **url** – `{string}` – 需要请求的资源的绝对或相对 URL
  - **params** – `{Object.<string|Object>}` – 它会被 `paramSerializer`
  序列化字符串或对象的 `map` 形式，并添加到 GET 参数中。
  - **data** – `{string|Object}` – 将被当做请求的数据被发送。
  - **headers** – `{Object}` – 字符串或函数的 `map` 形式 代表 HTTP 头

  Map of strings or functions which return strings representing HTTP headers to send to the server. If the return value of a function is null, the header will not be sent. Functions accept a config object as an argument.
  - **xsrfHeaderName** – `{string}` – HTTP 头部名 Name of HTTP header to populate with the XSRF token.
  - **xsrfCookieName** – `{string}` – Name of cookie containing the XSRF token.
  - **transformRequest** – `{function(data, headersGetter)|Array.<function(data, headersGetter)>}` – transform function or an array of such functions. The transform function takes the http request body and headers and returns its transformed (typically serialized) version. See Overriding the Default Transformations
  - **transformResponse** – `{function(data, headersGetter, status)|Array.<function(data, headersGetter, status)>}` – transform function or an array of such functions. The transform function takes the http response body, headers and status and returns its transformed (typically deserialized) version. See Overriding the Default TransformationjqLiks
  - **paramSerializer** - `{string|function(Object<string,string>):string}` - A function used to prepare the string representation of request parameters (specified as an object). If specified as string, it is interpreted as function registered with the $injector, which means you can create your own serializer by registering it as a service. The default serializer is the $httpParamSerializer; alternatively, you can use the $httpParamSerializerJQLike
  - **cache** – `{boolean|Cache}` – If true, a default $http cache will be used to cache the GET request, otherwise if a cache instance built with $cacheFactory, this cache will be used for caching.
  - **timeout** – `{number|Promise}` – 设置毫秒延时，
  timeout in milliseconds, or promise that should abort the request when resolved.
  - **withCredentials** - `{boolean}` - 是否在 XHR 对象上设置 `withCredentials`。
   请看[使用证书的请求](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS#Requests_with_credentials) 以获取更多信息。
  - **responseType** - `{string}` - 请看 [XMLHttpRequest.responseType](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest#xmlhttprequest-responsetype).
  |

**返回**

`HttpPromise` - Returns a promise object with the standard then method and two http specific methods: success and error. The then method takes two arguments a success and an error callback which will be called with a response object. The success and error methods take a single argument - a function that will be called when the request succeeds or fails respectively. The arguments passed into these functions are destructured representation of the response object passed into the then method. The response object has these properties:

  - **data** – `{string|Object}` – The response body transformed with the transform functions.
  - **status** – `{number}` – HTTP status code of the response.
  - **headers** – `{function([headerName])}` – Header getter function.
  - **config** – `{Object}` – The configuration object that was used to generate the request.
  - **statusText** – `{string}` – HTTP status text of the response.

---

## 方法

### 1.`get(url, [config])`

执行 `GET` 请求的快捷方法。

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|url|`string`| 指定请求目的地的相对或绝对路径。|
|config *（可选）*|`Object`| 可选的配置对象|


**返回**

`HttpPromise` - `promise`

### 2.`delete(url, [config])`

执行 `DELETE` 请求的快捷方法。

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|url|`string`| 指定请求目的地的相对或绝对路径。|
|config *（可选）*|`Object`| 可选的配置对象|

**返回**

`HttpPromise` - `promise`


### 3.`head(url, [config])`

执行 `HEAD` 请求的快捷方法。


**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|url|`string`| 指定请求目的地的相对或绝对路径。|
|config *（可选）*|`Object`| 可选的配置对象|

**返回**

`HttpPromise` - `promise`

### 4.`jsonp(url, [config])`

执行 `JSONP` 请求的快捷方法。

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|url|`string`| 指定请求目的地的相对或绝对路径。回调函数的名字必须是字符串 `JSON_CALLBACK`|
|config *（可选）*|`Object`| 可选的配置对象|

**返回**

`HttpPromise` - `promise`



### 5.`post(url, data, [config])`

执行 `POST` 请求的快捷方法。

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|url|`string`| 指定请求目的地的相对或绝对路径。|
|data|`*`| 请求内容|
|config *（可选）*|`Object`| 可选的配置对象|


**返回**

`HttpPromise` - `promise`


### 6.`put(url, data, [config])`

执行 `PUT` 请求的快捷方法。


**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|url|`string`| 指定请求目的地的相对或绝对路径。|
|data|`*`| 请求内容|
|config *（可选）*|`Object`| 可选的配置对象|


**返回**

`HttpPromise` - `promise`


### 7.`patch(url, data, [config])`

执行 `PATCH` 请求的快捷方法。

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|url|`string`| 指定请求目的地的相对或绝对路径。|
|data|`*`| 请求内容|
|config *（可选）*|`Object`| 可选的配置对象|

**返回**

`HttpPromise` - `promise`


---

## 属性

### 1.`pendingRequests`

`Array.<Object>` - 	配置对象的当前待处理请求的数组，它的主要作用是为了调试。


### 2.`defaults`

`$httpProvider.defaults` 属性的运行时当量. 允许默认头配置, withCredentials,以及请求和响应的变化。

详细请看上面的 "设置 HTTP Headers", "Transforming Requests and Responses"。

---

## 例子

> index.html

``` html

<div ng-controller="FetchController">
  <select ng-model="method" aria-label="Request method">
    <option>GET</option>
    <option>JSONP</option>
  </select>
  <input type="text" ng-model="url" size="80" aria-label="URL" />
  <button id="fetchbtn" ng-click="fetch()">fetch</button><br>
  <button id="samplegetbtn" ng-click="updateModel('GET', 'http-hello.html')">Sample GET</button>
  <button id="samplejsonpbtn"
    ng-click="updateModel('JSONP',
                  'https://angularjs.org/greet.php?callback=JSON_CALLBACK&name=Super%20Hero')">
    Sample JSONP
  </button>
  <button id="invalidjsonpbtn"
    ng-click="updateModel('JSONP', 'https://angularjs.org/doesntexist&callback=JSON_CALLBACK')">
      Invalid JSONP
    </button>
  <pre>http status code: {{status}}</pre>
  <pre>http response data: {{data}}</pre>
</div>

```

> script.js

``` javascript

angular.module('httpExample', [])
.controller('FetchController', ['$scope', '$http', '$templateCache',
  function($scope, $http, $templateCache) {
    $scope.method = 'GET';
    $scope.url = 'http-hello.html';

    $scope.fetch = function() {
      $scope.code = null;
      $scope.response = null;

      $http({method: $scope.method, url: $scope.url, cache: $templateCache}).
        success(function(data, status) {
          $scope.status = status;
          $scope.data = data;
        }).
        error(function(data, status) {
          $scope.data = data || "Request failed";
          $scope.status = status;
      });
    };

    $scope.updateModel = function(method, url) {
      $scope.method = method;
      $scope.url = url;
    };
  }]);

```

> http-hello.html

``` html

Hello, $http!

```

> protactor.js

``` javascript

var status = element(by.binding('status'));
 var data = element(by.binding('data'));
 var fetchBtn = element(by.id('fetchbtn'));
 var sampleGetBtn = element(by.id('samplegetbtn'));
 var sampleJsonpBtn = element(by.id('samplejsonpbtn'));
 var invalidJsonpBtn = element(by.id('invalidjsonpbtn'));

 it('should make an xhr GET request', function() {
   sampleGetBtn.click();
   fetchBtn.click();
   expect(status.getText()).toMatch('200');
   expect(data.getText()).toMatch(/Hello, \$http!/);
 });

// Commented out due to flakes. See https://github.com/angular/angular.js/issues/9185
// it('should make a JSONP request to angularjs.org', function() {
//   sampleJsonpBtn.click();
//   fetchBtn.click();
//   expect(status.getText()).toMatch('200');
//   expect(data.getText()).toMatch(/Super Hero!/);
// });

 it('should make JSONP request to invalid URL and invoke the error handler',
     function() {
   invalidJsonpBtn.click();
   fetchBtn.click();
   expect(status.getText()).toMatch('0');
   expect(data.getText()).toMatch('Request failed');
 });

```
