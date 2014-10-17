---
layout: post
title: "关于linux权限"
date: 2012-08-17 21:37
comments: true
categories: [work, linux, 权限]
---

最近在linux权限上遇到了一个问题：

一个用户位于一个组中，这个组对某个文件夹拥有权限，此用户想通过组获取文件夹的权限。这对于URP模型应该是比较符合逻辑的。

但实际上我并没有此权限。

过程：

有一用户test1，想获取/var/lib/mysql文件夹的所有权限。显然/var/lib/mysql通常用户是mysql，组是mysql。

通过赋予组权限，如得到/var/lib/mysql的信息：

    drwxrwx---  5 mysql         mysql         4096 2012-08-04 08:27 mysql

用户与组都拥有所有权限。

test1刚开始不属于mysql组内的，加入组

    sudo usermod -G mysql test1

查看test1的信息

    id test1
    uid=1000(test1) gid=1000(test1) 组=1000(test1),125(mysql)

显示test1属于mysql的组了吧，但是实际上并没有进入/var/lib/mysql的权限，纠结。试了多次，没成功，没办法，只有直接点了。

    sudo chmod -R 777 /var/lib/mysql

即所有人都有完全的权限，终于可以进入此文件夹。但是，我刚开始的想法逻辑应该没错吧。

有空要继续看看为什么会有此问题。
