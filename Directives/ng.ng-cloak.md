# `ng-cloak`
- Angular@1.4.7
- `ng` 模块中的指令

`ngCloak` 指令用来在你的应用加载时防止浏览器短暂地显示出 Angular html 模板的原始（未编译）的状态。
使用这个指令避免由 html 模板显示而引起的不良的闪烁效果。

这个指令虽然可以应用到 `<body>` 元素上，但是为了使浏览器视图能够渐进地渲染，首选还是在页面上的多个小部分来使用它。

`ngCloak` 会同下面的 css 规则被一同嵌入到 angular.js 和 angular.min.js 中。使用 CSP
模式时请在你的 html 文件中添加 angular-csp.css （请看 `ngCsp`）.

``` css
[ng\:cloak], [ng-cloak], [data-ng-cloak], [x-ng-cloak], .ng-cloak, .x-ng-cloak {
  display: none !important;
}
```

在这条 css 被浏览器载入后，所有带有  `ngCloak` 的 html 元素（包括他们的子类）会被标记为隐藏。
当 Angular 在模板编译时遇见这个指令时，Angular 会删除元素上的 `ngCloak` 属性，使编译后的元素可见。

为了达到最好的效果，angular.js 脚本必须在 html 文档的头部（head）被加载进来，
或者作为替代的方法，上面的 css 规则必须被应用外部的样式表包含了。


## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<ANY>
</ANY>
```

用作 CSS 类：

``` html
<ANY class=""> ... </ANY>
```


## 例子

> index.html

``` html
<div id="template1" ng-cloak>{{ 'hello' }}</div>
<div id="template2" class="ng-cloak">{{ 'world' }}</div>
```

> protractor.js

``` javascript
it('should remove the template directive and css class', function() {
  expect($('#template1').getAttribute('ng-cloak')).
    toBeNull();
  expect($('#template2').getAttribute('ng-cloak')).
    toBeNull();
});
```
