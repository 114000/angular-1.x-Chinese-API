# `ng-src`
- Angular@1.4.7
- `ng` 模块中的指令

在 `src` 属性中使用使用像 `{{hash}}` 这样的 Angular 标记时不能正确的工作：
直到直到 Angular 替换掉标记之前，浏览器都会使用包含了 `{{hash}}` 的字面文本来获取源数据。
`ng-src` 指令将会解决这个问题。

错误的写法:

``` html
<img src="http://www.gravatar.com/avatar/{{hash}}" alt="Description"/>
```

正确的写法：

``` html
<img ng-src="http://www.gravatar.com/avatar/{{hash}}" alt="Description" />
```


## 指令信息

这个指令的执行优先级为99级

## 用法

用作属性：

``` html
<IMG
  ng-src="template">
...
</IMG>
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngSrc|`expression`| 任何包含双花括号标记的字符串。|
