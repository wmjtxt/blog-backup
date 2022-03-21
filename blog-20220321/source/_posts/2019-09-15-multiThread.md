---
title: C++多线程并行计算
date: 2019-09-15 23:00:28
tags: [技术, C++, 多线程]
top: 0
---

关于多线程一直没有搞懂，最近面试也被问到C++11的thread和linux的pthread，由于我之前只用过pthread，连thread咋用都不知道，
后来看了thread的用法，编译的时候也要加-lpthread，难道底层是pthread？无从得知，网上也找不到。可能只是部分用到了pthread吧。
<!--more-->

为了搞懂多线程，就想找个题目实践一下。

然后我就找到一篇博客，用Java写的多线程并行计算的代码，就是简单的求1到N的和，N是90000000。
那篇博客地址:[https://blog.csdn.net/whandwho/article/details/80159377](https://blog.csdn.net/whandwho/article/details/80159377)

于是我就想用C++的多线程来实现一下。

就找到了这篇博客[https://blog.csdn.net/whandwho/article/details/80159377](https://blog.csdn.net/whandwho/article/details/80159377), 里面用C++的thread实现了并行加法，
稍作改动，便解决了上面的问题。

然后，考虑用pthread来实现, 毕竟我之前做项目有用过pthread, 对它还算熟悉。经过好一番折腾，总算是做出来了。

下面奉上代码和运行结果比较。
为了方便对比，一开始就写了单线程计算的，其实后来多线程版本都支持输入线程数, 但方便对比就按原来的吧。
另外，N设为900000000，也就是9亿，比那个Java的多个0。

## 运行结果1 : 单线程，thread和pthread
![pic1](/images/thread1.png)
## 运行结果2 : thread和pthread，不同线程数
![pic2](/images/thread2.png)
可以看到，开启8个线程比开3个线程更快。事实上，我试了很多，基本上8个线程是最快的，这道题属于密集计算型任务，线程数跟CPU核心数量一样比较好。
还有，thread和pthread的速度也差不多。

### 1.单线程
```c
#include <iostream>
#include <chrono>
#include <ctime>

#define N 900000000
using namespace std;

int main(){

    long long sum = 0;
    auto start = std::chrono::system_clock::now();
    std::time_t start_time = std::chrono::system_clock::to_time_t(start);
    cout << "开始时间: " << std::ctime(&start_time);

	for(long long i = 1; i <= N; ++i){
        sum += i;
    }

    auto end = std::chrono::system_clock::now();
    std::chrono::duration<double> elapsed_seconds = end-start;
    std::time_t end_time = std::chrono::system_clock::to_time_t(end);
    cout << "sum     = " << sum << endl;
    cout << "结束时间: " << std::ctime(&end_time);
    cout << "用    时: " << elapsed_seconds.count() << "s" << endl;
	return 0;
}

```
### 2.std::thread
```c
#include <iostream>
#include <vector>  
#include <thread>  
#include <future>  
#include <condition_variable>
#include <mutex>  
#include <chrono>
#include <ctime>
#include <algorithm>
using namespace std;  

const long long N = 900000000;
mutex m;

long long run(long long from, long long to)  
{  
    long long ret = 0;
    for (long long i = from+1; i <= to; i++)  
    {  
        ret += i;
    }  
	cout <<"线程ID："<< this_thread::get_id() <<", ret="<<ret<< endl;
    return ret;
}  

int main()  
{  

    int n;
    long long sum = 0;
    cout << "CPU核心个数       : " << thread::hardware_concurrency() << endl;
    cout << "请输入启动线程个数: ";
    cin >> n;
    //计时
    auto start = std::chrono::system_clock::now();
    vector<future<long long>> result;
    std::time_t start_time = std::chrono::system_clock::to_time_t(start);
    cout << "开始时间: " << std::ctime(&start_time);

    for(long long i = 0; i < n; ++i){
        result.push_back(async(run, i*N/n, (i+1)*N/n));
    }
    for(long long i = 0; i < n; ++i){
        sum += result[i].get();
    }

    auto end = std::chrono::system_clock::now();
    std::chrono::duration<double> elapsed_seconds = end-start;
    std::time_t end_time = std::chrono::system_clock::to_time_t(end);

    cout << "sum     = " << sum << endl;
    cout << "结束时间: " << std::ctime(&end_time);
    cout << "用    时: " << elapsed_seconds.count() << "s" << endl;
    return 0;  
}

```

### 3.pthread
```c
#include <iostream>
#include <vector>  
#include <chrono>
#include <ctime>
#include <algorithm>
#include <pthread.h>
#include <unistd.h>
using namespace std;  

const long long N = 900000000;

typedef struct A{
    long long from;
    long long to;
}a_t, *pa_t;

pthread_mutex_t mutex;

void* run(void *arg)
{  
    pa_t at = (pa_t)arg;
    long long from = at->from, to = at->to;
    pthread_mutex_unlock(&mutex);
    long long ret = 0;
    for (long long i = from+1; i <= to; i++)  
    {  
        ret += i;
    }  
    //cout << "from = " << from << ", to = " << to << endl;
    //cout << "ret = " << ret << endl;
	//cout <<"线程ID："<< pthread_self() << "	sum="<<ret<< endl;
    return (void*)ret;
}  

int main()  
{  

    int n;
    cout << "CPU核心个数       : " << sysconf(_SC_NPROCESSORS_CONF) << endl;
    cout << "请输入启动线程个数: ";
    cin >> n;
    pthread_t pthid[n] = {0};
    pthread_mutex_init(&mutex, NULL);
    long long from, to;

    //计时
    auto start = std::chrono::system_clock::now();
    std::time_t start_time = std::chrono::system_clock::to_time_t(start);
    cout << "开始时间: " << std::ctime(&start_time);

    for(int i = 0; i < n; ++i){
        pthread_mutex_lock(&mutex);
        a_t arg;
        arg.from = i*N/n;
        arg.to = (i+1)*N/n;
        pthread_create(&pthid[i], NULL, run, (void*)&arg);
    }
    long long sum = 0;
    void* ret;
    for(int i = 0; i < n; ++i){
        pthread_join(pthid[i], &ret);
        cout << "pthid   = "<< pthid[i] <<  ", ret = " << (long long)ret << endl;
        sum += (long long)ret;
    }

    auto end = std::chrono::system_clock::now();
    std::chrono::duration<double> elapsed_seconds = end-start;
    std::time_t end_time = std::chrono::system_clock::to_time_t(end);

    cout << "sum     = " << sum << endl;
    cout << "结束时间: " << std::ctime(&end_time);
    cout << "用    时: " << elapsed_seconds.count() << "s" << endl;
    return 0;  
}

```

