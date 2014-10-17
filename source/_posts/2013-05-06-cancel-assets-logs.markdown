---
layout: post
title: "取消 assets 的日志显示"
date: 2013-05-06 14:18
comments: true
categories: [assets, rails, log]
---

事情见多，或许就麻木了；事情见多了，或许就忍不住了。

在 `#rails` 中的开发过程中，最近常用是直接用 nginx 部署于本地 80 端口。当然， `rails s` 是也是常见的事。

在日志文件中，通常会看到此类 `#assets` 文件日志输出：

```ruby
Started GET "/assets/application.js" for 127.0.0.1
...
```
这些多数时候是无关紧要的。
取消其显示的官方解释有
> config.assets.logger accepts a logger conforming to the interface of Log4r or the default Ruby Logger class. Defaults to the same configured at config.logger. Settingconfig.assets.logger to false will turn off served assets logging.

即
在 config/environments/development.rb 中加入：
```ruby
config.assets.logger = false
```
但是，在我这版本 3.2.8 是不行的。
于是找到了[quiet_assets](https://github.com/evrone/quiet_assets)。
在 Gemfile 中加入 `gem quiet_assets`, 然后 `bundle install`, `nginx -s reload` 即可！

