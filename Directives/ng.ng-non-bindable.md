# `ng-non-bindable`
- Angular@1.4.7
- `ng` 模块中的指令

`ngNonBindable` 指令告知 Angular 不要在当前 DOM 元素上编译或绑定内容。这个指令用于，
如果元素包含了类似于 Angular 的指令或者绑定的内容时，让 Anuglar忽略的它。
可能是这种情况，你想在你的网站中展示代码片段。

## 指令信息

这个指令的执行优先级为1000级

## 用法

用作属性：

``` html
<ANY>
...
</ANY>
```

用作 CSS 类：

``` html
<ANY class=""> ... </ANY>
```

## 例子

在这个例子中有两个部分使用了简单的插值绑定（`{{表达式}}`），但是其中一个被 `ngNonBindable`
而没有生效。

> index.html

``` html
<div>Normal: {{1 + 2}}</div>
<div ng-non-bindable>Ignored: {{1 + 2}}</div>
```

> protractor.js

``` javascript
it('should check ng-non-bindable', function() {
  expect(element(by.binding('1 + 2')).getText()).toContain('3');
  expect(element.all(by.css('div')).last().getText()).toMatch(/1 \+ 2/);
});
```
