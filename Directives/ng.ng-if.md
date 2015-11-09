# `ng-if`
- Angular@1.4.7
- `ng` 模块中的指令

`ngIf` 指令根据一个 `{ 表达式 }` 的值在 DOM 树中删除或者重新创建某个部分。如果表达式链接的
`ngIf` 解析为 `false` 则元素则会从 DOM 中删除，否则（为`true`）元素的一个克隆会被插入到 `DOM` 中。

`ngIf` 与 `ngShow` 和 `ngHide` 的不同之处在于，`ngIf` 会将元素在 DOM 中完全的删除或者重建，
而并不是通过 `display` 这个 css 属性来改变它的可见性。一个比较常见的例子是使用CSS选择器时要依赖于
DOM 中的某个元素的位置，如 `:first-child` 或者 `:last-child` 伪类，这个差异会变得很明显。

需要注意的是当使用 `ngIf` 删除一个元素时，这个元素的作用域（scope）也会被销毁，反之当元素恢复时一个新的作用域会被创建。
被 `ngIf` 创建的作用域会使用原型继承的方式来从它的父级继承作用域。关于这点的一个重要意义是，
如果 `ngIf` 同时使用 `ngModel` 绑定到父作用域上的一个 javascript 原始定义。在这种情况下，
任何针对子作用域的修改可见性的方式都将会被父作用域的值覆盖。

同样地，`ngIf` 使用他们的编译状态来再现元素。这个行为的一个例子是，如果一个元素的 `class`
的属性会在该元素编译完成后被直接的改写，使用一些像 jQuery 的 `.addClass()` 这样的方法，
在这之后删除了这个元素。当 `ngIf` 重新展现这个元素的时候被添加的 CSS 类会丢失掉，
这是因为再生元素时使用的是原始的编译状态。

另外，你可以通过 `ngAnimate` 模块提供进入和离开的动画效果。

## 指令信息

这个指令会创建一个新的作用域
这个指令的执行优先级为600级
这个指令可以针对嵌套元素（multiElement）使用

## 用法

以元素属性的形式使用:

``` html
<ANY
  ng-if="expression">
...
</ANY>
```

## 动画

enter - 仅在 `ngIf` 内容改变，新的 DOM 元素被创建并注入到 `ngIf` 容器之后生效。
leave - 在 `ngIf` 的内容从 DOM 被删除之前生效。

点击这儿了解更多关于动画中的执行步骤（`$animate`）。

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngIf|`expression`| 如果表达式解析值为假则元素会从 DOM 树中删除，反之向 DOM 树中添加一个编译元素的拷贝。|

## 例子

> index.html

``` html
<label>Click me: <input type="checkbox" ng-model="checked" ng-init="checked=true" /></label><br/>
Show when checked:
<span ng-if="checked" class="animate-if">
  This is removed when the checkbox is unchecked.
</span>
```
> animations.css

``` css
.animate-if {
  background:white;
  border:1px solid black;
  padding:10px;
}

.animate-if.ng-enter, .animate-if.ng-leave {
  transition:all cubic-bezier(0.250, 0.460, 0.450, 0.940) 0.5s;
}

.animate-if.ng-enter,
.animate-if.ng-leave.ng-leave-active {
  opacity:0;
}

.animate-if.ng-leave,
.animate-if.ng-enter.ng-enter-active {
  opacity:1;
}
```
