---
title: 闲谈机器学习(4)
date: 2016-04-09 17:46:05
tags:
---
## 1.kernel Ridge Regression
将表示定理用于Ridge Regression，可以将线性Ridge Regression推广到非线性。再叙述一次表示定理：

简单来说，一个优化问题表示为：argmin err(out)=l[(x1,y1,f(x1),...,(xn,yn,f(xn)]+g(||f||) 

此时，l(.)非负，g(.)为单调增函数，那么存在满足Mercer定理的核函数，使得解总可以写作：
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.6.5.gif"  />

对于l2-norm regularization LM：
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.7.1.gif"  />
我们将看到，这个定理的强大作用。它使LM变成了非线性模型，而不需要对LM的框架更改许多。

Ridge Reg可以表示为:argmin 平方损失+l2-norm regularization。完全符合表示定理。则：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.7.2.gif"  />

使用矩阵表示：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.7.3.gif"  />

解得

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.7.4.gif"  />
但是这个矩阵比较dense，数值解会浪费时间，且解不稳定。所以实务中用的不多。
## 2.kernel l2-norm regularized LogReg
将表示定理用于l2-norm regularized LogReg，可以将线性Ridge Regression推广到非线性。

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.7.5.gif"  />

这个问题不像SVM具有稀疏性，实务中用的也不多。
## 3.Probabilistic SVM
继续研究LogReg和SVM。

soft-margin SVM使用hinge loss，LogReg使用log loss。

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.7.6.jpg" width = "500" height = "200"  />

可以看到log-loss和hinge-loss很相近，实际上SVM约等于l2-norm regularized LogReg。通常情况下，他们的性能也相当。LogReg主要优势在于可以直接解释为概率。SVM的主要优势在于SVM的解具有稀疏性，因而需要的样本也更少，开销小。

我们可以将SVM和LogReg结合起来。先使用SVM得到一个线性模型。然后将转换后的数据带入LogReg，SVM中可以使用kernel，这个模型同样可以实现非线性分类。这就是所谓的2-stage model。叫做probabilistic SVM。

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.7.7.gif"  />
## 4.SVR
将SVM的思想推广到回归就得到了SVR。让回归曲线也尽可能“胖”一点。仿照soft-margin SVM：
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.9.1.gif"  />

按照SVM的思路，可以导出SVR，同样可以kernel化。
