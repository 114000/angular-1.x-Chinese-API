# `lowercase`
- Angular@1.4.7
- `ng` 模块中的过滤器

将字符串转化为小写

## 用法

HTML 模板绑定中

``` html
{{ lowercase_expression | lowercase}}
```

JavaScript 中

``` javascript
$filter('lowercase')(input)
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|input|`string`| 过滤器的输入 |

#### 返回

`string` 格式化后的数值
