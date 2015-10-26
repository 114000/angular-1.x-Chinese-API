# `ng-click`
- Angular@1.4.7
- `ng` 模块中的指令

`ngClick` 指令允许你在一个元素被点击后，为其指定自定义的行为。


## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<ANY ng-click="expression">
...
</ANY>
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngClick|`expression`| 通过点击操作触发需要解析的表达式（事件对象可以通过 $event 获得）|


## 例子

> index.html

``` html
<button ng-click="count = count + 1" ng-init="count=0">
  Increment
</button>
<span>
  count: {{count}}
</span>

```

> protractor.js

``` javascript
it('should check ng-click', function() {
  expect(element(by.binding('count')).getText()).toMatch('0');
  element(by.css('button')).click();
  expect(element(by.binding('count')).getText()).toMatch('1');
});
```
