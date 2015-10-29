# `ng-dblclick`
- Angular@1.4.7
- `ng` 模块中的指令

`ngClick` 指令允许为双击事件指定自定义的行为。


## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<ANY ng-dblclick="expression">
...
</ANY>
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngDblclick|`expression`| 通过双击操作触发需要解析的表达式（事件对象可以通过 $event 获得）|


## 例子

> index.html

``` html
<button ng-dblclick="count = count + 1" ng-init="count=0">
  Increment (on double click)
</button>
count: {{count}}

```
