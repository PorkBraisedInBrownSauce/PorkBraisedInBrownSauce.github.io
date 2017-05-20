---
title: 闲谈机器学习(5)
date: 2016-04-11 17:46:05
tags:
---
## 1.Transform OR Basis
<img src='http://7xs6jl.com1.z0.glb.clouddn.com/4.24.3.gif'>

对x在一个basis下施以变换，使得x首先完成一个特征转换。LM是指w的线性，而不是特征特征一定是线性。

## 2.kernel trick
我采用MLPP的说法：

> Rather than defining our feature vector in terms of kernels, φ(x) = [κ(x,x 1),...,κ(x,xn)], we can instead work with the original feature vectors x, but modify the algorithm so that it replaces all inner products of the form  <x,x'> with a call to the kernel function, κ(x,x').

凡是在内积空间的变换总是可以有kernel与之对应。而且如前所述，kernel的一大特点是可以embedding infinite features。Representer Theorem只是kernel trick一个子集。

## 3.kernel kNN and kernel k-means
kNN天然可以kernelized，因为采用l2-norm的kNN天然具备内积这一个优化条件。

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.24.4.gif">

l2-norm的k-means也是这样。两点之间的距离可以化为kernel。

## 4.kernel machine
GLM中feature表示为：<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.12.1.gif"  />叫做kernel machine。

如果为RBF kernel，则得到一个RBF NNet。误差采用二次误差，激活函数采用RBF函数。
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.12.2.png"  />
Architecture of a radial basis function network. An input vector x is used as input to all radial basis functions, each with different parameters. The output of the network is a linear combination of the outputs from radial basis functions.

<center><img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.12.3.png"  /></center>

<center><img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.12.4.png"  /></center>

注意，这和RBF-kernelized SVM很相似，除了采用的loss func.不同，实际上，ANN可以包含一切supervised model，ANN每一层都在做feature transformation~T(x)，BP算法在argmin erri。LM只是层数较少的（2~3层）ANN而已。

再注意到，如果采用uniform vote，那么这个kernel machine等同于欧氏距离的kNN算法。因为内积空间的内积度量实际也在距离度量，RBF kernel实际在度量特征的欧氏距离。

如何确定ci？可以使用clustering，例如k-means。

如果我不想使用unsupervised算法呢？最直接的方法是：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.12.5.gif"  />

但是这样会导致模型的泛化能力很成问题，计算复杂度也有点高，我们希望w尽可能稀疏一点，这样就引入了sparse vector machine，SVM就是一种sparse vector machine。

## 5.kernel PCA
PCA方法实际是在操作Gram矩阵，而Gram矩阵与kernel有天然的联系。所以kernel PCA是最天然不过的了。对于非线性相关的数据，通过kernel将其转化为线性相关问题，这样PCA就可以奏效。
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.12.6.png"  />

则kernel为：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.12.7.png"  />

这个kernel可以组成kernel矩阵，也是cov矩阵，再做一点变换：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.12.8.gif"  />

对变换后的kernel矩阵做标准的PCA操作就可以得到kernel PCA

## 6.非参统计的核方法（待）
