# `ng-focus`
- Angular@1.4.7
- `ng` 模块中的指令

为聚焦事件（`focus`）设置自定义行为。

**注意**：由于当 `input.focus()`调用时聚焦事件是被同步执行的。所以，如果事件被触发，
AngularJS 会使用 `scope.$evalAsync` 来解析执行表达式并使用 `$apply` 以确保拥有一致的状态。


## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<window, input, select, textarea, a
  ng-focus="expression">
...
</window, input, select, textarea, a>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngFocus|`expression`| 在聚焦时需要解析的表达式 (事件对象可以通过 `$event` 获得)|

## 例子

请看 `ngClick`
