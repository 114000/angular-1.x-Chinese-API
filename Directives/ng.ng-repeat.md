# `ng-repeat`
- Angular@1.4.7
- `ng` 模块中的指令

`ngRepeat` 指令会为集合中每一项实例化出一个模板。每个模板的实例有它自己的作用域（scope），
在此作用域中会给出集合中当前项所对应的循环变量，和与该项目索引或键值对应的 `$index`。

会在每个模板实例所在的局部作用域中暴露一些特殊的属性。包括：

| 变量 | 形式 | 详细 |
|:----|:---:|:----|
|`$index`|`number`|重复元素的在迭代中的偏移（0……length-1）|
|`$first`|`boolean`| 如果重复元素在迭代的第一位则为 `true`|
|`$middle`|`boolean`| 如果重复元素在迭代的中间位置则为 `true` |
|`$last`|`boolean`| 如果重复元素在迭代的最后一位则为 `true`|
|`$even`|`boolean`| 如果迭代位置 `$index` 是偶数则为真 |
|`$odd`|`boolean`| 如果迭代位置 `$index` 是奇数则为真|

``` html
可以使用 `ngInit` 为这些属性创建别名。可以用于实例化嵌套的 `ngRepeat`。
```

## 迭代对象的属性

可以通过如下方式使用 `ngRepeat` 迭代对象的属性。

``` html
<div ng-repeat="(key, value) in myObj"> ... </div>
```

你需要知道 JavaScript 规范中没有定义返回的对象键的顺序（Angular 1.3 之后，`ng-repeat`
指令使用了字母对键进行排序）。

