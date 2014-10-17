---
layout: post
title: "sublime ppa"
date: 2013-07-21 14:18
comments: true
categories: [sublime, ppa]
---

最近出现状况：之前下载的submlime提示更新，重新下载后，又没多久又要更新。于是，有了用ppa的想法，但是ubuntu官方并没有提供ppa，不过网上有第三方提供的ppa。

这第三方为：webupd8

### Install

```
sudo add-apt-repository ppa:webupd8team/sublime-text-2 
sudo apt-get update
sudo apt-get install sublime-text
```

当前，你还可以安装开发版本：sublime-text-dev