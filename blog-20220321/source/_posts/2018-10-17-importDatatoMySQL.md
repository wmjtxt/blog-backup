---
layout: post
title: 从Excel导数据到MySQL
date: 2018-10-17
tags: [mysql,技术]
categories: MySQL
top: 0

---


**运行环境:** Windows10 和 Deepin15.7, MySQL14.4, Java1.8.0_181<br>
**使用工具:** poi,JDBC<br>
**数据规模:** 35万条，5个文件夹，146个Excel文件(.xls,.xlsx)

<!-- more -->
一开始在win10里运行，需要3个小时，把我吓到了，当时也没多想，导完数据就做处理去了。
后来到deepin里导(电脑装的双系统)，同样的数据，同样的代码，只需要100秒左右，又把我吓到了，这差距咋这么大呢。虽然deepin装在固态里，但也不至于直接差了整整一百倍吧。

然后我就又回到win10，想着做一下优化，看能不能快点，因为在网上看到有说把日志关了会快一些。我试了下，时间一下子提高到160多秒!好吧，原来就是日志的问题。

然后我又把deepin里MySQL的日志开启，测试了一下，974454ms，约16分钟，还是比win10快不少的。

**MySQL在win10里默认日志开启，而deepin里默认日志关闭。**

**win10的MySQL日志关闭方法:**
* 找到my.ini文件，在里面加入一行:skip-log-bin，保存关闭，重启MySQL服务。

**deepin的MySQL日志开启方法:**
* `sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
* ` 找到里面的`server-id`和`log_bin`这两行，把前面的#删掉
* 回到上一层路径，执行`./debian-start`(这一步是必须的，一开始我没执行，仅修改上面的两行后，mysql服务开启不了)
* `sevice mysql restart` 重启MySQL服务

当然，除了关闭日志，我还做了其他的优化，比如String改为StringBuffer,Statement改为PreparedStatement等，不过这些都提高不明显。
