# `ng-copy`
- Angular@1.4.7
- `ng` 模块中的指令

为复制（copy）事件指定自定义的行为。


## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<window, input, select, textarea, a
  ng-copy="expression">
...
</window, input, select, textarea, a>
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngCopy|`expression`| 通过复制操作触发需要解析的表达式（事件对象可以通过 $event 获得）|


## 例子

> index.html

``` html
<input ng-copy="copied=true" ng-init="copied=false; value='copy me'" ng-model="value">
copied: {{copied}}

```