版本 1.4 移除了字母排序。当轮询 `mgObj` 中的 key 时，我们默认使用的是浏览器返回的顺序。
似乎是这样的，浏览器通常情况下遵循的策略是键被定义时提供的顺序。尽管如此，也有例外情况，
就是在键被删除并恢复的时候，请看[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/delete#Cross-browser_issues](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/delete#Cross-browser_issues)

如果这不是你所期望的，那么推荐你在把数据提供给 `ngRepeat` 之前将你的对象转化为排序过的数组。
你可以通过使用像 `toArrayFilter` 这样的过滤器或者自行为对象添加一个 `$watch` 来实现。

## 跟踪与重复（Tracking and Duplicates）

当集合的内容改变时，`ngRepeat` 也会针对 DOM 做出相应的改变：

- 添加一项，就会向 DOM 中添加这个模板的新的实例。
- 删除一项，从DOM 中删除它的模板实例。
- 当项目发生重排序的时候，它们各自的模板也会在 DOM 中重新排序。

默认情况下，`ngRepeat` 不允许数组中有重复的项。这是由于如果存在重复项的话，不可能维持集合项和
DOM 项之间一一映射的关系。

如果你想迭代重复的项，你可以使用你自己的 `track by` 表达式来替换默认的跟踪行为。

举个例子，你可以使用特殊的作用域属性 `$index` 作为索引来跟踪集合中的每一项。

``` html
<div ng-repeat="n in [42, 42, 43, 43] track by $index">
  {{n}}
</div>
```

你可以在 `track by` 中使用任意的表达式，包括在作用域中的自定义函数。

``` html
<div ng-repeat="n in [42, 42, 43, 43] track by myTrackingFunction(n)">
  {{n}}
</div>
```

如果你使用的对象拥有可以用来识别的属性，你可以跟踪这个识别码来代替跟踪整个对象。如果你稍后重载了你的数据，
`ngRepeat` 也不会为已经渲染过的项重建 DOM 元素，即时集合中的 JavaScript 对象已经被新的东西替换。

``` html
<div ng-repeat="model in collection track by model.id">
  {{model.name}}
</div>
```

在没有为 `track by` 提供表达式的情况下就使用项的特性进行跟踪，相当于使用了内置的 `$id` 函数。

``` html
<div ng-repeat="obj in collection track by $id(obj)">
  {{obj.prop}}
</div>
```

**注意**：`track by` 必须总是位于表达式的最末尾的位置：

``` html
<div ng-repeat="model in collection | orderBy: 'id' as filtered_result track by model.id">
    {{model.name}}
</div>
```

## 特殊的重复的起止点（Special repeat start and end points）

为了重复一系列的元素而不是一个父元素，`ngRepeat`（以及其他的 `ng` 指令）提供了可以扩展循环范围的工具，
它分别使用 `ng-repeat-start` 和 `ng-repeat-end` 明确定义循环的起止点。`ng-repeat-start`
虽然指令与 `ng-repeat` 工作方式相同，但是它会重复出从它自身起（包括 `ng-repeat-start` 所在的标签）
到 `ng-repeat-end` 所在标签为止，内部所有的 HTML 代码。


下面的例子使用了这个特性：

``` html
<header ng-repeat-start="item in items">
  Header {{ item }}
</header>
<div class="body">
  Body {{ item }}
</div>
<footer ng-repeat-end>
  Footer {{ item }}
</footer>
```

And with an input of `['A','B']` for the items variable in the example above, the output will evaluate to:

``` html
<header>
  Header A
</header>
<div class="body">
  Body A
</div>
<footer>
  Footer A
</footer>
<header>
  Header B
</header>
<div class="body">
  Body B
</div>
<footer>
  Footer B
</footer>
```

自定义 `ngRepeat` 起止点的这种方式还支持 AngularJS 中提供的其他所有 HTML 指令的句法风格。
（如 **data-ng-repeat-start**，**x-ng-repeat-start** 和 **ng:repeat-start**）


## 指令信息

这个指令会创建一个新的作用域
这个指令的执行优先级为1000级
这个指令可以针对嵌套元素（multiElement）使用

## 用法

以元素属性的形式使用:

``` html
<ANY
  ng-repeat="repeat_expression">
...
</ANY>
```

## 动画

**.enter** - 当一个新的项被添加到列表中或者被过滤器保留了下来。

**.leave** - 当一个新的项从列表中被移除或者被过滤器过滤掉。

**.move** - 当一个邻项被过滤掉而引起了重排序，或者项目内容发生了重排序。

点击这儿了解更多关于动画中的执行步骤（`$animate`）。

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngRepeat|`repeat_expression`|这个表达式需要说明怎样去枚举一个集合，当前支持以下几种形式：<br><br>- `variable in expression` - `variable` 是用户定义的循环变量，`expression` 是用于集合枚举的一个作用域上的表达式。<br>例如：`album in artist.albums` <br><br>- `(key, value) in expression` - `key` 和 `value` 是可以任意由定义的识别符，`expression` 是用于集合枚举的一个作用域上的表达式。<br>例如：`(name, age) in {'adam':10, 'amalie':12}`<br><br>- `variable in expression track by tracking_expression` - 你可以设置一个跟踪（tracking）表达式用于将 DOM 元素和集合中的对象联系起来。如果没有提供跟踪用的表达式，`ng-repeat` 默认使用身份（idetity）来连接元素。使用多个跟踪表达式来解析同一个键是会产生错误的。（这将意味着两个独立的对象映射到同一个 DOM 元素上，所以这是不可能的）<br>要注意，跟踪表达式要写在最末尾的位置，在过滤器之后，在其他表达式之后。<br>例如：`item in items` 与 `item in items track by $id(item)` 是等效的，这表明是通过数组项的身份连接了DOM元素。<br>例如：`item in items track by $id(item)`。内置的 `$id()` 函数可以用来为数组项分配唯一的 `$$hashKey`属性。这个属性之后会作为一个键将 DOM 元素和相应的数组项连接起来。移动数组中相同的对象也会移动以同样的方式移动 DOM 中的元素。<br>例如：`item in items track by item.id` 是一个来自数据库的项的一个典型的模式。在这种情况下，对象的身份就不重要了，两个对象只有 `id` 属性相同是才会被认为相等。<br>例如：`item in items 竖线 filter:searchText track by item.id` 为项添加过滤去的跟组表达式。<br><br>- `variable in expression as alias_expression` – 你可以提供一个别名表达式来存储过滤后结果。比较有代表性的用法当过滤器在循环体上生效时渲染一个特殊消息，但是过滤后的结果被置空。<br>例如：<br>`item in items 竖线 filter:x as results` 将会在过滤进程完成后将 `items` 存储成 `results`<br>请注意 `as [variable name]` 不是一个操作行为而是 `ngRepeat` 微语法的一部分。所以他只能用在最后（不能用作一个操作，而且要再表达式的内部）。 <br>例如：`item in items 竖线 filter : x 竖线 orderBy : order 竖线 limitTo : limit as results`.|






## 例子

这个例子，在作用域中初始化了一个名字的列表，之后使用 `ngRepeat` 展示出了其中的每一个人。

> index.html

``` html
<div ng-init="friends = [
  {name:'John', age:25, gender:'boy'},
  {name:'Jessie', age:30, gender:'girl'},
  {name:'Johanna', age:28, gender:'girl'},
  {name:'Joy', age:15, gender:'girl'},
  {name:'Mary', age:28, gender:'girl'},
  {name:'Peter', age:95, gender:'boy'},
  {name:'Sebastian', age:50, gender:'boy'},
  {name:'Erika', age:27, gender:'girl'},
  {name:'Patrick', age:40, gender:'boy'},
  {name:'Samantha', age:60, gender:'girl'}
]">
  I have {{friends.length}} friends. They are:
  <input type="search" ng-model="q" placeholder="filter friends..." aria-label="filter friends" />
  <ul class="example-animate-container">
    <li class="animate-repeat" ng-repeat="friend in friends | filter:q as results">
      [{{$index + 1}}] {{friend.name}} who is {{friend.age}} years old.
    </li>
    <li class="animate-repeat" ng-if="results.length == 0">
      <strong>No results found...</strong>
    </li>
  </ul>
</div>
```

> animations.css

``` css
.example-animate-container {
  background:white;
  border:1px solid black;
  list-style:none;
  margin:0;
  padding:0 10px;
}

.animate-repeat {
  line-height:40px;
  list-style:none;
  box-sizing:border-box;
}

.animate-repeat.ng-move,
.animate-repeat.ng-enter,
.animate-repeat.ng-leave {
  transition:all linear 0.5s;
}

.animate-repeat.ng-leave.ng-leave-active,
.animate-repeat.ng-move,
.animate-repeat.ng-enter {
  opacity:0;
  max-height:0;
}

.animate-repeat.ng-leave,
.animate-repeat.ng-move.ng-move-active,
.animate-repeat.ng-enter.ng-enter-active {
  opacity:1;
  max-height:40px;
}
```

> protractor.js

``` javascript
var friends = element.all(by.repeater('friend in friends'));

it('should render initial data set', function() {
  expect(friends.count()).toBe(10);
  expect(friends.get(0).getText()).toEqual('[1] John who is 25 years old.');
  expect(friends.get(1).getText()).toEqual('[2] Jessie who is 30 years old.');
  expect(friends.last().getText()).toEqual('[10] Samantha who is 60 years old.');
  expect(element(by.binding('friends.length')).getText())
      .toMatch("I have 10 friends. They are:");
});

 it('should update repeater when filter predicate changes', function() {
   expect(friends.count()).toBe(10);

   element(by.model('q')).sendKeys('ma');

   expect(friends.count()).toBe(2);
   expect(friends.get(0).getText()).toEqual('[1] Mary who is 28 years old.');
   expect(friends.last().getText()).toEqual('[2] Samantha who is 60 years old.');
 });
```
