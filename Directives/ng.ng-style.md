# `ng-style`
- Angular@1.4.7
- `ng` 模块中的指令

`ngStyle` 指令允许你在 HTML 元素上设置 CSS 样式。

## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<ANY
  ng-style="expression">
...
</ANY>
```

用作 CSS 类
``` html
<ANY class="ng-style: expression;"> ... </ANY>
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngStyle|`expression`|表达式解析成为一个对象，这个对象的键就是 CSS 的样式名，键对应的值就是样式的值<br>由于一些 CSS 的样式名对于一个对象来说不是一种合法的键名的表示，所以它们必须添加引号。请看下面关于 'background-color'的例子|

## 例子

> index.html

``` html
<input type="button" value="set color" ng-click="myStyle={color:'red'}">
<input type="button" value="set background" ng-click="myStyle={'background-color':'blue'}">
<input type="button" value="clear" ng-click="myStyle={}">
<br/>
<span ng-style="myStyle">Sample Text</span>
<pre>myStyle={{myStyle}}</pre>
```

> style.css

``` css
span {
  color: black;
}
```

> protractor.js

``` javascript
var colorSpan = element(by.css('span'));

it('should check ng-style', function() {
  expect(colorSpan.getCssValue('color')).toBe('rgba(0, 0, 0, 1)');
  element(by.css('input[value=\'set color\']')).click();
  expect(colorSpan.getCssValue('color')).toBe('rgba(255, 0, 0, 1)');
  element(by.css('input[value=clear]')).click();
  expect(colorSpan.getCssValue('color')).toBe('rgba(0, 0, 0, 1)');
});
```
