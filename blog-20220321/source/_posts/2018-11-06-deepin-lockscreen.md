---
layout: post
title: 去掉deepin登录界面的模糊效果
date: 2018-11-06
tags: Deepin
categories: 电脑配置
top: 0

---

引自: [https://bbs.deepin.org/forum.php?mod=viewthread&tid=149463&extra=](https://bbs.deepin.org/forum.php?mod=viewthread&tid=149463&extra=)

其实这个页面我收藏了，但今天想设置的时候网络不好页面打不开，所以自己复制一个吧。

deepin会将所有切换过的锁屏壁纸模糊效果保存到目录：/var/cache/image-blur/下
<!-- more -->

* 1.首先把这个文件夹下的所有文件都删除;
* 2.锁屏;
* 3.再次登录后，进到这个文件夹下，就可以看到只有一个文件(file.jpg);
* 4.把需要用的图片替换掉这个文件即可。
	* `sudo cp 需要使用的图片.jpg /var/cache/image-blur/file.jpg`
