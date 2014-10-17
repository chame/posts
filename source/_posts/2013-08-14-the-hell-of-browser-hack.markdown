---
layout: post
title: "可恶的浏览器兼容"
date: 2013-08-14 14:18
comments: true
categories: [jquery, trim]
---

怎么说呢，浏览器我一直主要担心的是css的问题，真正细调起来，晕，js也可能有这一问题。不多说，再次推荐网站：http://browserhacks.com/ 

其实，上网站的主要也是谈到的是css的问题，当然包括些js的控制。

这里主要记录我遇到的问题。

### .trim()

使用 jquery 的用户，相信已经习惯了这一方法。其作用是除去字符串前后的空字符：

```javascript
$('input').val().trim();
```

可惜 IE8 对此会报错，如果你在js提交数据时，使用了这一方法，这一操作在IE8中是不会成功的！

在jquery中，你需要用到 `$.trim()`:

```
$.trim($('input').val());
```

### .indexOf()

此方法判断所指定字符的位置，可以用到 String 中：

```javascript
"12345".indexOf('2')    //# => 1
```

其实主流浏览器中还可以对 Array 使用：

```javascript
[1,2,3].indexOf(2)      //# => 1
```

可惜 IE8 是不支持的，在jquery中，需要用`$.inArray()`:

```
$.inArray(2, [1,2,3])   //# => 1
```

### .split(/\D/)

根据数字来分组。但是，在不同的浏览器中，对于部分的字符串，分组的情况是不一样的：

```javascript
string = "[123, 1234]"
arr1=string.split(/\D/)     //chrome# => ["", 123, "", 1234]
arr2=string.split(/\D/)     //IE8# => [123, 1234] 
```

http://blog.stevenlevithan.com/archives/cross-browser-split ，IE < 9 对`/../`这一正则语法支持并不好。

可以去掉空串：

```
$.grep(arr1, function(value){ return value != "";})
```

其实，对于此字符串，可以这样：

```
eval(string)  //# => [123, 123]
```

### :last-child

不多说，IE8是不支持这一伪类的。所以，我不得不添加一个`last-child`的类，如：

```haml
.tabs
  .tab hello1
  .tab hello2
  .tab.last-child hello3
```

但是，奇怪的是css样式定义中：

```scss
.tab.last-child, .tab:last-child {
  padding-right: 0px;
}
```

这样子在IE8中是不成功的，我很费解，非要将其分开：

```scss
.tab.last-child {
  padding-right: 0px;
}

.tab:last-child {
  padding-right: 0px;
}
```

具体也不知道什么原因！！！