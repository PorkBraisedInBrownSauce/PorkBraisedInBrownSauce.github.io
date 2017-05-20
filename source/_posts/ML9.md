---
title: 闲谈机器学习(9)
date: 2016-04-14 17:46:05
tags:
---
## 1.Bootstrapping
bootstrapping是统计学的一大类方法。

In statistics, bootstrapping can refer to any test or metric that relies on random sampling with replacement.  This technique allows estimation of the sampling distribution of almost any statistic using random sampling methods. Generally, it falls in the broader class of resampling methods.

简单来说，bootstrap sampling给定m个样本的数据集，我们随机抽取一个样本放入采样集，再把该样本放回原始数据集，使得它仍有机会被抽到，这样经过m次随机采样，得到m个样本。

由于每一次，每个样本被选取的概率都是1/m，采样m次，则某个样本始终没出现的概率为：

<img src="http://7xs6jl.com1.z0.glb.clouddn.com/4.13.2.gif"  />

也就是说，会有36.8%的数据总是没有被选到，这样在validation set就可以利用这36.8%的数据。叫做“out-of-bag estimate"。实务上，只需要记录没有被选取的数据就可以。


## 2.Bootstrap Aggregation
简称bagging，是机器学习中常用的并行aggregation方法。具体原理就是采用boostrap sampling，训练不同的base learners。最后组合到一起。这里的base learner要尽可能相互独立。

例如将决策树作为base learner，将LinReg作为base learner，将ANN作为base learner都无不可。但是仍然要注意overfitting，如果全是精英，那么往往听不到下层的声音。。。由于bagging 关注于降低var，所以var较大的base learner可以与bagging结合，例如未剪枝的决策树。

## 3.随机森林
Random Forest是将D.T.与bagging结合起来的一种模型。但是RF在D.T.的训练过程中引入了随机属性选择机制，这样大大提高了base learner的多样性。RF往往比单纯的D.T.+bagging效果好。

在RF中，岁D.T.的每个节点，先从该点属性集合中随机选择一个包含k个属性的子集，然后再从这个子集中选择一个最优属性用于划分。这样得到若干棵D.T.，再将这个森林进行集成，就可以得到模型的输出。