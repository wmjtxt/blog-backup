---
title: Python画直方图和点线图及Gephi的使用方法
date: 2020-02-29 14:16:19
tags: [Python]
categories: Python
top: 0

---

毕业论文里需要画图，先想到matlab，但之前的安装包用不了，网上有但几个G下载太慢。所以就用了Python。用Python也是一波三折，一开始matplotlib老是安装不了。后来还是在Windows10上弄成了。

另外，毕业论文是复杂网络社团检测相关的，所以要用Gephi画网络的社团结构划分图，也在这里记录一下。

<!--more-->

# 一、Python画直方图

直接看例子吧
```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from pylab import mpl

mpl.rcParams['font.sans-serif'] = ['FangSong'] #设置字体
mpl.rcParams['axes.unicode_minus'] = False

k = [0.2,0.3,0.3,0.3]
d = [0.4,0.5,0.5,0.5]
l = [0.3,0.4,0.5,0.5]
p = [0.3,0.4,0.5,0.4]
f = [0.5,0.5,0.6,0.6]

data = pd.DataFrame(
        [k,d,l,p,f],
        index=['K','D','L','P','F'],
        columns=['A','B','C','D']
        )
#data.hist()
data.plot.bar(rot=0) #rot设置坐标轴文字的方向
#data.plot.barh()
plt.title("Title")
#plt.ylim(0,0.8) #y轴的范围
plt.show()
```

结果：
![直方图](/images/zhifang.png)

# 二、Python画点线图

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from pylab import mpl

mpl.rcParams['font.sans-serif'] = ['FangSong']
mpl.rcParams['axes.unicode_minus'] = False

plt.title("title")

x = [0.3,0.4,0.5,0.6,0.7]

y1 = [1,0.9,0.8,0.6,0.3]
y2 = [1,0.9,0.9,0.6,0.2]
y3 = [1,0.9,0.7,0.4,0.0]
y4 = [1,0.9,0.7,0.3,0.0]
# 下面几行的gbcr表示颜色
# ×s^v表示点的格式
# 后面的-表示画线，不加-的话只画点图了
plt.plot(x,y1,'g*-',label='A') 
plt.plot(x,y2,'bs-',label='B')
plt.plot(x,y3,'c^-',label='C')
plt.plot(x,y4,'rv-',label='D')
plt.legend()

plt.xlabel(r'$\mu$') #x轴标签为希腊字母谬
plt.ylabel('Q')
plt.xlim(0.3,0.7)#设置x轴范围
plt.ylim(0,1)
plt.show()
```

结果：
![点线图](/images/dianxian.png)

# 三、Gephi的使用步骤

以Karate网络为例

#### 1.添加网络的邻接表, 格式为csv

![1](/images/matplotlib1.png)
选择图的类型：无向的，选New workspace

#### 2.添加网络的社团划分, 格式为csv

![2](/images/matplotlib3.png)
选择图的类型：无向的，选Append to existing workspace

#### 3.调整参数，选择布局，调整网络图

![3](/images/matplotlib4.png)

#### 4.结果

![karate](/images/karate.png)
