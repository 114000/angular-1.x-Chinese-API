# `angular.fromJson`
- Angular@1.4.7
- `ng` 模块中的函数

将输入序列化成 JSON 形式的字符串。以 `$$` 开头的属性将被略过，因为这代表它是 angular 内部使用的标记。

## 用法

`angular.toJson(obj, pretty)`

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|obj|`string` `array` <br> `Date` `object` <br> `number`| 需要序列化成 JSON 的输入|
|pretty（可选）|`boolean` `number`| 如果设置为 `true`，输出的 JSON 会包含换行符和空白。如果设置为整数，输出的 JSON 将会在每个缩进处使用给定个数的空白。（默认为2）|  

#### 返回

`string` `undefined` 表示 `obj` 的JSON形式的字符串。
