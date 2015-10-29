# `ng-cut`
- Angular@1.4.7
- `ng` 模块中的指令

为剪切（cut）指定自定义行为。


## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<window, input, select, textarea, a
  ng-cut="expression">
...
</window, input, select, textarea, a>
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngCut|`expression`| 通过剪切操作触发需要解析的表达式（事件对象可以通过 $event 获得）|


## 例子

> index.html

``` html
<input ng-cut="cut=true" ng-init="cut=false; value='cut me'" ng-model="value">
cut: {{cut}}
```
