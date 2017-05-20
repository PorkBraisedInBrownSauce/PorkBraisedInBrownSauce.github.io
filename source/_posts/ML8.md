---
title: 闲谈机器学习(8)
date: 2016-04-13 11:46:05
tags:
---
决策树(D.T.)属于conditional voting的multi-stage模型。D.T.的base learner是只含2层的D.T.，叫做decision stump，每一层在一定条件下进行分支。

同样套入统计机器学习框架，我们关注的还是erri和Regul.两个问题。（当然还有优化方法和数值计算问题）

## 1.Regression Tree
首先看erri。对于回归，最普遍的是采用二次损失。在D.T.中，将erri解释为impurity，在树分支的过程中，impurity要越来越低，而每一步的分支要尽可能降低impurity。

每一步要寻找让impurity降低最明显的feature，并且要找到这个feature让impurity降低最明显的阈值。

如下例：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.13.3.png" />

在二维空间，表示的分界面如下：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.13.4.png" />

可以看到，RegTree实际上在用一定数量的超平面模拟分界曲面，如果分支无限下去，就可以得到曲面。

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.13.5.png" />

## 2.Classification Tree
对于分类，我们前面采用过0/1 loss,log-loss,cross-entropy loss,exp-loss,hindge loss,etc。这里没有得分的概念，输入空间是完全离散的，但是impurity依然适用。可以采用误分类率，信息增益，Gini index等。CART采用Gini index。

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.13.6.gif" />

直观上说，Gini反映了数据集D中随机抽取两个样本，其类别标记不一致的概率，也就是impurity。

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.13.7.png" />

## 3.pruning
不论是RegTree还是ClsTree，共有的缺点是variance较大，对外部反应过于敏感，泛化能力不足。容易overfitting。这时肯定要采用Regul.，对D.T来说，就是pruning。

prepruning是在D.T.的生长过程中，对每个节点在划分之前，先于val. set上的数据对比，看正确率大小，如果在val. set上正确率提升，就划分，否则就禁止划分。postpruning是在训练结束后回溯地进行pruning。prepruning不仅能防止overfitting，还可以减少训练时间。

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.13.8.png" />