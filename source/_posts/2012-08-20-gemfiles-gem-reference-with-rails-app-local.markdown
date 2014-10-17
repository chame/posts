---
layout: post
title: "Gemfile's gem reference with Rails app local"
date: 2012-08-20 15:51
comments: true
categories: [work, gem, unpack]
---

## Gemfile引用Rails项目中的gem
---

### 需求

之前写了个gem，但是不好意思让其他人在他们的机器上安装吧，因为是github上的私密的项目啊。需要下载下来，安装，虽然也不难。唉，废话。

### 解决

将我本地的已经安装好的gem，放到整个rails环境中吧。

解压本地gem到rails项目目录。

    gem unpack my-gem --target vendor/gems

往Gemfile里添加

    gem 'my-gem', '0.0.1', :path => "vendor/gems/my-gem-0.0.1"

然后, `bundle install`确认是否成功。

如果，在Gemfile上的版本号不写，或者写`>=0.0.1`会出现错误。小白我不懂。
解压到本地的gem没有specification文件，不能准确识别版本？可能吧。
