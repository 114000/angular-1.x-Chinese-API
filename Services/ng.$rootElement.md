# Angular_1.4.3 API 服务篇 $rootElement

- `ng` 模块中的服务

`$rootElement` 是Angular应用中的根元素。它或是 `ngApp`声明处的元素， 或是传入 `Angular.bootstrap` 方法中的元素。它代表应用的根元素。它也是应用的 `$injector` 服务被暴露的位置，并能用 `$rootElement.injector()`方法检索到。
