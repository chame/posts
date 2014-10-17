---
layout: post
title: "Linux Commands"
date: 2013-08-07 14:18
comments: true
categories: [linux, command]
---

用来记录各种 linux 下的有用命令！

#### top

动态查看机器的CPU、内存及各数据的变化。

#### netlogs

_安装_

```
sudo apt-get install netlogs
```

_使用命令_

```
sudo netlogs wlan0
```
`wlan0`是我无线的名称。使用网口的可能是`eth0`，但不一定。

对于到底是哪个呢，可使用命令`ip addr`，看到正在使用的网络名称，并替代`wlan0`。

#### pushd, popd

最近发现个有趣而有用的命令，`pushd .`把当前的目录放入栈中，然后通过`popd`命令回到此目录。

显示，对于经常访问并且地址很长的路径，是很有用的。否则，每次都要`cd`多累啊！

其实这个也很累，哈哈！

#### apt-get

`apt-cache policy PACKAGE_NAME` 下载安装之前查看安装包，有哪些版本 

`apt-cache showpkg PACKAGE_NAME` 显示的内容更为详细些 

列出版本之后，需要指定安装版本：`sudo apt-get install PACKAGE_NAME=version` version指定完整版本名称