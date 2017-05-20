---
title: 几个优化算法小程序
date: 2016-04-16 16:46:05
tags:
---
# 几个优化算法小程序
写了几个maltab版的无约束优化计算程序：

* Wolfe非精确线搜索步长求解
* Armjio非精确线搜索步长求解
* GD法求解非约束优化
* Newton法求解非约束优化
* BFGS quasi-Newton法求解非约束优化
* CG法求解非约束优化
* 使用差分法求解Gradient和Hessian

由于都是简单的程序，计算速度不快。另外CG法的precondioning没做，LBFGS没做，Newton法是最原始的版本,所以经常会出问题。

接下来，希望把LBFGS搞出来，简单的约束优化方法也准备弄出来。python版本也准备开发。

<s>使用范例：[x,y]=tGD(@funName, w0, varargin)</s>

**<u>Codes are in my github, welcome pulling request**</u>

<s>**采用100*2的feature，对Logistic回归做了一下数值模拟**，统计了一下错误率和训练时间(s)：</s>

<s>GD+Armijo  12%  0.3</s>
<s>GD+Wolfe    14%  4.7</s>

<s>Newton+Armijo NaN：原因是Hessian条件数太差</s> 
<s>Newton+Wolfe  17% 17</s>

<s>CG+Armijo 16%  0.11</s>
<s>CG+Wolfe   9%    0.35</s>

<s>BFGS+Armijo 7%    0.04</s>
<s>BFGS+Wolfe  2.4%  0.03</s>

<s>GD+Wolfe训练时间长且错误率高；Newton+Armijo有时可能无法训练出结果，原始Newton法非常慢；BFGS效果最好。</s>

**********************

**UPDATE:**(Apr. 18)

1. 改进原始Newton法为damped modified Newton法
2. 添加LBFGS
3. 接口改变，可以设置参数

eg:

> options.method='BFGS';
> options.nmax=1000;
> [x,y1,time,niter]=tmin(f,x0,options,X,y);

数值模拟：10000*2 feature，Logistic分类

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.18.1.png" />

顺便试了一下SGD，发现几分钟都运行不出来，偶尔会非常快。估计主要原因是SGD等同于直接迭代，在具有数值优化功能的软件比较慢，在C一类的语言中，估计和GD差不多，但是写着简单点。

小规模精确求解BFGS最好，大规模LBFGS较好。LBFGS错误率虽然有点高，但是时间优势太大。Newton法本来应该比CG法块，但是计算Hessian花了太多的时间。


<b>**UPDATE(Apr.20):**</b>

已知对LBFGS的误差耿耿于怀，研究了一下，找到了问题。LogReg的训练很容易产生underflow，进而得到NaN的结果，原来也发现，然后加了一个0.01的bias。但是估计LBFGS算法本身会抛掉一些Sample，以节省内存，把bias放大了。不论调什么参数都没办法降低err。看了一下MLPP，发现可以使用log-sum-exp trick解决，试了一下，发现LBFGS错误率大大降低。

使用100000*5的样本训练：

LBFGS：用时0.96s，5000次迭代，错误率12.21%

BFGS：用时4.625s，12次迭代，错误率10.78%

另几个算法，这个规模的数据几分钟都算不出来，降低数据规模10000*5：

Newton：用时37.28s，269次迭代，错误率5.42%

CG：用时92.34s，2500次迭代，错误率11.22%

GD：用时104.82s，2500次迭代，错误率5.74%
