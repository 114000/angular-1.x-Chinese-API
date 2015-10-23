# `<a>`
- Angular@1.4.7
- `ng` 模块中的指令

修改了 `html` 中 `A`标签的默认行为，以阻止当 `href` 属性为空时触发的默认行为。

这个修改会让使用 `ngClick` 指令创建的简写链接不会不会改变 `location` 或者引起页面的重载。如
`<a href="" ng-click="list.addItem()">Add Item</a>`


## 指令信息

这个指令的执行优先级为0级

## 用法

像个元素（标签）一样使用

> html

``` html
<a>
...
</a>
```
