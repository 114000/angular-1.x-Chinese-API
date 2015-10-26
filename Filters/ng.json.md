# `json`
- Angular@1.4.7
- `ng` 模块中的过滤器

允许你将一个 JavaScript 对象转化成一个 JSON 形式的字符串。

该过滤器多用于调试。当使用双花括号时，绑定将自动转化成 JSON。

## 用法

HTML 模板绑定中

``` html
{{ json_expression | json : spacing}}
```

JavaScript 中

``` javascript
$filter('json')(object, spacing)
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|object|`*`| 需要过滤的任何 JavaScript 对象（包括数组和原始类型） |
|spacing(可选)|`number`| 空格缩进的数量, 默认为两个 2. |

#### 返回

`string` JSON 的字符串

## 例子

> index.html

``` html
<pre id="default-spacing">{{ {'name':'value'} | json }}</pre>
<pre id="custom-spacing">{{ {'name':'value'} | json:4 }}</pre>
```

> protractor.js

``` javascript
it('should jsonify filtered objects', function() {
  expect(element(by.id('default-spacing')).getText()).toMatch(/\{\n  "name": ?"value"\n}/);
  expect(element(by.id('custom-spacing')).getText()).toMatch(/\{\n    "name": ?"value"\n}/);
});
```
