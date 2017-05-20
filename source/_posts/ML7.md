---
title: 闲谈机器学习(7)
date: 2016-04-12 17:46:05
tags:
---
## 1.training set, validation set, test set 
training proceeds on the training set, after which evaluation is done on the validation set, and when the experiment seems to be successful, final evaluation can be done on the test set.

* training set：学习模型的参数
* validation set：模型选择和调参
* test set：最终的模型评估 

固定的划分数据集的方法叫做handout，这种方法的缺点是容易导致训练数据减少，一种可行的方法是cross-validation，对于k-fold CV，将数据分为k份，数据在k-1份上做训练，在剩下来的数据上做validation。这样可以在数据集上做k次训练和测试，最终按照一定方法取值（例如均值）。由于划分数据集的方法是随机的，所以可以进行p次数据集划分，叫做p次k-fold CV。

## 2.multi-stage
multi-stage是将特征进行多次映射，最终映射到输出空间。广义上讲，大部分学习器都是multi-stage的，这里的multi-stage特指将base learner aggregate to good(or better) learner。这类模型的最大优点在于降低variance，提升泛化性能，但是erri却不一定降低。所以抗overfitting的性能很好。

那么问题是base learner怎样获得，base learner到better learner的映射关系如何建立。

事实上，base learner的要求只有一条"good enough but different"，精确性和多样性本来就是有矛盾的。但我们想达到一种平衡。但是good enough即可，如果太好，反而可能造成overfitting（全是精英，商量的结果有可能脱离实际。。。）

其次，base learner到better learner的映射关系如何建立。主要有5种：selection，uniform voting，linear voting，any voting，conditional voting。selection是uniform voting的退化情形，uniform voting是linear voting的退化情形，linear voting是any voting的退化情形，conditional voting是决策树采用的方式。

方式上，对样本的方法有bootstapping和boosting，对model的方法有blending。

## 3.Blending
从model角度，实现multi-stage的方法。具体而言，至少2-stages：

* get base learners

* blend them to a better learner

如果采用selction，那么就属于模型选择问题。如果采用uniform voting，就等同于uninformative prior。linear voting或者叫weighted voting，如果有很好的先验知识，优于uniform voting，但是multi-stage多多少少都有blackbox的性质，精确的先验确实是比较难的。any voting非常强大，但是又有overfitting之虞。

如果不采用prior，而是采用data-driven的方法，叫做stacking。简单来说，在traning set上得到base learner，把它当做一个特征转换，然后在把base learner的输出当做次级的输入，在validation set上继续训练。