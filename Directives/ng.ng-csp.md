# `ng-csp`
- Angular@1.4.7
- `ng` 模块中的指令


Angular 的一些特性会破坏某些 CSP （Content Security Policy，内容安全策略）规则。

如果你想使用这些规则就需要告知 Angular 不要使用破坏这些规则的特性。

当开发想 Google Chrome 扩展或者通用的 Windows 应用时，这么做还是很有必要的。

下面的规则会影响 Angular：

- `unsafe-eval`：这个规则禁止应用使用 `eval` 或者 `Function(string)` 生成函数（以及其中携带的其他东西）。
Angular 在 `$parse` 服务中使用了这种方式来为解析 Angular 表达式来增加 30% 的速度。

- `unsafe-inline`: 这个规则禁止应用将自定义的样式注入到文档中。Angular 使用了这种方式添加了一些
CSS 规则（如：`ngCloak` 和 `ngHide`）。为了使这些只能能在 CSP 规则下正常工作，你必须手动引入
angular-csp.css

如果你不提供 `ngCsp` Angular 会尝试自动检测，如果 CSP 阻止了不安全的 eval (unsafe-eval)
Angular 就会自动关闭 `$parse` 服务中的这个特性。虽然有这个自动检测，但是会触发一个 CSP 的错误。
这个错误会被打印到控制台中：


``` html
拒接将一个字符串解析为 JavaScript，因为在 CSP 指令中 'unsafe-eval'："default-src 'self'"
是一个不被认可的脚本源。请注意 'script-src' 没有被明确的设置。所以 'default-src' 作为了备用。
```

这个错误虽然无害但是很招人烦。为了防止报错，你可以在引入 angular.js 文件的那个 `<script>`
之前的一个元素上添加 `ngCsp` 指令。

**注意**： 这个指令仅会在 `ng-csp` 和 `data-ng-csp` 这两种属性形式下生效。

你可以为 `ng-csp` 属性赋值来控制 Angular 应该关闭那些与 CSP 相关的特性。设置参见下面：

- `no-inline-style`：禁止 Angular 向 DOM 中注入 CSS 样式。
- `no-unsafe-eval`：禁止 Angular 使用以不安全的 eval 字符串的方式优化了的 `$parse`

你可以想下面的方式那样组合着使用这些值：

不声明意味着 Angular 会假设你允许使用了行内样式，但是它在执行时会为不安全的 eval 做检查。
例如 `<body>` 。 这与 Angular 之前的版本向后兼容。

一个简单的 `ng-csp`（或者 `data-ng-csp`）属性就可以告知 Angular 将行内样式和不安全的 eval
特性全部禁止。例如：`<body ng-csp>`。这与 Angular 之前的版本向后兼容。

仅仅赋值 `no-unsafe-eval` 则告诉 Angular 不能使用 eval。但是我们仍然可以使用行内样式。
例如：`<body ng-csp="no-unsafe-eval">`

仅仅赋值 `no-inline-style` 则告诉 Angular 不能注入样式。但是我们仍然可以使用 eval -
不会发生自动检查不安全的 eval 的情况。例如：`<body ng-csp="no-inline-style">`

将 `no-inline-style` 和 `no-unsafe-eval` 都赋值给 `ngCsp` 则告诉 Angular 我们不能使用
eval 和注入样式。这种情况与置空效果是一样的：`ng-csp`。例如 `<body ng-csp="no-inline-style;no-unsafe-eval">`

## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<html>
...
</html>
```

## 例子

这个指令展示了如何在 html 标签上使用 `ngCsp` 指令。

``` html
<!doctype html>
<html ng-app ng-csp>
...
...
</html>
```

// 注意: the suffix .csp in the example name triggers // csp mode in our http server!

> index.html

``` html
<div ng-controller="MainController as ctrl">
  <div>
    <button ng-click="ctrl.inc()" id="inc">Increment</button>
    <span id="counter">
      {{ctrl.counter}}
    </span>
  </div>

  <div>
    <button ng-click="ctrl.evil()" id="evil">Evil</button>
    <span id="evilError">
      {{ctrl.evilError}}
    </span>
  </div>
</div>
```

> script.js

``` javascript
angular.module('cspExample', [])
.controller('MainController', function() {
   this.counter = 0;
   this.inc = function() {
     this.counter++;
   };
   this.evil = function() {
     // jshint evil:true
     try {
       eval('1+2');
     } catch (e) {
       this.evilError = e.message;
     }
   };
 });
```

> protractor.js

``` javascript
var util, webdriver;

var incBtn = element(by.id('inc'));
var counter = element(by.id('counter'));
var evilBtn = element(by.id('evil'));
var evilError = element(by.id('evilError'));

function getAndClearSevereErrors() {
  return browser.manage().logs().get('browser').then(function(browserLog) {
    return browserLog.filter(function(logEntry) {
      return logEntry.level.value > webdriver.logging.Level.WARNING.value;
    });
  });
}

function clearErrors() {
  getAndClearSevereErrors();
}

function expectNoErrors() {
  getAndClearSevereErrors().then(function(filteredLog) {
    expect(filteredLog.length).toEqual(0);
    if (filteredLog.length) {
      console.log('browser console errors: ' + util.inspect(filteredLog));
    }
  });
}

function expectError(regex) {
  getAndClearSevereErrors().then(function(filteredLog) {
    var found = false;
    filteredLog.forEach(function(log) {
      if (log.message.match(regex)) {
        found = true;
      }
    });
    if (!found) {
      throw new Error('expected an error that matches ' + regex);
    }
  });
}

beforeEach(function() {
  util = require('util');
  webdriver = require('protractor/node_modules/selenium-webdriver');
});

// For now, we only test on Chrome,
// as Safari does not load the page with Protractor's injected scripts,
// and Firefox webdriver always disables content security policy (#6358)
if (browser.params.browser !== 'chrome') {
  return;
}

it('should not report errors when the page is loaded', function() {
  // clear errors so we are not dependent on previous tests
  clearErrors();
  // Need to reload the page as the page is already loaded when
  // we come here
  browser.driver.getCurrentUrl().then(function(url) {
    browser.get(url);
  });
  expectNoErrors();
});

it('should evaluate expressions', function() {
  expect(counter.getText()).toEqual('0');
  incBtn.click();
  expect(counter.getText()).toEqual('1');
  expectNoErrors();
});

it('should throw and report an error when using "eval"', function() {
  evilBtn.click();
  expect(evilError.getText()).toMatch(/Content Security Policy/);
  expectError(/Content Security Policy/);
});
```
