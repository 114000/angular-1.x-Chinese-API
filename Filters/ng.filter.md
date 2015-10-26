# `filter`
- Angular@1.4.7
- `ng` 模块中的过滤器

从输入数组中过滤出符合条件的子集，并返回这个子集的复制品。

## 用法

HTML 模板绑定中

``` html
{{ filter_expression | filter : expression : comparator}}
```

JavaScript 中

``` javascript
$filter('filter')(array, expression, comparator)
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|array|`array`| 原数组 |
|expression|`string`<br>`object`<br>`function()`| 用于选择的谓词（译：既条件）可以是以下之一:<br> - `string`:字符串用来匹配数组的内容。所有包含它的字符串或者字符串属性包含它的对象会被返回。同样适用于嵌套的对象属性中。这个谓词可以通过在字符串前加`!`前缀进行取反。<br><br> - `Object`: 一个可以用于过滤包含在数组内对象的特定属性模式对象，例如：`{name:"M", phone:"1"}` 这个谓词将会返回一个数组，这个数组中的每个项目的 name 属性都会包含 "M"，而 phone 属性都会包含 "1"。`$` 这个特殊的属性名可以用于匹配对象的任何属性，或者它嵌套的对象属性。就相当于像用简单的子字符串匹配字符串那样。这个谓词可以通过在字符串前加`!`前缀进行取反。例如 谓词 `{name: "!M"}` 将会使返回值为一个由 `name` 属性不包含'M'的对象组成的数组<br>**注意**：谓词一个属性仅仅会匹配相同层级的属性，但特殊属性 `$` 会匹配相同层级和更深层级的属性。例如：一个数组 `{name: {first: 'John', last: 'Doe'}}` 不会与 `{name: 'John'}`，但是会与 `{$: 'John'}` 匹配。<br><br>- `function(value, index, array)`: 一个谓词函数可以写出任意的过滤器。这个函数会被数组的每个元素调用，函数得到的参数依次为 （`元素`, `元素索引`, `整个数组`）<br>最后返回的结果是一个 用谓词返回为真值的元素组成的数组。|
|comparator|`function(actual, expected)`<br>`true`<br>`undefined`| 比较器，它用于确定预期值 (来自过滤的表达式) 和实际值 (来自数组中的对象) 是否匹配。可以为一下之一：- `function(actual, expected)`：函数会将对象的值和谓词的值进行比较，如果两个值是相等的则返回 `true`。<br><br>- `true`：`function(actual, expected) { return angular.equals(actual, expected)}` 的简写形式. 对预期值和实际值进行严格的比较。<br><br>- `false`或`undefined`：不区分大小写的方式来寻找子字符串的函数的简写形式.<br><br>原始值被转化成字符串，对象不能与原始值相比较，除非他们自定义的 `toString` 方法（如：Date 对象）。|


#### 返回

`string` 格式化后的数值

## 例子

> index.html

``` html
<div ng-init="friends = [{name:'John', phone:'555-1276'},
                         {name:'Mary', phone:'800-BIG-MARY'},
                         {name:'Mike', phone:'555-4321'},
                         {name:'Adam', phone:'555-5678'},
                         {name:'Julie', phone:'555-8765'},
                         {name:'Juliette', phone:'555-5678'}]"></div>

<label>Search: <input ng-model="searchText"></label>
<table id="searchTextResults">
  <tr><th>Name</th><th>Phone</th></tr>
  <tr ng-repeat="friend in friends | filter:searchText">
    <td>{{friend.name}}</td>
    <td>{{friend.phone}}</td>
  </tr>
</table>
<hr>
<label>Any: <input ng-model="search.$"></label> <br>
<label>Name only <input ng-model="search.name"></label><br>
<label>Phone only <input ng-model="search.phone"></label><br>
<label>Equality <input type="checkbox" ng-model="strict"></label><br>
<table id="searchObjResults">
  <tr><th>Name</th><th>Phone</th></tr>
  <tr ng-repeat="friendObj in friends | filter:search:strict">
    <td>{{friendObj.name}}</td>
    <td>{{friendObj.phone}}</td>
  </tr>
</table>
```

> protractor.js

``` javascript
var expectFriendNames = function(expectedNames, key) {
  element.all(by.repeater(key + ' in friends').column(key + '.name')).then(function(arr) {
    arr.forEach(function(wd, i) {
      expect(wd.getText()).toMatch(expectedNames[i]);
    });
  });
};

it('should search across all fields when filtering with a string', function() {
  var searchText = element(by.model('searchText'));
  searchText.clear();
  searchText.sendKeys('m');
  expectFriendNames(['Mary', 'Mike', 'Adam'], 'friend');

  searchText.clear();
  searchText.sendKeys('76');
  expectFriendNames(['John', 'Julie'], 'friend');
});

it('should search in specific fields when filtering with a predicate object', function() {
  var searchAny = element(by.model('search.$'));
  searchAny.clear();
  searchAny.sendKeys('i');
  expectFriendNames(['Mary', 'Mike', 'Julie', 'Juliette'], 'friendObj');
});
it('should use a equal comparison when comparator is true', function() {
  var searchName = element(by.model('search.name'));
  var strict = element(by.model('strict'));
  searchName.clear();
  searchName.sendKeys('Julie');
  strict.click();
  expectFriendNames(['Julie'], 'friendObj');
});
```
