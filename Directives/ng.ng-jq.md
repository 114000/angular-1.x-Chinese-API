# `ng-jq`
- Angular@1.4.7
- `ng` 模块中的指令

使用这个指令指定 `angular.element` 使用的库。要么是通过空的 `ng-jq` 来指定 `jqLite` 要么是在
`window` 下设置 `jquery` 的变量名（例如：`jQuery`）。

由于 Angular 在其载入后查询这个指令（不要等待 `DOMContentLoaded` 事件），它必须被放置到载入
Angular 的 script 标签前。而且，只有第一个 `ng-jq` 实例会被使用，其他则会被忽略。

## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<ANY
  [ng-jq="string"]>
...
</ANY>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngJq（可选）|`string`| 被用于 `angular.element` 的库名，这个库可以在 `window` 下获取到。|


## 例子

这个例子展示了如何使用 `ngJq` 指令来指定 `jqLite` 到 html 标签上。

``` html
<!doctype html>
<html ng-app ng-jq>
...
...
</html>
```

这个例子展示了如何以另一个不同的名字来使用 `jQuery` 库。这个库的名字必须可以在顶层（`window`）获取到。

``` html
<!doctype html>
<html ng-app ng-jq="jQueryLib">
...
...
</html>
```
