# `ng-class`
- Angular@1.4.7
- `ng` 模块中的指令

`ngClass` 指令允许你在 HTML 元素上通过数据绑定一个表达式动态的方式设置 CSS 类，
这个表达式代表了所有要被添加的 CSS 类名。

操作指令有三种不同的方式，取决于解析表达式的三种形式：


* 如果表达式解析为一个字符串，这个字符串应该使用空格来分割 CSS 类名。

* 如果表达式解析为一个对象，那么每个键值对（key-value）中，如果值（value）为真值（true）
那么与它对应的键名（key）将被当做 CSS 类名添加进去。（译：{red: true, blue: 0} `red`
会被添加，`blue` 则不会）

* 如果表达式解析为一个数组，那么数组中的每个元素的形式都应该为类型1中所述的字符串形式，或者类型2
中所述的对象形式。也就是说你可以在一个数组中混合使用这两种表达式来更好的控制出现的 CSS 类名。
下面的例子会展现出来。

`ngClass` 不会重复添加一个已经被设置了的CSS类名

如果表达式改变了，先删除之前添加的类，然后才加入新的类。


## 指令信息

这个指令的执行优先级为0级

## 用法

用作属性：

``` html
<ANY ng-class="expression">
...
</ANY>
```

用作 CSS 类
``` html
<ANY class="ng-class: expression;">
...
</ANY>
```

## 动画

**add** - 会在 CSS 类被应用到元素之前触发。

**remove** - 会在 CSS 类从元素上移除之前触发。

点击这里了解更多关于参与动画的步骤。（译：链接就是到 `$animate` 服务中）

#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|ngClass|`expression`| 解析的结果可以是一个使用空格分隔的 CSS 类名的字符串，一个数组，或者一个 CSS类名到布尔值的映射（map）。
如果是映射（译：也就是之前说的 `object`），属性值为真值的**属性名**会被作为 CSS 类名添加到元素上。|


## 例子

这个例子听过使用 `ngClass` 指令来说明基础的绑定。

> index.html

``` html
<p ng-class="{strike: deleted, bold: important, 'has-error': error}">Map Syntax Example</p>
<label>
   <input type="checkbox" ng-model="deleted">
   deleted (apply "strike" class)
</label><br>
<label>
   <input type="checkbox" ng-model="important">
   important (apply "bold" class)
</label><br>
<label>
   <input type="checkbox" ng-model="error">
   error (apply "has-error" class)
</label>
<hr>
<p ng-class="style">Using String Syntax</p>
<input type="text" ng-model="style"
       placeholder="Type: bold strike red" aria-label="Type: bold strike red">
<hr>
<p ng-class="[style1, style2, style3]">Using Array Syntax</p>
<input ng-model="style1"
       placeholder="Type: bold, strike or red" aria-label="Type: bold, strike or red"><br>
<input ng-model="style2"
       placeholder="Type: bold, strike or red" aria-label="Type: bold, strike or red 2"><br>
<input ng-model="style3"
       placeholder="Type: bold, strike or red" aria-label="Type: bold, strike or red 3"><br>
<hr>
<p ng-class="[style4, {orange: warning}]">Using Array and Map Syntax</p>
<input ng-model="style4" placeholder="Type: bold, strike" aria-label="Type: bold, strike"><br>
<label><input type="checkbox" ng-model="warning"> warning (apply "orange" class)</label>
```

> style.css

``` css
.strike {
    text-decoration: line-through;
}
.bold {
    font-weight: bold;
}
.red {
    color: red;
}
.has-error {
    color: red;
    background-color: yellow;
}
.orange {
    color: orange;
}
```

> protractor.js

``` javascript
var ps = element.all(by.css('p'));

it('should let you toggle the class', function() {

  expect(ps.first().getAttribute('class')).not.toMatch(/bold/);
  expect(ps.first().getAttribute('class')).not.toMatch(/has-error/);

  element(by.model('important')).click();
  expect(ps.first().getAttribute('class')).toMatch(/bold/);

  element(by.model('error')).click();
  expect(ps.first().getAttribute('class')).toMatch(/has-error/);
});

it('should let you toggle string example', function() {
  expect(ps.get(1).getAttribute('class')).toBe('');
  element(by.model('style')).clear();
  element(by.model('style')).sendKeys('red');
  expect(ps.get(1).getAttribute('class')).toBe('red');
});

it('array example should have 3 classes', function() {
  expect(ps.get(2).getAttribute('class')).toBe('');
  element(by.model('style1')).sendKeys('bold');
  element(by.model('style2')).sendKeys('strike');
  element(by.model('style3')).sendKeys('red');
  expect(ps.get(2).getAttribute('class')).toBe('bold strike red');
});

it('array with map example should have 2 classes', function() {
  expect(ps.last().getAttribute('class')).toBe('');
  element(by.model('style4')).sendKeys('bold');
  element(by.model('warning')).click();
  expect(ps.last().getAttribute('class')).toBe('bold orange');
});
```

## 动画例子

> index.html

``` html
<input id="setbtn" type="button" value="set" ng-click="myVar='my-class'">
<input id="clearbtn" type="button" value="clear" ng-click="myVar=''">
<br>
<span class="base-class" ng-class="myVar">Sample Text</span>
```

> style.css

``` css
.base-class {
  transition:all cubic-bezier(0.250, 0.460, 0.450, 0.940) 0.5s;
}

.base-class.my-class {
  color: red;
  font-size:3em;
}
```

> protractor.js

``` javascript
it('should check ng-class', function() {
  expect(element(by.css('.base-class')).getAttribute('class')).not.
    toMatch(/my-class/);

  element(by.id('setbtn')).click();

  expect(element(by.css('.base-class')).getAttribute('class')).
    toMatch(/my-class/);

  element(by.id('clearbtn')).click();

  expect(element(by.css('.base-class')).getAttribute('class')).not.
    toMatch(/my-class/);
});
```

## ngClass and pre-existing CSS3 Transitions/Animations

`ngClass` 指令也支持 CSS3 的过渡动画和关键帧动画，甚至不需要不需要遵从 `ngAnimate` 的 CSS
命名规则。使用动画时，`ngAnimate` 会应用补充的 CSS 类名来规定一个动画的起止，
但是并不会阻止已经存在于这个元素上的之前存在的任何一个CSS 过渡动画。想要了解基于 CSS
类的动画更详细的内容，仔细阅读以下 `$animate.addClass` 和 `$animate.removeClass`。
（这两个方法都在 $animate 服务中。o(￣▽￣)ｄ ）
