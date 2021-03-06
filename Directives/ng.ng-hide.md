# `ng-hide`
- Angular@1.4.7
- `ng` 模块中的指令

根据元素的 `ng-hide` 属性上表达式的解析式来控制元素的显示与隐藏。元素的显示与隐藏是根据元素上是否有
`.ng-hide` CSS 类来决定的。这个 CSS 类在 AngularJS 内部被预设好并设置了 `display` 样式为 `none`
（使用了 `!important` 标志）。在 CSP 模式下请在你的文件中引入 `angular-csp.css`。（请看 `ngCsp`）

``` html
<!-- 当 $scope.myValue 值为真 (元素被隐藏) -->
<div ng-hide="myValue" class="ng-hide"></div>

<!-- 当 $scope.myValue 值为假 (元素则可见) -->
<div ng-hide="myValue"></div>
```

`ngHide` 的表达式被解析为真值后会为该元素添加一个 `.ng-hide` 的 CSS 类来使其隐藏。如果解析值为假，
则会移除 `.ng-hide` CSS 类来让元素显示出来。

## 为什么要用 `!important` ?

你可能想知道为什要在 `.ng-hide` CSS 类中使用 `!important`。这是因为 `.ng-hide`
选择器可以轻易被级别较高的选择器覆盖。举个例子，像改变一个 HTML 列表项目的 `display`
样式这样简单的事，就会将隐藏的元素显示出来。这更会为 CSS 架构带来更大的问题。

使用 `!important` 之后，显示和隐藏的行为就会按照预期实现，就算 CSS 选择器之前存在冲突也无妨
（当然 `!important` 不能用于有冲突的样式上）。如果开发者想要去通过覆盖样式的方式来改变隐藏元素的方式，
则在他们专属的 CSS 代码中使用 `!important` 就可以了。

#### 覆盖 `.ng-hide`

默认的， `.ng-hide` CSS 类使用 `display: none!important` 来修饰元素。
如果你希望改变 `ngShow/ngHide` 的隐藏方式，那么你可以通过重设 `.ng-hide` CSS 类的样式来实现：

``` css
.ng-hide {
  /* 这是隐藏元素的另一种方式 */
  display: block!important;
  position: absolute;
  top: -9999px;
  left: -9999px;
}
```

## 关于使用 `ngHide` 动画需要注意的一点

`ngShow/ngHide` 动画的工作方式是，当指令的表达式解析为 `true` 和 `false` 时，随即触发显示和隐藏的事件。
这个系统的工作方式就像 `ngClass` 的动画系统，除非被添加或删除的 `.ng-hide` CSS 类被你自己的 CSS 类取代了。

``` css
/* 在页面底部可以找到例子 */

.my-element.ng-hide-add, .my-element.ng-hide-remove {

  transition: 0.5s linear all;
}

.my-element.ng-hide-add { ... }
.my-element.ng-hide-add.ng-hide-add-active { ... }
.my-element.ng-hide-remove { ... }
.my-element.ng-hide-remove.ng-hide-remove-active { ... }
```

要记得，截至 AngularJS 的 1.3.0-beta.11，在动画进行的过程中那个，不需要将显示属性更改为块
—— `ngAnimate` 将会为你触发样式的自动切换。

## 指令信息

这个指令的执行优先级为0级
这个指令可以被当做集合元素（multiElement）使用

## 用法

以元素属性的形式使用:

``` html
<ANY
  ng-hide="expression">
...
</ANY>
```

## 动画

- 删除类（removeClass）：`.ng-hide` - 将会在 `ngHide` 表达式解析为真之后并且在内容被设置为隐藏之前生效。
- 添加类（addClass）：`.ngHide` - 将会在 `ngHide` 表达式解析为非真之后并且在内容被设置为可见之前生效。

点击这儿了解更多关于动画中的执行步骤（`$animate`）。

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngHide|`expression`| 如果表达式解析值为真，元素则被隐藏。|

## 例子

> index.html

``` html
Click me: <input type="checkbox" ng-model="checked" aria-label="Toggle ngShow"><br/>
<div>
  Show:
  <div class="check-element animate-hide" ng-show="checked">
    <span class="glyphicon glyphicon-thumbs-up"></span> I show up when your checkbox is checked.
  </div>
</div>
<div>
  Hide:
  <div class="check-element animate-hide" ng-hide="checked">
    <span class="glyphicon glyphicon-thumbs-down"></span> I hide when your checkbox is checked.
  </div>
</div>
```

> glyphicons.css

``` css
@import url(../../components/bootstrap-3.1.1/css/bootstrap.css);
```

> animations.css

``` css
.animate-hide {
  transition: all linear 0.5s;
  line-height: 20px;
  opacity: 1;
  padding: 10px;
  border: 1px solid black;
  background: white;
}

.animate-hide.ng-hide {
  line-height: 0;
  opacity: 0;
  padding: 0 10px;
}

.check-element {
  padding: 10px;
  border: 1px solid black;
  background: white;
}
```

> protractor.js

``` javascript
var thumbsUp = element(by.css('span.glyphicon-thumbs-up'));
var thumbsDown = element(by.css('span.glyphicon-thumbs-down'));

it('should check ng-show / ng-hide', function() {
  expect(thumbsUp.isDisplayed()).toBeFalsy();
  expect(thumbsDown.isDisplayed()).toBeTruthy();

  element(by.model('checked')).click();

  expect(thumbsUp.isDisplayed()).toBeTruthy();
  expect(thumbsDown.isDisplayed()).toBeFalsy();
});
```
