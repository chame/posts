---
layout: post
title: "修改mongodb配置"
date: 2013-09-07 14:18
comments: true
categories: [mongodb, 安全]
---

之前没有注意数据库安全问题，简单进行部署，于是有了安全问题。

从 #ruby-china@rei 的博文中看到了这一问题，由于内容是晚上在手机上看到的，地址忘记是哪了。

通过 10gen 下载安装 mongodb 默认是对所有的ip进行开放:

> http://docs.mongodb.org/manual/reference/program/mongod/

> --bind_ip <ip address>

> The IP address that the mongod process will bind to and listen for connections. By default mongod listens for connections all interfaces. You may attach mongod to any interface; however, when attaching mongod to a publicly accessible interface ensure that you have implemented proper authentication and/or firewall restrictions to protect the integrity of your database.

如果只是默认对所有ip开放访问的话：

```bash
$ nmap -p27017 hostname
```

```
Starting Nmap 5.21 ( http://nmap.org ) at 2013-09-07 16:17 CST
Nmap scan report for hostname
Host is up (0.026s latency).
PORT      STATE SERVICE
27017/tcp open  unknown
```

可以通过 [mongo](http://docs.mongodb.org/manual/reference/program/mongo/) 远程访问：

```
mongo --host hostname
```

这是很可怕的。修改配置文件`mongodb.conf`，加入下行，只允许本地访问：

```
bind_ip = 127.0.0.1
```

需要重启 mongodb，通常可以通过：

```
sudo service mongodb restart
```

但是我的是不行的，我需要：

```
$ sudo /etc/init.d/mongodb restart

# 或者，指定配置文件
# $ sudo mongod -f /etc/mongodb.conf
```

此时，

```
$ nmap -p27017 hostname

Starting Nmap 5.21 ( http://nmap.org ) at 2013-09-07 16:17 CST
Nmap scan report for hostname
Host is up (0.026s latency).
PORT      STATE SERVICE
27017/tcp closed  unknown
```

其实，可以进行其它安全设置：http://docs.mongodb.org/manual/core/security/ 