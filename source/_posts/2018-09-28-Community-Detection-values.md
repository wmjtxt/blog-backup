---
layout: post
title: 社团检测基本概念
date: 2018-09-28
tags: 复杂网络

---

## 1.模块度(Modularity)

&emsp;&emsp;把划分社团后的网络与相应的零模型(Null model)进行比较，以度量社团划分划分的质量。一个网络的模块度定义为：该网络的社团内部边数与相应的零模型的社团内部变数之差占整个网络边数的比例。<br>
&emsp;&emsp;计算公式为：<br>
<!-- more -->
<center><images src="http://chart.googleapis.com/chart?cht=tx&chl= Q = \sum\limits_{v}[e_{vv} - (a_v)^2]"></center><br>


&emsp;&emsp;其中,<images src="http://chart.googleapis.com/chart?cht=tx&chl= e_{vv}">为社团v和社团w之间的连边占整个网络边数的比例。<images src="http://chart.googleapis.com/chart?cht=tx&chl= a_v">为一端与社团v中节点相连的边的比例。<br>

## 2.度中心性(Degree centrality)

&emsp;&emsp;度中心性值定义为：<center><images src="http://chart.googleapis.com/chart?cht=tx&chl= DC_i = \frac{k_i}{N-1}"></center><br>
&emsp;&emsp;其中，<images src="http://chart.googleapis.com/chart?cht=tx&chl= k_i">为节点i的度，N为网络中的节点个数。<br>

## 3.介数中心性(Betweenness centrality)
&emsp;&emsp;介数中心性是以经过某个节点的最短路径的数目来刻画节点重要性的指标,简称介数(BC)。<br>
&emsp;&emsp;节点i的介数定义为：<center><images src="http://chart.googleapis.com/chart?cht=tx&chl= BC_i= \sum\limits_{s{\ne}i{\ne}t}\frac{n_{st}^{i}}{g_{st}}"></center><br>
&emsp;&emsp;其中,<images src="http://chart.googleapis.com/chart?cht=tx&chl= g_{st}">为从节点s到节点t的最短路径的数目, <images src="http://chart.googleapis.com/chart?cht=tx&chl=n_{st}^{i}">为从节点s到节点t的<images src="http://chart.googleapis.com/chart?cht=tx&chl= g_{st}">条最短路径中经过节点i的最短路径的数目。

## 4.接近中心性(Closeness centrality)

接近数，即平均路径长度的倒数。

## 5.k-壳与k-核

## 6.特征向量中心性

