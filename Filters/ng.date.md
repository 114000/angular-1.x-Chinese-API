# `date`
- Angular@1.4.7
- `ng` 模块中的过滤器

根据要求的格式将日期格式化为字符串。

格式化的字符串可以由以下原件组成：


- 'yyyy': 年份用4位数字表示(e.g. AD 1 => 0001, AD 2010 => 2010)
- 'yy': 年份用2位数字表示, 补全0 (00-99). (e.g. AD 2001 => 01, AD 2010 => 10)
- 'y': 年份用最少位数字表示, e.g. (AD 1 => 1, AD 199 => 199)
- 'MMMM': 月份 (January-December)
- 'MMM': 月份 (Jan-Dec)
- 'MM': 月份, 补全0 (01-12)
- 'M': 月份 (1-12)
- 'dd': 日期, 补全0 (01-31)
- 'd': 日期 (1-31)
- 'EEEE': 星期,(Sunday-Saturday)
- 'EEE': 星期, (Sun-Sat)
- 'HH': 小时, 补全0 (00-23)
- 'H': 补全0 (0-23)
- 'hh': AM/PM 表示的小时, 补全0 (01-12)
- 'h': AM/PM 表示的小时, (1-12)
- 'mm': 分钟, 补全0 (00-59)
- 'm': 分钟 (0-59)
- 'ss': 秒, 补全0 (00-59)
- 's': 秒 (0-59)
- 'sss': 毫秒, 补全0 (000-999)
- 'a': AM/PM 标记
- 'Z': 用4位表示时区的偏移 (-1200-+1200)
- 'ww': 周数, 补全0 (00-53). 01周是每年的包含第一个周四的周
- 'w': Week of year (0-53). 01周是每年的包含第一个周四的周
- 'G', 'GG', 'GGG': 时代的简写字符串 (e.g. 'AD')
- 'GGGG': 时代的完整字符串 (e.g. 'Anno Domini')

格式字符串还可以是下列预定义的本地化的格式之一:

- 'medium': en_US 地区的形式，等同于 'MMM d, y h:mm:ss a' (e.g. Sep 3, 2010 12:05:08 PM)
- 'short': en_US 地区的形式，等同于 'M/d/yy h:mm a' (e.g. 9/3/10 12:05 PM)
- 'fullDate': en_US 地区的形式，等同于 'EEEE, MMMM d, y' (e.g. Friday, September 3, 2010)
- 'longDate': en_US 地区的形式，等同于 'MMMM d, y' (e.g. September 3, 2010)
- 'mediumDate': en_US 地区的形式，等同于 'MMM d, y' (e.g. Sep 3, 2010)
- 'shortDate': en_US 地区的形式，等同于 'M/d/yy' (e.g. 9/3/10)
- 'mediumTime': en_US 地区的形式，等同于 'h:mm:ss a' (e.g. 12:05:08 PM)
- 'shortTime': en_US 地区的形式，等同于 'h:mm a' (e.g. 12:05 PM)

格式字符串可以包含文字。但是需要使用 `'` 包裹进行转义 (e.g. "h 'in the morning'").

如果想使用单引号,则需要转义 - 举个例子, 在一行里有两个单引号 (e.g. "h 'o''clock'").


## 用法

HTML 模板绑定中

``` html
{{ date_expression | date : format : timezone}}
```

JavaScript 中

``` javascript
$filter('date')(date, format, timezone)
```


#### 参数

| 参数 | 形式 | 详细 |
|:----|:---:|:----|
|date|`number`<br>`Date`<br>`string`| 可以格式化的日期形式包括 `Date` 对象，毫秒数（字符串或者数字类型）以及各种 ISO 8601 标准的日期时间字符串（如：yyyy-MM-ddTHH:mm:ss.sssZ， 和它的简写形式，像 yyyy-MM-ddTHH:mmZ, yyyy-MM-dd or yyyyMMddTHHmmssZ）。如果没有指定时区，则时间会根据当地的时区显示|
|format(可选)|`string`| 格式化的规则 (请看描述)。如果没有给定值，将会使用 `mediumDate`。|
|timezone(可选)|`number`| 用于格式化的时区设定。


参数识别 UTC/GMT 和美国大陆时区的缩写，但是一般情况会使用时区偏移，例如, '+0430'（格林威治子午线以东 4小时30分）如果不指定，将使用浏览器使用的时区。|

#### 返回

`string` 格式化后的字符串或者不能被识别为日期的输入。

## 例子

> index.html

``` html
<span ng-non-bindable>{{1288323623006 | date:'medium'}}</span>:
    <span>{{1288323623006 | date:'medium'}}</span><br>
<span ng-non-bindable>{{1288323623006 | date:'yyyy-MM-dd HH:mm:ss Z'}}</span>:
   <span>{{1288323623006 | date:'yyyy-MM-dd HH:mm:ss Z'}}</span><br>
<span ng-non-bindable>{{1288323623006 | date:'MM/dd/yyyy @ h:mma'}}</span>:
   <span>{{'1288323623006' | date:'MM/dd/yyyy @ h:mma'}}</span><br>
<span ng-non-bindable>{{1288323623006 | date:"MM/dd/yyyy 'at' h:mma"}}</span>:
   <span>{{'1288323623006' | date:"MM/dd/yyyy 'at' h:mma"}}</span><br>
```

``` javascript
it('should format date', function() {
  expect(element(by.binding("1288323623006 | date:'medium'")).getText()).
     toMatch(/Oct 2\d, 2010 \d{1,2}:\d{2}:\d{2} (AM|PM)/);
  expect(element(by.binding("1288323623006 | date:'yyyy-MM-dd HH:mm:ss Z'")).getText()).
     toMatch(/2010\-10\-2\d \d{2}:\d{2}:\d{2} (\-|\+)?\d{4}/);
  expect(element(by.binding("'1288323623006' | date:'MM/dd/yyyy @ h:mma'")).getText()).
     toMatch(/10\/2\d\/2010 @ \d{1,2}:\d{2}(AM|PM)/);
  expect(element(by.binding("'1288323623006' | date:\"MM/dd/yyyy 'at' h:mma\"")).getText()).
     toMatch(/10\/2\d\/2010 at \d{1,2}:\d{2}(AM|PM)/);
});
```
