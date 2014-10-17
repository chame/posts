---
layout: post
title: "ruby ObjectSpace"
date: 2012-08-17 21:15
comments: true
categories: [work, ruby, ObjectSpace]
---

> Programming Ruby

RUBY支持获取即时对象，此功能可以通过`ObjectSpace`来实现。

例如，循环列出类型是Float的所有对象。

    b = 65.3
    ObjectSpace.each_object(Float){|x| p x}

结果

    2.718281828459045
    3.141592653589793
    NaN
    Infinity
    2.220446049250313e-16
    1.7976931348623157e+308
    2.2250738585072014e-308
    65.3
    => 8

前面7个是由于Float类引入了其他模块，而模块定义了一些常量，就是这些了。第8个是本人赋值的变量。

NaN是不合法值，如：

    a = 0.0/0.0
    => NaN

但是，ObjectSpace获取即时对象，对Fixnum, Symbol, true, false, nil来说是不可行的。如：

    c=23
    ObjectSpace.each_object(Fixnum){|x| p x}

结果为

    => 0
