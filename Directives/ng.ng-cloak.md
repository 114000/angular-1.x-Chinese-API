# `ng-cloak`
- Angular@1.4.7
- `ng` 模块中的指令

The ngCloak directive is used to prevent the Angular html template from being briefly displayed by the browser in its raw (uncompiled) form while your application is loading. Use this directive to avoid the undesirable flicker effect caused by the html template display.

The directive can be applied to the <body> element, but the preferred usage is to apply multiple ngCloak directives to small portions of the page to permit progressive rendering of the browser view.

ngCloak works in cooperation with the following css rule embedded within angular.js and angular.min.js. For CSP mode please add angular-csp.css to your html file (see ngCsp).

``` css
[ng\:cloak], [ng-cloak], [data-ng-cloak], [x-ng-cloak], .ng-cloak, .x-ng-cloak {
  display: none !important;
}
```

When this css rule is loaded by the browser, all html elements (including their children) that are tagged with the ngCloak directive are hidden. When Angular encounters this directive during the compilation of the template it deletes the ngCloak element attribute, making the compiled element visible.

For the best result, the angular.js script must be loaded in the head section of the html document; alternatively, the css rule above must be included in the external stylesheet of the application.

## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<ANY>
</ANY>
```

用作 CSS 类：

``` css
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
