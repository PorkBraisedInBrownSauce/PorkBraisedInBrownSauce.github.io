---
 title: 安装Torch和相关东西的小脚本
 date: 2016-9-28 11:20:21
 tags:
---
深度学习四大框架：[Caffe](http://caffe.berkeleyvision.org), [Torch](http://torch.ch), [Theano](http://deeplearning.net/software/theano/), [Tensorflow](https://www.tensorflow.org/)，Caffe根本不需要编程（且已装），可扩展性太差；Theano和Tensorflow在服务器上都没搞定,Theano的扩展形式Keras, Lasagne可扩展性也有问题，Theano在某些方面有点太low-level了；只能把最后的希望寄托在Torch上了。在工作站上安装了几天:P，发现GPU真的很难配置，主要是因为原来都安装了Caffe，版本问题和依赖库问题是Linux安软件最蛋疼的问题。我特意把它做成了一个脚本，这个脚本安装Torch相关，[iTorch](https://github.com/facebook/iTorch)相关（但不保证安上）和[zbStudio](https://studio.zerobrane.com/)（一个Lua的IDE）。安装前应该安装好cuda和cudnn，并记录清楚版本，因为cutorch,cunn和cudnn这三个与GPU相关的工具都对版本有要求，可以先安装cutorch,cunn和cudnn，如果版本不对，建议去github上自己下载编译。zbStudio上显示不了图像，经确认是解释器有问题，我已改为qlua解释器。iTorch安装了几天都没搞好，还是jupyter和torch的配合有问题，本身itorch是可以require了。最后确认到pyzmq引入不了cffi，研究了pyzmq的代码，无解，现在使用命令行和zbStudio搞torch。

1

接下来会写一些Torch的教程。敬请期待lol

脚本：[https://github.com/foowaa/miscellaneous_code/tree/master/SomeShellScripts](https://github.com/foowaa/miscellaneous_code/tree/master/SomeShellScripts)