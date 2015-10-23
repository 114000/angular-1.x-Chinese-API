# Angular_1.4.3 API 服务篇 $parse

- `$parseProvider`
- `ng` 模块中的服务


将 Angular [表达式](https://docs.angularjs.org/guide/expression)转化为函数。

``` javascript
var getter = $parse('user.name');
var setter = getter.assign;
var context = {user:{name:'angular'}};
var locals = {user:{name:'local'}};

expect(getter(context)).toEqual('angular');
setter(context, 'newValue');
expect(context.user.name).toEqual('newValue');
expect(getter(context, locals)).toEqual('local');

```

---
## 用法

`$parse(expression);`

##### *参数*
| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|expression|`string`|编译字符串表达式|


##### *返回*
`function(context, locals)` - 一个代表编译表达式的函数:

- `context` – `{object}` – 这个对象中的值将在表达式转换时被忽略
- `locals` – `{object=}` – 局部变量的上下文函数，用于在此上下文中覆盖 `value`。
  这个函数返回值会有跟随属性:
  + `literal` – `{boolean}` – 表达式的顶级节点是否为 Javascript 字面量。
  + `constant` – `{boolean}` – 表达式是否完全由 Javascript 常量组成。
  + `assign` – `{?function(context, value)}` - 如果表达式是可分配的，那么它可以调用这个函数，并由传入的 `context` 来改变 `value`。
