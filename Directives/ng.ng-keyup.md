# `ng-keyup`
- Angular@1.4.7
- `ng` 模块中的指令

为 `keyup` 事件指定之定义行为。

## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<ANY ng-keyup="expression">
...
</ANY>
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngKeyup|`expression`|通过键弹起操作触发需要解析的表达式（事件对象可以通过 $event 获得获得并且有 `keyCode`，`altKey` 等|


## 例子

> index.html

``` html
<p>Typing in the input box below updates the key count</p>
<input ng-keyup="count = count + 1" ng-init="count=0"> key up count: {{count}}

<p>Typing in the input box below updates the keycode</p>
<input ng-keyup="event=$event">
<p>event keyCode: {{ event.keyCode }}</p>
<p>event altKey: {{ event.altKey }}</p>
```
