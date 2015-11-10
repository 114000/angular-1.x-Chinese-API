# `ng-mouseover`
- Angular@1.4.7
- `ng` 模块中的指令

为 `mouseover` 事件指定之定义行为。

## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<ANY ng-mouseover="expression">
...
</ANY>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngMouseover|`expression`| 通过 mouseover 操作触发需要解析的表达式（事件对象可以通过 $event 获得）。|


## 例子

``` html
<button ng-mouseover="count = count + 1" ng-init="count=0">
  Increment (when mouse is over)
</button>
count: {{count}}
```
