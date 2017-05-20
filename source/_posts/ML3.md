---
title: 闲谈机器学习(3)
date: 2016-04-09 17:45:15
tags:
---
## 1.hard-margin SVM
支持向量模型是第二大类线性模型，它着眼于直接增强模型的泛化能力。即让分界面尽可能的“胖”。SVM用于分类。

如果分类器能正确分类：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.5.1.gif"   />

一个点在n维空间到分类超平面的距离：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.5.2.gif"   />

取分子为边界值：（即hard-margin）

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.5.3.gif"   />

此时，我们得到一个有约束的优化问题：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.5.4.gif"   />

这个问题不好优化，把它变成一个好优化的问题（等价变换）：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.5.5.gif"   />

这是一个二次优化问题。（凸问题是最愿意见到的一个问题）对比具有hard-margin和没有margin的情形：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.5.6.png"   />   

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.5.7.png"   />


## 2.soft-margin SVM
在前面的讨论中，我们一直假定训练样本是线性可分的，然而，现实中很多数据不是线性可分的，这个时候要进一步提升模型的泛化能力。使它也能容忍一部分线性不可分的training data。

采用增加正则化项的方法：
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.5.8.gif"   />

后面那部分就是正则化项（罚函数），也就是优化问题会同时考虑margin最大和误分类点尽可能少。但0/1损失数学性质不好，我们改为：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.5.9.gif"   />

其中max(0,1-z)叫做hinge损失函数。如果换一个看法：err(out)=hinge(s)+l2-norm Regularization。又回到了经典的LM那里。

换第三个看法：引入松弛变量，将式子再改写一下

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.5.10.gif"   />

注意到和hard-margin SVM 相比，soft-margin SVM多了松弛变量，正是一个松弛变量让margin可以自动调整。

## 3.SVM的Dual问题
基于优化的primal-dual理论，我们可以得到hard-margin SVM和soft-margin SVM的dual问题。

dual hard-margin SVM:
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.5.11.gif"   />

dual soft-margin SVM:
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.5.12.gif"   />

由对偶松弛条件，dual问题有较为稀疏的解，这是我们愿意看到的。

## 4.Representer Theorem
kernel在数学上是一个很宽泛的概念，这里的kernel特指能够隐形的进行内积运算的函数：
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.6.1.gif"  />
试想，如果没有kernel，进行上述运算，运算复杂度为平方量级，如果引入kernel，会变成线性量级。其次，kernel可以很好的表示无限维的feature，以similarity为度量，将无限维的feature隐式的包含在kernel里。（embedding infinite features in kernel）

观察dual hard-margin SVM和dual soft-margin SVM，里面含有内积运算。但是他们仍然只能进行线性分类，如果我们有一种映射，将低维不可分映射到足够高维，可能就会成为高维的线性可分问题。

此时dual soft-margin SVM:
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.6.2.gif"  />

使用kernel:
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.6.3.gif"  />

这样线性分类的SVM自然地扩展到了非线性。

再深入讨论。只有核矩阵是半正定（半正定Gram矩阵）的kernel才是可用的kernel(Mercer定理)。事实上，对于一个半正定的核矩阵，总能找到一个与之对应的映射。任何一个kernel都隐式定义了一个再生希尔伯特核空间(RKHS)。（内积可以再生成原始函数的Hilbert Space）

数学推导得到了重要的**表示定理**：
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.6.4.png"  />

简单来说，一个优化问题表示为：argmin err(out)=l[(x1,y1,f(x1),...,(xn,yn,f(xn)]+g(||f||) 

此时，l(.)非负，g(.)为单调增函数，那么存在满足Mercer定理的核函数，使得解总可以写作：
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.6.5.gif"  />

对于l2-norm Regul. LM：
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.7.1.gif"  />
我们将看到，这个定理的强大作用。它使LM变成了非线性模型，而不需要对LM的框架更改许多。

将SVM的dual很容易推广到非线性情形。
