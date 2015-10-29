# `ng-disabled`
- Angular@1.4.7
- `ng` 模块中的指令

如果 `ngDisabled` 内部的表达式解析的结果为真值，则这个指令会在元素上设置失效（`disabled`）的属性。

由于不能在 `disabled` 属性上使用插值，所以我们需要这样一个特殊的指令。下面的例子会使 chrome/Firefox
中的按钮失效，但是在老版本的 IE 中不会生效。

``` HTML
<!-- See below for an example of ng-disabled being used correctly -->
<div ng-init="isDisabled = false">
 <button disabled="{{isDisabled}}">Disabled</button>
</div>
```

这是因为 HTML 规范不需要浏览器去保存像 disabled 这样的属性的布尔值（这些属性存在则意味着生效（true），
这些属性不存在则意味着无效（false））。如果我们将 Angular 的插值表达式放入到这样一个属性中，
那么当浏览器移除这个属性的时候我们就是遗失绑定在上面的信息。

## 指令信息

这个指令的执行优先级为100级 （译：终于有不是零的了 `o(*≧▽≦)ツ` ）

## 用法

用作属性：

``` html
<INPUT ng-disabled="expression">
...
</INPUT>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngDisabled|`expression`| 如果表达式的值为真，则将会在元素上添加 `disabled` 属性。|

## 例子

> index.html

``` html
<label>点击我切换: <input type="checkbox" ng-model="checked"></label><br/>
<button ng-model="button" ng-disabled="checked">Button</button>
```

> protractor.js

``` javascript
it('需要切换按钮状态', function() {
  expect(element(by.css('button')).getAttribute('disabled')).toBeFalsy();
  element(by.model('checked')).click();
  expect(element(by.css('button')).getAttribute('disabled')).toBeTruthy();
});
```
