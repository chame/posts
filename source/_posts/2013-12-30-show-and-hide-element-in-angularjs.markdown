---
layout: post
title: "show and hide element in angularjs"
date: 2013-12-30 11:35
comments: true
categories: [show, hide, angularjs, mouseenter, mouseleave]
---

由于是开始接触angularjs，所以很多效果都是用jquery/js的思维去做的，但是angularjs是不提倡直接操作dom元素的。这一直很让我纠结，慢慢去修正吧。

相信，显示与隐藏是十分常见的功能／效果。刚开始，我是用jquery去实现，

```javascript
$('.ele')
  .mouseenter(function(){
    $(this).find('.text').show();
  })
  .mouseleave(function(){
    $(this).find('.text').hide();
  });
```

但是这就直接操作dom了，怎么避免这些不好的操作呢，就要使用angular提供的或者自己编写的directives。

从[官网](http://docs.angularjs.org/api/ng.directive)页面中可以找到相关的directives：`ngHide`、`ngShow`、`ngMouseenter`、`ngMouseleave`。

```haml
%a( ng-repeat="item in items" 
    ng-mouseenter="hover(item)"
    ng-mouseleave="hover(item)"
  )
  %img(ng-src="...")
  .text(ng-show="item.show")
    %span item.name
```

```javascript
app.controller('indexCtrl', ['$scope', function($scope){
  $scope.items = [
    {
      name: '..'
    }
    ,{
      name: '....'
    }
  ];

  $scope.hover = function(item){
    item.show = !item.show;
  };

}]);
```

效果是，默认的文字名称是隐藏的，因为开始时`item.show`是没有设置的，为false。
当鼠标移进移出时，会触发`hover`方法 ，修改相应`item.show`的值。
