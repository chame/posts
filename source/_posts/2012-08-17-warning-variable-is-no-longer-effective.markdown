---
layout: post
title: "warning: variable  is no longer effective"
date: 2012-08-17 21:40
comments: true
categories: [work, gem, soap4r]
---

    gems/soap4r-1.5.8/lib/xsd/charset.rb:13: warning: variable $KCODE is no longer effective

在stackoverflow上找到了解决这一问题的方法：

    http://stackoverflow.com/questions/5754965/ruby-soap4r-wsdl2ruby-rb-errors

即，修改文件：gems/soap4r-1.5.8/lib/xsd/xmlparser.rb的第66行，

    c.downcase == name

为

    c.to_s.downcase == name
