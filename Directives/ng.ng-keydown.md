# `ng-keydown`
- Angular@1.4.7
- `ng` 模块中的指令

为 `keydown` 事件指定之定义行为。

## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<ANY ng-keydown="expression">
...
</ANY>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngKeydown|`expression`| 通过 keydown 操作触发需要解析的表达式（事件对象可以通过 $event 获得并且有 `keyCode`，`altKey` 等。|


## 例子

``` html
<input ng-keydown="count = count + 1" ng-init="count=0">
key down count: {{count}}
```
