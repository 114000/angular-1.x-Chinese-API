# `ng-checked`
- Angular@1.4.7
- `ng` 模块中的指令

如果 `ngChecked` 的值为真，则在元素上设置选中（`checked`）属性。

**注意**： 这个指令需要和 `ngModel` 一同使用，否则可能会产生意外行为。

### 为什么我们需要 `ngCheck` ?

HTML 规范并没有要求浏览器保存布尔值类型属性的值，如：`checked`。（他们存在则代表 `true`，
如果没有这个属性则代表 `false`）如果我们将一个 Angular 的插值表达式放到一个这样的属性中，
当浏览器山吃这个属性的时候我们就会失去绑定在这个属性上的信息。 `ngCheck` 指令为选中属性解决了这个问题。
这个补充指令并不会被浏览器删掉，为绑定信息提供了一个永久可靠的存储位置。

## 指令信息

这个指令的执行优先级为100级 （译：终于有不是零的了 `o(*≧▽≦)ツ` ）

## 用法

用作属性：

``` html
<INPUT ng-checked="expression">
...
</INPUT>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngChecked|`expression`| 如果表达式的值为真，则将会在元素上添加 `checked` 属性。|

## 例子

> index.html

``` html
<label>
  Check me to check both:
  <input type="checkbox" ng-model="master">
</label>
<br/>
<input id="checkSlave" type="checkbox" ng-checked="master" aria-label="Slave input">
```

> protractor.js

``` javascript
it('should check both checkBoxes', function() {
  expect(element(by.id('checkSlave')).getAttribute('checked')).toBeFalsy();
  element(by.model('master')).click();
  expect(element(by.id('checkSlave')).getAttribute('checked')).toBeTruthy();
});
```
