# `linky`
- Angular@1.4.7
- `ngSanitize` 模块中的过滤器

在文本输入中找到链接（links）并将他们转化为真实的 HTML 链接。 支持 http/https/ftp/mailto 和普通的电子邮件地址的链接。

需要依赖 `ngSanitize` 模块。

## 用法

HTML 模板绑定中

``` html
<span ng-bind-html="linky_expression | linky"></span>
```

JavaScript 中

``` javascript
$filter('linky')(text, target)
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|text|`string`| 输入的内容 |
|target|`string`| `Window` (_blank|_self|_parent|_top) 或者用于打开连接的命名了的 `frame` |

#### 返回

`string` html化链接的文本。

## 例子

> index.html

``` html
<script>
  angular.module('linkyExample', ['ngSanitize'])
    .controller('ExampleController', ['$scope', function($scope) {
      $scope.snippet =
        'Pretty text with some links:\n'+
        'http://angularjs.org/,\n'+
        'mailto:us@somewhere.org,\n'+
        'another@somewhere.org,\n'+
        'and one more: ftp://127.0.0.1/.';
      $scope.snippetWithTarget = 'http://angularjs.org/';
    }]);
</script>
<div ng-controller="ExampleController">
Snippet: <textarea ng-model="snippet" cols="60" rows="3"></textarea>
<table>
  <tr>
    <td>Filter</td>
    <td>Source</td>
    <td>Rendered</td>
  </tr>
  <tr id="linky-filter">
    <td>linky filter</td>
    <td>
      <pre>&lt;div ng-bind-html="snippet | linky"&gt;<br>&lt;/div&gt;</pre>
    </td>
    <td>
      <div ng-bind-html="snippet | linky"></div>
    </td>
  </tr>
  <tr id="linky-target">
   <td>linky target</td>
   <td>
     <pre>&lt;div ng-bind-html="snippetWithTarget | linky:'_blank'"&gt;<br>&lt;/div&gt;</pre>
   </td>
   <td>
     <div ng-bind-html="snippetWithTarget | linky:'_blank'"></div>
   </td>
  </tr>
  <tr id="escaped-html">
    <td>no filter</td>
    <td><pre>&lt;div ng-bind="snippet"&gt;<br>&lt;/div&gt;</pre></td>
    <td><div ng-bind="snippet"></div></td>
  </tr>
</table>
```

> protractor.js

``` javascript
it('should linkify the snippet with urls', function() {
  expect(element(by.id('linky-filter')).element(by.binding('snippet | linky')).getText()).
      toBe('Pretty text with some links: http://angularjs.org/, us@somewhere.org, ' +
           'another@somewhere.org, and one more: ftp://127.0.0.1/.');
  expect(element.all(by.css('#linky-filter a')).count()).toEqual(4);
});

it('should not linkify snippet without the linky filter', function() {
  expect(element(by.id('escaped-html')).element(by.binding('snippet')).getText()).
      toBe('Pretty text with some links: http://angularjs.org/, mailto:us@somewhere.org, ' +
           'another@somewhere.org, and one more: ftp://127.0.0.1/.');
  expect(element.all(by.css('#escaped-html a')).count()).toEqual(0);
});

it('should update', function() {
  element(by.model('snippet')).clear();
  element(by.model('snippet')).sendKeys('new http://link.');
  expect(element(by.id('linky-filter')).element(by.binding('snippet | linky')).getText()).
      toBe('new http://link.');
  expect(element.all(by.css('#linky-filter a')).count()).toEqual(1);
  expect(element(by.id('escaped-html')).element(by.binding('snippet')).getText())
      .toBe('new http://link.');
});

it('should work with the target property', function() {
 expect(element(by.id('linky-target')).
     element(by.binding("snippetWithTarget | linky:'_blank'")).getText()).
     toBe('http://angularjs.org/');
 expect(element(by.css('#linky-target a')).getAttribute('target')).toEqual('_blank');
});
```
