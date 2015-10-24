# `ng-class-even`
- Angular@1.4.7
- `ng` 模块中的指令

`ngClassOdd` 与 `ngClassEven` 指令工作方式其实与 `ngClass` 是一致的。只不过它们是配合
`ngRepeat` 使用的，并且只在奇数行（或偶数行）生效。

这个指令仅限于在一个 `ngRepeat` 指令的作用域内使用。


## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<ANY ng-class-even="expression">
...
</ANY>
```

用作 CSS 类
``` html
<ANY class="ng-class-even: expression;"> ... </ANY>
```

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngClassEven|`expression`| 需要解析转化的表达式。解析的结果会是一个以空格分割的代表多个 CSS 类名或者一个数组的字符串|


## 例子

> index.html

``` html
<ol ng-init="names=['John', 'Mary', 'Cate', 'Suz']">
  <li ng-repeat="name in names">
   <span ng-class-odd="'odd'" ng-class-even="'even'">
     {{name}} &nbsp; &nbsp; &nbsp;
   </span>
  </li>
</ol>
```

> style.css

``` css
.odd {
  color: red;
}
.even {
  color: blue;
}
```

> protractor.js

``` javascript
it('should check ng-class-odd and ng-class-even', function() {
  expect(element(by.repeater('name in names').row(0).column('name')).getAttribute('class')).
    toMatch(/odd/);
  expect(element(by.repeater('name in names').row(1).column('name')).getAttribute('class')).
    toMatch(/even/);
});
```
