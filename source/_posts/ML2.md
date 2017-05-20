---
title: 闲谈机器学习(2)
date: 2016-04-02 23:04:05
tags:
---
## 1.简单的线性分类模型
前面介绍的线性模型(LM)适应于回归。一般的线性模型可以用于分类吗？可以有以下朴素模型：
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/3.31.1.gif"   />
但是这个优化问题不可导，类似于组合优化，是一个NP-hard问题。

放宽一下，不要求那么精确，我们只要看y(w'x)是否同号即可：
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/3.31.2.gif"   />
这被称为：感知机模型(perceptron)，是最简单的一个线性分类模型，也是最简单的神经网络模型(ANN)。这个问题容易优化，使用SGD（随机梯度下降法）很容易求解。但是这个模型能力非常有限，具体将在ANN部分说明。

## 2.广义线性模型(GLM)
GLM是LM的推广，在统计学上有很严密的理论支撑。机器学习领域最明显的应用是：logistic回归。
这里通过3个角度简单说明LogReg。

所谓GLM，是指它扩展了OLS的误差正态性假设，假设y服从指数分布族的一个分布（正态分布是指数分布族的一支）。如果y~Bern(p)(y服从伯努利分布），那么：
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/3.31.3.gif"   />
当p<=0.5，则y=-1;当p>0.5，则y=+1.
这个模型具有很强的扩展性，如果y~Cat(p1,p2,...)时，可以得到用于多分类的LogReg。（这里的具体推导不书写）

第二个角度更容易理解。我们定义归一化sigmoid函数(softmax)：
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/3.31.4.gif"   />
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/3.31.5.png"  width = "500" height = "200" />
sigmoid(w'x)相当于将值域压缩到[0,1]。

并且，sigmoid函数二阶可导，g'(z)=g(z)(1-g(z))，这对优化而言是好事。对LogReg的优化采用MLE+GD/SGD/Newton/quasi-Newton等数值优化方法皆可。使用GD或者Newton法都可以迭代求解。
<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.1.1.gif"   />
第三个角度仍然从形式化的LM出发。

还是从模型本身出发，P(Y=0|X)=1/(1+exp(-y(w'x))，MLE的目的是让P(Y=1|X)最大，也就是P(Y=0|X)最小：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.19.2.gif"   />

erri=log(1+exp(-ys))，而log(1+exp(-ys))被称为log-loss。

如果y~N(0,1)，我们将得到probit模型，这是一个和LogReg很相似的模型，同样具有归一化作用。