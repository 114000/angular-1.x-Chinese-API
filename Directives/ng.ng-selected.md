# `ng-selected`
- Angular@1.4.7
- `ng` 模块中的指令


HTML 规范中没有要求浏览器保存属性的布尔值，如 `selected` （这些属性存在则代表 `true`，不存在则代表
`false`）如果我们将一个 Angular 的插值表达式放入一个这样的属性中，那么当浏览器删除这个属性时，
我们将丢失绑定的信息。`ngSelected` 指令解决了 `selected` 属性的这个问题。这个补充的属性不会被浏览器移除
并且提供了一个永久可靠的地方来存储绑定信息。

## 指令信息

这个指令的执行优先级为100级

## 用法

用作属性：

``` html
<OPTION
  ng-selected="expression">
...
</OPTION>
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngSelected|`expression`| 如果表达式为真，则特殊的属性 selected 将会被设置到元素上|


## 例子

> index.html

``` html
<label>Check me to select: <input type="checkbox" ng-model="selected"></label><br/>
<select aria-label="ngSelected demo">
  <option>Hello!</option>
  <option id="greet" ng-selected="selected">Greetings!</option>
</select>
```

> protractor.js

``` javascript
it('should select Greetings!', function() {
  expect(element(by.id('greet')).getAttribute('selected')).toBeFalsy();
  element(by.model('selected')).click();
  expect(element(by.id('greet')).getAttribute('selected')).toBeTruthy();
});
```
