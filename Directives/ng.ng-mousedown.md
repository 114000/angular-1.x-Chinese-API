# `ng-mousedown`
- Angular@1.4.7
- `ng` 模块中的指令

为 `mousedown` 事件指定之定义行为。

## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<ANY ng-mousedown="expression">
...
</ANY>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngKeydown|`expression`| 通过 mousedown 操作触发需要解析的表达式（事件对象可以通过 $event 获得。|


## 例子

``` html
<button ng-mousedown="count = count + 1" ng-init="count=0">
  Increment (on mouse down)
</button>
count: {{count}}
```
