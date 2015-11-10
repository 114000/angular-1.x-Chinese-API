# `ng-mouseup`
- Angular@1.4.7
- `ng` 模块中的指令

为 `mouseup` 事件指定之定义行为。

## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<ANY ng-mouseup="expression">
...
</ANY>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngMouseup|`expression`| 通过 mouseup 操作触发需要解析的表达式（事件对象可以通过 $event 获得）。|


## 例子

``` html
<button ng-mouseup="count = count + 1" ng-init="count=0">
  Increment (on mouse up)
</button>
count: {{count}}
```
