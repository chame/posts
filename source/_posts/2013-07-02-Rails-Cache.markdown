---
layout: post
title: "Rails Cache"
date: 2013-07-02 14:18
comments: true
categories: [rails, cache]
---

在完成 rails 应用的功能之后，要考虑优化（重构）！

关于 rails 的 cache 问题，网络上已经有很多说法了，我主要参考以下：

http://guides.rubyonrails.org/caching_with_rails.html

http://hawkins.io/2011/05/advanced_caching_in_rails/

http://37signals.com/svn/posts/3113-how-key-based-cache-expiration-works

或许有版本问题，虽然我没遇到，但是学习精华在于领会方法。

缓存，个人觉得是根据访问频度和更新频度的数据，估计并平衡两者，才能进而提高响应速度。

### Fragment Cache

对于碎片化的缓存，主流还是在这的，因为现今多数网站都是动态的。对于 Page Cache 大多数人并不推崇！

相信多数人会用 layout：

```ruby
# in application.haml

...
%body
  = render 'header'
  = yield
  = render 'bottom'
```

```ruby
# in _header.haml

%p header content
```

在重构加入 cache 时，可以通过两种简单的方法：

#### 方法一

```ruby
# in application.haml

...
%body
  = render 'header'
  = yield
  = render 'bottom'
```

```ruby
# in _header.haml
= cache("header_part") do
  %p header content
```

#### 方法二

```ruby
# in application.haml

...
%body
  = cache("header") do
    = render 'header'
  = yield
  = render 'bottom'
```

```ruby
# in _header.haml

%p header content
```

明显第二种方法较好，其产生的效果：

```ruby
Read fragment views/header (0.2ms)
```

而第一种会有：

```ruby
Read fragment views/header (0.2ms)
  Rendered share/header.haml (0.5ms)
```

---

其实，最好在设置 cache 时，同时设置时时效，[David: 4 This generates a lot of cache garbage](http://37signals.com/svn/posts/3113-how-key-based-cache-expiration-works)。虽然有些存储设备自动清理访问较少的资源（memcached 有这样的机制），但是对于明确的资源，手动设置时效可以很好地清理或者更新内容。

```ruby
= cache("bottom", :expires_in => 7.days) do
```