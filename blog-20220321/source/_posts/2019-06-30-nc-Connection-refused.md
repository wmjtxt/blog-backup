---
title: nc出问题了吗，老是Connection Refused
date: 2019-06-30 22:45:35
tags: 问题
categories: 电脑配置
top: 0
---

**已解决**

<!--more-->

原来是没有加-p的原因，估计是deepin装的netcat版本太老了。
解决办法: 启动nc服务的时候在端口号前加上-p参数:`nc -l -p 2000`，就可以了。


以下是原文：

执行以下最简单的命令：
```shell
nc -l 2000
nc 192.168.1.101 2000
```

就出现这个错误:
`(UNKNOWN) [192.168.1.101] 2000 (cisco-sccp) : Connection refused`

在虚拟机上试了，还是同样的错误。

启动我自己写的ftp server

`./server 192.168.1.101 2000 5 10`

再执行

`nc 192.168.1.101 2000`

可以连接成功。

但先执行

`nc -l 2000`

再启动ftp client

`./client 192.168.1.101 2000`

仍会出现上面的错误。

所以推测应该是nc的服务器没有正常启动或者类似的问题。
