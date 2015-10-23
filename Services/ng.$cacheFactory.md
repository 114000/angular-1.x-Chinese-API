# Angular_1.4.3 API 服务篇 $cacheFactory

- [ng](https://code.angularjs.org/1.4.3/docs/api/ng)模块中的服务

以工厂模式构造cache对象，并且使它们可以被访问。

> javascript

```javascript
var cache = $cacheFactory('cacheId');

expect($cacheFactory.get('cacheId')).toBe(cache);
expect($cacheFactory.get('noSuchCacheId')).not.toBeDefined();

cache.put("key", "value");
cache.put("another key", "another value");

// 创建时我们没有指定配置项
expect(cache.info()).toEqual({id: 'cacheId', size: 2});

```

## 用法

`$cacheFactory(cacheId, [配置项]);`

| 参数 | 形式 | 具体 |
|:-----|:----:|:-----|
|`cacheId`| `string`|新缓存的名称或ID。|
|配置项 *(选填)*| `object`|配置对象会指定缓存的行为。 性能:<br><br> *{number=} 容量 — 将缓存转化成LRU缓存。*|




##### *返回*
|---|---|
|:-----|:----|
|`object`|新创建的缓存对象有以下的配置方法: <br><br> - `{object}` `info()` — 返回 id, 大小, 和缓存的配置。<br><br>-`*`,`put({string} key, {*} value)` — 向缓存中插入以个新的键值对并将它返回。<br><br> - `*`,`get({string} key)` — 返回与`key`对应的`value`值，如果未命中则返回`undefined`。 <br><br> - `{void}` `remove({string} key)`  — 从缓存中删除一个键值对 <br><br> - `{void}` `removeAll()` — 删除所有缓存中的数据 <br><br> - `{void}` `destroy()` — 删除从`$cacheFactory`引用的这个缓存.


## 方法
### 1.`info();` 获取所有被创建的缓存的信息


##### *返回*
`Object` 返回一个关于`cacheId`的键值

### 2.`get(cacheId);` 如果与cacheId相对应的缓存对象被创建，则获取它

##### *参数*
| 参数 | 形式 | 具体 |
|:-----|:----:|:-----|
|`cacheId`|`string`| 一个可以通过的缓存名字或ID


##### *返回*

Returns
`Object` - 通过 `cacheId` 确认的缓存对象，或是确认失败的 `undefined`

## 例子

> html

```html
<div ng-controller="CacheController">

  <input ng-model="newCacheKey" placeholder="Key">
  <input ng-model="newCacheValue" placeholder="Value">

  <button ng-click="put(newCacheKey,newCacheValue)">Cache</button>

  <p ng-if="keys.length">Cached Values</p>
  <div ng-repeat="key in keys">
    <span ng-bind="key"></span>
    <span>: </span>
    <b ng-bind="cache.get(key)"></b>
  </div>

  <p>Cache Info</p>
  <div ng-repeat="(key, value) in cache.info()">
    <span ng-bind="key"></span>
    <span>: </span>
    <b ng-bind="value"></b>
  </div>
</div>
```

> javascript

```javascript
angular.module('cacheExampleApp', []).
controller('CacheController', ['$scope', '$cacheFactory', function($scope, $cacheFactory) {
  $scope.keys = [];
  $scope.cache = $cacheFactory('cacheId');
  $scope.put = function(key, value) {
    if ($scope.cache.get(key) === undefined) {
      $scope.keys.push(key);
    }
    $scope.cache.put(key, value === undefined ? null : value);
  };
}]);
```

>css

```css
p {
  margin: 10px 0 3px;
}
```
