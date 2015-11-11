# `<script>`
- Angular@1.4.7
- `ng` 模块中的指令

将 `<script>` 里的内容载入到 `$templateCache`，以便 `ngInclude`，`ngView`
或其他指令使用这个模板，`<script>` 的 `type` 属性必须设置成 `text/ng-template`，
并且还要通过 `id` 属性为模板设置一个缓存名称，然后这个名称就可以用在指令的 `templateUrl` 属性中了。

## 指令信息

这个指令的执行优先级为0级

## 用法

像个元素（标签）一样使用

> html

``` html
<script
  type="string"
  id="string">
...
</script>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|type|`string`| 必须设置为 'text/ng-template'.|
|id|`string`| 模板的缓存名称|


## 例子

> index.html

``` html
<script type="text/ng-template" id="/tpl.html">
  Content of the template.
</script>

<a ng-click="currentTpl='/tpl.html'" id="tpl-link">Load inlined template</a>
<div id="tpl-content" ng-include src="currentTpl"></div>
```

> protractor.js

``` javascript
it('should load template defined inside script tag', function() {
  element(by.css('#tpl-link')).click();
  expect(element(by.css('#tpl-content')).getText()).toMatch(/Content of the template/);
});
```
