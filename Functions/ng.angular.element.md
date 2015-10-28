# `angular.element`
- Angular@1.4.7
- `ng` 模块中的函数

将一个原生的 DOM 元素或者 HTML 字符串包装成一个 jQuery 元素。

如果引入了 jQuery。`angular.element` 则将是 jQuery 函数的别名。如果没有引入 jQuery，
则 `angular.element` 则表示 Angular 内建的 jQuery 的子级，叫做“jQuery 精简版”或者“jqLite”。

``` html
jqLite 是 jQuery 的一个精简的，并且API兼容的自己，它允许 Angular 使用跨浏览器兼容的方式操纵 DOM。
jqLite 的目标是尽可能小影响代码，所以仅实现最常用功能。
```

如果使用 jQuery，需要确认它在 `angular.js` 文件之前载入。

**注意**：在 Angular 中，所有的元素总是被 `jQuery` 或 `jqLite` 封装过的，它们不会是原生的 DOM。

## Angular's jqLite

jqLite 仅提供以下的 jQuery 方法：

- `addClass()`
- `after()`
- `append()`
- `attr()` - 不支持传入参数
- `bind()` - 不支持命名空间，选择器和事件数据
- `children()` - 不支持选择器
- `clone()`
- `contents()`
- `css()` - 仅仅取回行内样式，不会调用 `getComputedStyle()` 方法。当作为设置元素样式的时候，不会将数值转化为字符串和添加 px。
- `data()`
- `detach()`
- `empty()`
- `eq()`
- `find()` - 限制到只可以用标签名查找
- `hasClass()`
- `html()`
- `next()` - 不支持选择器
- `on()` - 不支持命名空间，选择器和事件数据
- `off()` - 不支持命名空间，选择器和事件对象
- `one()` - 不支持命名空间，选择器
- `parent()` - 不支持选择器
- `prepend()`
- `prop()`
- `ready()`
- `remove()`
- `removeAttr()`
- `removeClass()`
- `removeData()`
- `replaceWith()`
- `text()`
- `toggleClass()`
- `triggerHandler()` - 传入一个虚拟事件对象来处理。
- `unbind()` - 不支持命名空间，选择器和事件对象
- `val()`
- `wrap()`

## jQuery/jqLite 附加功能

Angular 还为 jQuery 和 jqLite 提供了一下的附加方法。

#### 事件

- `$destroy` - AngularJS 拦截了所有的 qLite/jQuery 的摧毁 DOM 的 API 并且在所有 DOM
节点被删除时触发这个事件。这个事件可以用来在 DOM 元素被移除之前清除绑定在它上的任何第三方绑定。

#### 方法

- `controller(name)` - 检索当前元素或其父级的控制器，默认检索与 `ngController` 指令关联的控制器。
如果 `name` 为驼峰形式的指令名，则检索这个指令的控制器（例如：ngModel）。
- `injector()` - 检索当前元素或其父级的注射器。
- `scope()` - 检索当前元素或其父级的作用域，请求调试数据（Requires Debug Data）会被启用。
- `isolateScope()` - 如果当前元素有一个独立的作用域时，可以使用这个方法检索。
这个获取方法只能用在包含有独立作用域指令的元素上。在此元素调用 `scope()` 只返回原有的非独立作用域。
请求调试数据会被启用。
- `inheritedData()` - 与 `data()` 用法相同，但是除非找到一个值，或者到达顶层父元素，否则不会中断。

## 用法

`angular.element(element)`

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|element|`string`<br>`DOMElement`| 需要被封装进 jQuery 的 HTML 字符串或者 DOM 元素。|


#### 返回

`Object` jQuery 对象.
