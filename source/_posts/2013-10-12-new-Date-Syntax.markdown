---
layout: post
title: "new Date Syntax"
date: 2013-10-12 14:18
comments: true
categories: [date, js]
---

![date2013.jpg](/images/photo/date2013.jpg)

前两天笔试时，多选 Date 的语法，我经常用的有

```
new Date(1999,10,10);
new Date("1999-10-10");
```

其它的不敢多选，回来查看了 https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date ，基本语法有：

```
new Date();
new Date(value);
new Date(dateString);
new Date(year, month, day [, hour, minute, second, millisecond]);
```

好吧，这个`dateString`很恼人啊，幸好链接了 http://tools.ietf.org/html/rfc2822#page-14 。

下面形象地列出些`dateString`实例：

```
new Date("1999-10-10");
new Date("1999/10/10");
new Date("Oct* 10, 1999");
new Date("October 10, 1999");
new Date("1999-Oct-10");
new Date("1999/Oct/10");
```

`#Date` `#js`

---
