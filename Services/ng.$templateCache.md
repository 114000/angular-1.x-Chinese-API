# Angular_1.4.3 API 服务篇 $templateCache

- [ng](https://docs.angularjs.org/api/ng)模块中的服务

在模板第一次被引用时,它会被载入到模板缓存中以便快速的检索。你可以直接使用一个script标签来添加一个
模板，或者直接通过 `$templateCache` 服务来达到相同的效果。


通过 script 标签添加:

> html

``` html

<script type="text/ng-template" id="templateId.html">
  <p>This is the content of the template</p>
</script>

```
**注意**：script 标签包含的模板并不需要 `document` 的头部，但它必须是 `$rootElement` 的
后代(IE，带有 `ng-app` 属性的元素)，否则这个模板将被忽略掉。

通过`$templateCache` 服务添加:

> javascript

``` javascript
var myApp = angular.module('myApp', []);
myApp.run(function($templateCache) {
  $templateCache.put('templateId.html', 'This is the content of the template');
});
```
在检索模板之后，你就可以在你的 HTML 里轻松的使用了。

> html

``` html
<div ng-include=" 'templateId.html' "></div>
```


或者通过 Javascript 得到它 :

> javascript

``` javascript
$templateCache.get('templateId.html')
```

请看 [`$cacheFactory`](https://docs.angularjs.org/api/ng/service/$cacheFactory).

---
