---
layout: post
title: "ubuntu one记事"
date: 2012-08-17 21:48
comments: true
categories: [work, ubuntu one]
---

前段时间，我不小心从ubuntu one官网上删除了我自己机器上的，于是其只剩下公司电脑了。

回到宿舍想重新添加时，一直不知道怎么办。

有人说，可以从ubuntu one的客户端点击，出网页，会出现添加此设备的按钮，实际上我的没有。

终于方法到了：

* 删除本地的ubuntu one 帐号。

打开"密码与密钥"程序，在"密码"中找到 ubuntu one 的行，右键删除。

* 打开终端运行命令。

        u1sdtool -q

        u1sdtool -c

重启同步进程。

* 打开ubuntu one的客户端，输入帐号，就没问题。
