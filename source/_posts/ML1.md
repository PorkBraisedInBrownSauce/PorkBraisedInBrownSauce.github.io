---
title: 闲谈机器学习(1)
date: 2016-04-02 21:04:42
tags:
---
## 1.OLS：一般最小二乘回归
这是最普通也是最原始的回归模型。在20世纪就由Gauss等数学家提出。OLS的最优特点是有闭式解，甚至不需要太多的优化手段。OLS顾名思义，采用平方损失函数。

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/3.23.1.gif" />

那么最优化问题为：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/3.23.2.gif"   />

得到：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/3.23.3.gif"   />

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/3.23.4.gif"  />

由于有close form的解（不讨论广义逆等情况），所以求解起来非常容易。请注意w*，之后的很多变形和它都有关系。

## 2.Ridge Regression
对于预测的w*，有如下定理：
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/3.23.5.gif"   />，这是bias-varience分解的另一种表达。

由于w是无偏估计（Gauss-Markov），所以：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/3.23.6.gif"  />

当X'X的特征值非常小时，会出现MSE非常大的情况。所以我们期望对w*的表达式做一改造，使之避免这种情况。

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/3.23.7.gif"  />

显然，X'X的特征值将同时增大。这也是防止X'X变为奇异矩阵的方法。这会减小varience。

从机器学习的角度来说，这相当于Regul.，惩罚过大的协方差。使用l2-norm Regul.(Tikhonov Regul.)防止overfitting。

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/3.23.8.png"   />

Ridge Reg的实现和OLS很相似，这里不再赘述。只有一个超参数需要人为调整，可以采用Grid Search或者使用先验知识等方法进行设定。

## 3.Lasso
我们还可以发现，Ridge Reg解决的实际上是数据的特征>数据量(n>m)的问题，由于r(X'X)<=r(X)<=m<n，而X'X是n*n的矩阵。所以减少数据的维数就可以解决。

Lasso添加l1-norm Regul.罚项，它是一种更趋向于选择稀疏解的线性回归。（具体可以看统计学教材）

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/3.23.9.png"   />

罚函数为P(w)，我们假设P(w)为向量lp范。p>1，此时P(w)为光滑凸函数；p=1，为不光滑凸函数；0<p<1为非凸函数.

我们倾向于解决p>1的可微凸优化问题。当p=1时，就得到Lasso。p=2为Ridge Regression。Lasso无法给出一个闭式解。需要采用优化方法。由于Lasso的稀疏性，广泛运用在sparse learning中

## 4.概率视角
以上的理论推导并未用到任何概率知识，仅仅采用最小二乘法，解了一个线性方程组而已。以下采用概率统计的视角的得到相同的结果。

显然，

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.24.1.gif"   />

可以看出，OLS实际是在Gaussian分布下的MLE。

我们再进行一些更复杂的推导，可以得出另一个结论。选用共轭先验：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.24.2.gif"   />

Ridge Reg实际是先验为Gaussian的MAP估计。

