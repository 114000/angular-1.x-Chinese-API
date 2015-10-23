# Angular_1.4.3 API 服务篇 $animate
- $animateProvider
- `ng` 模块中的服务


`$animate` 服务暴露了一系列支持动画钩子的 DOM 实用方法。默认行为是DOM操作的应用程序，
然而，一个动画被检测到（并且动画是可用的），`$animate` 会做出大量的操作来确保那些触发DOM操作的动画。

默认情况下，`$animate` 不会触发动画， 这是因为 `ngAnimate` 模块没有被包含进来，而只有当
`ngAnimate` 被依赖之后，动画钩子`$animate` 触发器才能使用。依赖之后所有以 `ng-` 开头的指令
都将会根据他们相关的 DOM 操作来执行动画。其他的指令如：`ngClass`, `ngShow`, `ngHide`, `ngMessage`
也都提供了对动画的支持。

我们推荐你，总是在指令中使用 `$animate` 服务来执行DOM相关的程序。

了解更多的动画支持的使用，点解这访问 [`ngAnimate` 模块页面](https://code.angularjs.org/1.4.3/docs/api/ngAnimate)


## 方法

### 1.`on(event, container, callback)`

无论何时动画事件（enter, leave, move等）在被给定的元素（或其子元素）上执行都会触发你设置的事件监听器。
一旦监听器被触发，提供的回调函数就会接收到以下参数并触发。

``` javascript
$animate.on('enter', container,
   function callback(element, phase) {
     // 酷，我们检测到容器内的进入动画
   }
);
```

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|event |`string`|将被捕获的动画事件 (例如：enter, leave, move, addClass, removeClass,等等)|
|container|`DOMElement` | 容器元素将会捕获每一次动画事件在它自身以及子元素身上执行。|
|callback|`fn` |回调函数在监听器被触发后执行。<br>回调函数接收的参数如下：<br>`element` - 被捕获的需要执行动画的 DOM 元素。<br>`phase` - 动画的阶段，动画开始和结束时都有两个可能出现的状态。|

### 2.`off(event, [container], [callback])`

注销基于已与所提供的元素相关联的事件的事件监听器。这个方法可以依据参数有三种不同的使用方法:

``` javascript

// 删除所有监听 `enter`事件的动画监听器。
$animate.off('enter');

// 删除所有绑定在给定元素（及其子元素）上的监听 `enter` 事件的动画事件监听器
$animate.off('enter', container);

// 删除由 `listenerFn` 提供用来监听
// `container` 及其子元素身上的 `enter` 事件监听器函数
$animate.off('enter', container, callback);

```

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|event|	`string`| 动画事件 (如 enter, leave, move, addClass, removeClass等)|
|container *（可选）*|`DOMElement`| 事件监听器被放置的容器元素|
|callback *（可选）*|`Function=`|被注册为监听器的回调函数|

### 3.`pin(element, parentElement)`

将提供的元素和一个父元素联系在一起，允许即使它存在于 Angular 应用的DOM结构的外部时也可以出发动画。
这样一来，无论这个元素是在应用域外还是在另一个应用中 `$animate` 触发的动画都可以在该元素上生效。
例如说，假如应用在一个 `<body>` 内部的某一个元素上已经启动了。但是我们仍需要使一个元素成为
`document.body` 的直接子元素。这时我们可以通过使用 `$animate.pin(element)` 来实现这个连接这个元素。
别忘了，调用 `$animate.pin(element, parentElement)` 并不会实际地将其插入到 DOM 中。它仅仅是被连接起来的。

注意：这个特点仅仅在 `ngAnimate` 模块被引入的情况下可以使用。

**参数**

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
|element|	`DOMElement`| 需要被连接的外部元素|
|parentElement | `DOMElement`| 将会被外部元素连接的主父元素|


### 4.`enabled([element], [enabled])`

用于获取和设置动画是否启用与否对整个应用程序或元素及其子上。 这个函数可以使用以下四种方式调用:


``` javascript
// 返回 true 或者 false
$animate.enabled();

// 改变所有动画的启用状态
$animate.enabled(false);
$animate.enabled(true);

// 返回 true 或者 false 判断一个元素的动画是否启用

// 改变一个元素及其子元素动画的启用状态
$animate.enabled(element, true);
$animate.enabled(element, false);
```

**参数**

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
|element *（可选）*| `DOMElement` | 将被考虑检测/设置启用状态的元素|
|enabled *（可选）*| `boolean` | 对于 element 动画是否启用 |

**返回**

`boolean` - 动画是否启用

### 5.`cancel(animationPromise)`

取消提供的动画

**参数**

| 参数 | 形式 | 详细 |
|:---|:---:|:---|
|animationPromise|`Promise`| 一个动画开始时返回的动画 `promise`|




### 6.`enter(element, parent, [after], [options])`

将元素插入DOM或 `after` 元素(如果提供了)之后，或者作为父元素的第一个元素，之后会触发动画。
当动画执行完毕之后的 `digest` 将会返回一个被解析的 `promise`。

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|element|`DOMElement`| 需要插入 DOM 的元素|
|parent| `DOMElement`| 作为插入元素父级的元素(只要该元素之后不存在)|
|after *（可选）*| `DOMElement`| 作为插入元素的兄弟元素|
|options *（可选）*| `object` | 一个可选的集合，其中的项目和样式都将应用到元素上|


**返回**

`Promise` - 动画回调的 `promise`

### 7.`move(element, parent, [after], [options])`


插入或移动元素到一个 DOM 中的新的位置或 `after` 元素（如果提供的话）之后或作为母体元件内的第一个子元素，
然后触发一个动画。当动画执行完毕之后的 `digest` 将会返回一个被解析的 `promise`。

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|element|`DOMElement`| 将要移动到新位置的元素|
|parent	|`DOMElement`|作为插入元素父级的元素(只要该元素之后不存在)|
|after *（可选）*| `DOMElement`| 作为插入元素的兄弟元素|
|options *（可选）*| `object` | 一个可选的集合，其中的项目和样式都将应用到元素上|

**返回**

`Promise` - 动画的回调 `promise`


### 8.`leave(element, [options])`


触发一个动画并在之后重DOM中删除这个元素。当动画执行完毕之后的 `digest` 将会返回一个被解析的 `promise`。

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|element|`DOMElement`|将从 DOM 中删除的元素|
|options *（可选）*| `object` | 一个可选的集合，其中的项目和样式都将应用到元素上|

**返回**

`Promise` - 动画的回调 `promise`


### 9.`addClass(element, className, [options])`

当添加了一个提供的 CSS 类名时会触发一个 `addClass` 动画 。关于执行方面，`addClass` 操作仅会在下一次
`digest` 后被执行。但如果元素已经包含了这个 CSS 类名亦或 这个类名被稍后的动作删除了，那动画也
不会被触发。需要注意的是，基于类名的动画要区别于结构动画（像 `enter`, `move`, `leave`），
因为如果 CSS 或 Javascript 动画被使用，CSS 类名可能会在不同的地方被依赖使用（添加或删除）

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|element|`DOMElement`| 将要应用 CSS 类名的元素|
|className|`string`| 将要被添加的 CSS 类名 （多类名由空格分开）|
|options *（可选）*| `object`|一个可选的集合，其中的项目和样式都将应用到元素上|

**返回**

`Promise` - 动画的回调 `promise`


### 10.`removeClass(element, className, [options])`

当删除一个提供 CSS 类名时会触发一个 `removeClass` 动画。。关于执行方面，`removeClass` 操作仅会在下一次
`digest` 后被执行。如果元素没有包含 CSS 类名，或在稍后会添加这个类名则动画就不会被触发。
需要注意的是，基于类名的动画要区别于结构动画（像 `enter`, `move`, `leave`），
因为如果 CSS 或 Javascript 动画被使用，CSS 类名可能会在不同的地方被依赖使用（添加或删除）

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|element|`DOMElement`| CSS类名要应用的元素 |
|className|`string`| 将被移除的 CSS 类名（多类名由空格分开）|
|options *（可选）*|`object`|一个可选的集合，其中的项目和样式都将应用到元素上|

**返回**

`Promise` - 动画的回调 `promise`


### 11.`setClass(element, add, remove, [options])`

同时执行一个元素上 CSS 类名的添加和删除（在这个过程中）并依据添加或删除触发动画。与
`$animate.addClass` 和 `$animate.removeClass`类似，setClass 仅会在这次 `digest` 完成后
对类名进行一次修改。需要注意的是，基于类名的动画要区别于结构动画（像 `enter`, `move`, `leave`），
因为如果 CSS 或 Javascript 动画被使用，CSS 类名可能会在不同的地方被依赖使用（添加或删除）

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|element|`DOMElement`|CSS类名要应用的元素|
|add| `string` |将被添加的 CSS 类名（多类名由空格分开）|
|remove| `string` |将被删除的 CSS 类名（多类名由空格分开）|
|options *（可选）*|`object`|一个可选的集合，其中的项目和样式都将应用到元素上|

**返回**

`Promise` - 动画的回调 `promise`


### 12.`animate(element, from, to, [className], [options])`

在元素上执行一个指定了起始和结束的样式的内联动画。如果任何检测到的CSS过渡，关键帧或 JavaScript
匹配了提供的 className 的值，则该动画将使用他们所提供的样式。例如，如果一个过渡动画设置为了给定的
className，那么所提供的 `from` 和 `to` 的样式将沿着给定的过渡施加。如果检测到了 JavaScript
动画就将设定的样式最为函数的参数传递到动画的方法中（或者作为选项参数的一部分）

**参数**

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|element|`DOMElement`| CSS 样式应用的元素|
|from	| `object`| 会被应用到元素和整个动画过程的初始的 CSS 样式|
|to	| `object`| 会被应用到元素和整个动画过程的结束的 CSS 样式|
|className *（可选）*|`string`| 一个将要在动画的执行过程中应用到元素上的可设置的 CSS 类。<br>如果这个这保留为空，那么一个 `ng-inline-animate` CSS 类将被应用到元素中。<br>（注意，如果没有检测到动画，那么，这个这应不会应用到元素身上）|
|options *（可选）*| `object`| 一个将被应用到元素的可控的设置/样式的集合|

**返回**

`Promise` - 动画的回调 `promise`
