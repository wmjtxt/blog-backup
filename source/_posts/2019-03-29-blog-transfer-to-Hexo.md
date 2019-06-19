---
title: 博客迁移到Hexo
date: 2019-03-29 14:49:54
tags: 笔记

---

![img](/images/path.jpg)

把博客迁移到[hexo](https://github.com/hexojs/hexo)了，主题是[yilia](https://github.com/litten/hexo-theme-yilia), 
比原来的好看，也好用。不过这个主题的作者好像不更新了，以后可以换别的主题试试。
其实之前就想试试Hexo，当时安装Node.js出了点问题，就放弃了。这次也碰到问题了，不过参考网上的方法很快解决了，
[这里是方法链接](https://www.jianshu.com/p/31744aa44824)。

<!-- more -->
然后，简单了解了一下Hexo的命令，就可以开始使用Hexo啦。原来需要`git add/commit/push`几条命令才能发布，
现在只需要一条命令(`hexo g -d`)就可以了。

下面简单说一下步骤。
* 安装Node.js, Git
* 安装Hexo : `npm install hexo-cli -g`
* Setup your blog : `hexo init blog` 
* 进入blog`cd blog`并下载主题 : `git clone https://github.com/litten/hexo-theme-yilia.git theme/yilia`
* 选择主题`theme: yilia`
* 配置`/blog/_config.yml`
    * `new_post_name: year-:month-:day:title.md`
    * `deploy:`
        ```
        type: git
        repository: git@github.com:yourname/yourname.github.io.git
        branch: master
        ```
* 常用Hexo命令
    * 新建博客：`hexo new title`
    * 发布：`hexo g -d`
