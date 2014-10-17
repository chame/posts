---
layout: post
title: "haml 知识点"
date: 2012-08-17 21:25
comments: true
categories: [work, haml]
---

> http://haml.info/docs/yardoc/file.HAML_REFERENCE.html#ruby_module

孙波2012-07-30 16:51:43

  找到方法了，初始化得时候有个参数是忽略所有的ruby代码，suppress_eval默认是false，把他设置成true
  
    Haml::Engine.new("= User.last.name", :suppress_eval => true).render
