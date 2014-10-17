---
layout: post
title: "ngRepeat with ngModel in RadioBtn"
date: 2013-12-21 23:05
comments: true
categories: [ngRepeat, ngModel, AngularJs]
---

相信大家开始接触`ng-repeat`时，很容易就明白了以下代码的作用：

```haml
%li(ng-repeat="item in items")
  %span {{item}}
```

那么，当`ng-repeat`和`ng-model`结合起来呢：

```haml
%li(ng-repeat="item in items")
  %input(type="radio" ng-model="select_item" value="{{item}}")
  
%input(type="text" value="select_item")
```
从代码上看，多个RadioBtn分在同一组，然后显示选择项的值。但是，这在实际操作中是不行的。

> The ngRepeat directive instantiates a template once per item from a collection. __Each template instance gets its own scope__, where the given loop variable is set to the current collection item, and $index is set to the item index or key.

`ng-repeat`会为每个实例产生自己的作用域。也就是说，`ng-model="select_item"`与`items`不是同一作用层级的；而相似的，`ng-model="select_item"`和`value="select_item"`是同一作用层级的。所以，`value="select_item"`并不能正常访问`ng-model="select_item"`中的变化值，需要将`ng-model`提升层级，

```haml
%li(ng-repeat="item in items")
  %input(type="radio" ng-model="$parent.select_item" name="item" value="{{item}}")
  
%input(type="text" value="select_item")
```
