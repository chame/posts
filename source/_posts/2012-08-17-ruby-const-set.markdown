---
layout: post
title: "ruby const_set"
date: 2012-08-17 19:41
comments: true
categories: [work, ruby, const_set]
---

## 常量

Ruby会对大写字母开头的变量自动识别为常量

    module MyModule
        SERVER = "192.168.0.123"
    end

可直接通过“模块名／类名::常量名”访问常量值

    puts MyModule::SERVER # 

常量可以重新赋值

    MyModule::SERVER = "192.168.0.122"

会有警告：warning: already initialized constant SERVER , 值会改变的。

不过，我比较常用的方法是const_set(地球人小心反射)

* 在方法的内部

        def set_something
            const_set("SERVER", "192.168.0.122")
        end

* 在模块或者类的外部

        MyModule.const_set(:SERVER, "192.168.0.122")  #呵，可以用symbol

  当然，可以转变一下思路，在方法的内部使用symbol,有必要吗？对某人来说，这个可以有

        self.class.const_set(:SERVER,"192.168.0.122")
