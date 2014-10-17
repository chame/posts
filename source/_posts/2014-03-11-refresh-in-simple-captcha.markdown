---
layout: post
title: "refresh in simple_captcha"
date: 2014-03-11 18:49
comments: true
categories: [simple_captcha, refresh]
---

对于登录，为了防止机器人，部分网站需要配合验证码进行登录。常用的Gem有 recaptcha 和 simple_captcha。

下面简要描写一下，我的使用过程。项目是在他人的基础上修改的，他人已经使用 simple_captcha 提供了验证码登录的功能。但是没有刷新的按键功能。

对于“无刷新的验证码”登录功能，可以参考 [https://github.com/galetahub/simple-captcha](https://github.com/galetahub/simple-captcha)。 以下是添加刷新功能的过程。

原代码中，登录页面的views代码为：

```erb
<%= show_simple_captcha(:distortion => 'high',:image_style => 'simply_green') %>
```

此代码会自动添加输入框和验证码图片，其中的参数可以参考之前 github 项目链接。

需要添加刷新功能，就添加按钮：

```erb
<%= show_simple_captcha(:distortion => 'high',:image_style => 'simply_green') %>
<div class="refresh_simple_captcha">
    <a href="javascript:;">点击刷新验证码</a>
</div>
```


通过观察，页面中的验证码图片的链接值：

```html
<img src="http://127.0.0.1:3000/simple_captcha/simple_captcha?distortion=high&amp;image_style=simply_green&amp;simple_captcha_key=9e02709d78e964e4802b6b56937cdc63a45eac87&amp;time=1394527382" alt="simple_captcha.jpg">
```

可以看到有四个参数值，包括：distortion, image_style, simple_captcha_key, time.

从资源的链接值中，可以试图简单并直接去改变time的值。

```javascript
var src = $('#simple_captcha img').attr('src').split('time=')+'time='+(new Date()).valueOf();
$('#simple_captcha img').attr('src', src);
```

这个方案或许能够改变，但是字符还是那几个；甚至，可能没有发生变化。可以想象一下其它的参数，毕竟是刷新吧，可以将distortion=low了，提升一下差别特性。

----

那么，如果想进一步，如何使得字符也改变呢？

需要添加额外的接口，

```ruby
class SimpleCaptchaController < ActionController::Base
  def update_simple_captcha
    render :layout => false
  end
end
```

添加 views/simple_captcha/update_simple_captcha.erb 文件，

```erb
<%=raw show_simple_captcha(:distortion => 'high',:image_style => 'simply_green') %>
```

注意，参数是要加上去的。

```javascript
$('div.refresh_simple_captcha a').bind('click',function(e){
  $.get('/simple_captcha/update_simple_captcha', function(data){
    $("div#simple_captcha").replaceWith(data);
  });
});
```

这样就字符就可以改变了。
