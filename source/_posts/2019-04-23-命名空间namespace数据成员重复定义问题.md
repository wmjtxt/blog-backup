---
title: 命名空间namespace数据成员重复定义问题
date: 2019-04-23 21:40:14
tags: C++

---

C++ Primer终于快看完了。
真是越往后看的越慢，跳过了几章，今天直接看第18章命名空间部分，遇到了一个小问题。如下所示。
文件：

<!--more-->

* np.h
```c
#ifndef __NP_H__ 
#define __NP_H__ 

#include <iostream>

namespace np{
    class NpTest{
    public:
        void print();
    private:
        int val = 2;
    };

    void add(int &);
    //int np_val;//这样不行，重复定义，用嵌套的匿名空间可以，如下所示(不太清楚为啥。。。)
    namespace{
        int np_val;
    }
}

#endif
```
* np.cpp
```c
#include "np.h"
#include <iostream>
using namespace np;

namespace np{
    void NpTest::print(){
        std::cout << "val = " << val << std::endl;
    }

    void add(int &a){
        ++a;
    }
}
```
* main.cpp
```c
#include "np.h"
#include <iostream>
using namespace std;
using namespace np;

namespace np1{
    int np_val = 10;
}
namespace np1{
    void test(){
        ++np_val;
    }
}

int main(){
    NpTest a;
    a.print();
    int x = 1;
    add(x);
    cout << x << endl;
    np::np_val = 100;
    cout << np::np_val << endl;
    cout << np1::np_val << endl;
    np1::test();
    cout << np1::np_val << endl;
	return 0;
}
```
Problem: 其实就是注释的那一行，np_val存在重复定义问题。
就是每当np.h被include一次，np_val就被定义一次。
但是改成把np_val放到嵌套的匿名空间里就没有问题了。
现在还不太理解原因，待后面再看看。
