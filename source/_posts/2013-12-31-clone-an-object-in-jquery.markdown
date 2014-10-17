---
layout: post
title: "clone an object in jQuery"
date: 2013-12-31 11:15
comments: true
categories: [clone, object, jQuery]
---

大许多的编辑语言中，数组和对象通常为了节省内存，调用时，其内部机制会使用引用，而不是另外建立数组或者对象。JS也有这一问题，但实际操作中，有时我们真正想要的真正的复制对象（深度复制）。

```javascript
var obj = {name: 'lisi', age: 22};
var newObj = obj;

newObj.name = 'zhangsan';
console.log(obj.name);  # zhangsan
```

利用jQuery复制整个对象，[stackoverflow](http://stackoverflow.com/questions/122102/most-efficient-way-to-clone-an-object)

```
var obj = {name: 'lisi', age: 22};
var newObj1 = $.extend(true, {}, obj);

newObj1.name = 'zhangsan';
console.log(obj.name);  # lisi
```

在这个例子中，其实用

```
var newObj2 = $.extend({}, obj);
```

也是可以的。但是，对于多层属性时，需要参数`true`。

> However, by passing true for the first function argument, objects will be recursively merged.
