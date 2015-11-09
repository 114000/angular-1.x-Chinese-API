# `ng-href`
- Angular@1.4.7
- `ng` 模块中的指令

在 `href` 属性中使用像 `{{hash}}` 这样的 Angular 标记时，如果用户在 Angular 替换掉
`{{hash}}` 之前点击了链接，那么链接则会走向错误的 URL 地址。直到 Angular 替换掉标记。
这个链接将会被打破，并且很有可能将返回一个 404 错误。`ng-href` 指令将会解决这个问题。

错误的写法：

``` html
<a href="http://www.gravatar.com/avatar/{{hash}}">link1</a>
```

正确的写法：

``` html
<a ng-href="http://www.gravatar.com/avatar/{{hash}}">link1</a>
```

## 指令信息

这个指令的执行优先级为99级

## 用法

用作属性：

``` html
<A
  ng-href="template">
...
</A>
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngHref|`template`| 任何包含双花括号标记的字符串。|


## 例子

这个例子展示了链接中 `href`, `ng-href`,`ng-click` 属性的各种组合使用以及他们的不同行为。

> index.html

``` html
<input ng-model="value" /><br />
<a id="link-1" href ng-click="value = 1">link 1</a> (link, don't reload)<br />
<a id="link-2" href="" ng-click="value = 2">link 2</a> (link, don't reload)<br />
<a id="link-3" ng-href="/{{'123'}}">link 3</a> (link, reload!)<br />
<a id="link-4" href="" name="xx" ng-click="value = 4">anchor</a> (link, don't reload)<br />
<a id="link-5" name="xxx" ng-click="value = 5">anchor</a> (no link)<br />
<a id="link-6" ng-href="{{value}}">link</a> (link, change location)
```

> protractor.js

``` javascript
it('should execute ng-click but not reload when href without value', function() {
  element(by.id('link-1')).click();
  expect(element(by.model('value')).getAttribute('value')).toEqual('1');
  expect(element(by.id('link-1')).getAttribute('href')).toBe('');
});

it('should execute ng-click but not reload when href empty string', function() {
  element(by.id('link-2')).click();
  expect(element(by.model('value')).getAttribute('value')).toEqual('2');
  expect(element(by.id('link-2')).getAttribute('href')).toBe('');
});

it('should execute ng-click and change url when ng-href specified', function() {
  expect(element(by.id('link-3')).getAttribute('href')).toMatch(/\/123$/);

  element(by.id('link-3')).click();

  // At this point, we navigate away from an Angular page, so we need
  // to use browser.driver to get the base webdriver.

  browser.wait(function() {
    return browser.driver.getCurrentUrl().then(function(url) {
      return url.match(/\/123$/);
    });
  }, 5000, 'page should navigate to /123');
});

it('should execute ng-click but not reload when href empty string and name specified', function() {
  element(by.id('link-4')).click();
  expect(element(by.model('value')).getAttribute('value')).toEqual('4');
  expect(element(by.id('link-4')).getAttribute('href')).toBe('');
});

it('should execute ng-click but not reload when no href but name specified', function() {
  element(by.id('link-5')).click();
  expect(element(by.model('value')).getAttribute('value')).toEqual('5');
  expect(element(by.id('link-5')).getAttribute('href')).toBe(null);
});

it('should only change url when only ng-href', function() {
  element(by.model('value')).clear();
  element(by.model('value')).sendKeys('6');
  expect(element(by.id('link-6')).getAttribute('href')).toMatch(/\/6$/);

  element(by.id('link-6')).click();

  // At this point, we navigate away from an Angular page, so we need
  // to use browser.driver to get the base webdriver.
  browser.wait(function() {
    return browser.driver.getCurrentUrl().then(function(url) {
      return url.match(/\/6$/);
    });
  }, 5000, 'page should navigate to /6');
});
```
