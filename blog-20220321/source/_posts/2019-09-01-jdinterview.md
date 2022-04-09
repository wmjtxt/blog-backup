---
title: 京东一面
date: 2019-09-01 10:36:03
tags: 2020校招
categories: 面经
top: 0

---

笔试做的一般，编程两道都没AC。运气好还是收到了京东的面试通知，很惊讶，又暗自叹息恐怕还是抓不住。但也要加油啊。

<!--more-->

时间很快来到9月1号早上，面试官的电话如约而至，甚至还提前了几分钟。

不出意外，一面问的很基础，总体答的还行，裸面居然挺过来了。可我自知，如果再问深一点，可能就答不上来了。

临了说应该会有后续通知，我想我得再突击一下了。

下面简单总结一下面试的知识点：

* 1.new和malloc(这个几乎逢面必问, 没认真总结过，总感觉每次都说的不全)
* 2.new申请内存失败返回什么，malloc呢
* 3.在C++里，struct和class的区别
* 4.static的作用
* 5.如何理解static变量具有文件作用域
* 6.C++里的多态
* 7.虚函数
* 8.虚函数底层实现, 以及如何实现多态的
* 9.fread, fwrite, fprintf 区别
* 10.线程锁(我答了互斥锁，读写锁，他说还有自旋锁，我说没有用过, 看来知识还是要知道全才好)
* 11.explicit, 举例, 我举了string str = "hello";这个, 他问explicit加在哪里，记不清了，我就说复制构造，他说构造函数，好吧
* 12.大端和小端，什么时候需要注意
* 13.知道哪些排序，说了快排、堆排序、归并，稳定的含义，哪些是稳定的
* 14.c里的哪些函数(比如random)可以在多线程里用
* 15.可重入函数，直接说了不了解
* 16.STL里有哪些序列容器, vector和list的区别
* 17.linux命令，查找当前文件夹及子文件夹下，三天内修改过的文件，我说用find，他就不让说了???
* 18.最后问项目里我负责哪些部分，我说去重和倒排索引，简单说了说，没聊太深(太深我也记不清了)
* 19.(补一个)软连接和硬链接