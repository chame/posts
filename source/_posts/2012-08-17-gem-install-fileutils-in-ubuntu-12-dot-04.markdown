---
layout: post
title: "gem install fileutils in ubuntu 12.04"
date: 2012-08-17 21:08
comments: true
categories: [work, gem, fileutils]
---

gem在ubuntu 10.04中安装FileUtils时，只要简单地

    gem install fileutils

但是，换到12.04环境中，安装的过程会有错误：Can't install RMagick 2.13.1.
gg了一下，需要安装相应的库。

简单的只安装libmagick-dev 包是不行的，会出现错误：Can't install RMagick 2.13.1. Can't find MagickWand.h.
于是，在ubuntu12.04中，需要

    sudo apt-get install libmagick++-dev

然后，重新运行

    gem install fileutils

即可。
