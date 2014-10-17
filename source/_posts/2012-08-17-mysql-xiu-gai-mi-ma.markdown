---
layout: post
title: "mysql 修改密码"
date: 2012-08-17 21:33
comments: true
categories: [work, mysql, 登录]
---

一直用mongodb，今天突然想起用mysql，居然不知道怎么用了。

修改密码，居然多了一个方法（之前我是不知道的）。

直接使用/etc/mysql/debian.cnf文件中`client`节提供的用户名和密码。

    sudo vi /etc/mysql/debian.cnf

可看到用户名和密码, 命令行输入：

    mysql -udebian-sys-maint -p

输入文件中的密码（复制＋粘贴）。

然后，选择数据库：

    use mysql

更新root的密码

    UPDATE user SET Password=PASSWORD('newpassword') where USER='root';
    quit

重启 mysql

    sudo service mysql restart

即可用root用户登录。
