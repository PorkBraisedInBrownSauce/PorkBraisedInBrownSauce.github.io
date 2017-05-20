---
 title: 使用MPI和OPENMP优化的Kmeans-BIC算法
 date: 2016-9-29 12:20:21
 tags:
---
由于一点缘故，前一阵子写了一个基于[MPI](http://www.mpich.org)和[OPENMP](http://www.openmp.org)的CPU高并行的Kmeans程序。这个程序的工作是基于[1]，一方面加入了OPENMP的线程级别并行优化，另一方面加入了BIC自动选择cluster个数的能力，这个的想法主要基于[2]，但是实现有一些区别。在[天河-2](http://www.nscc-gz.cn/)运行结果显示：加速比接近于5倍，而且具有初始化的鲁棒性。

由于是第一次写MPI和OPENMP的程序，对进程并行和线程并行理解不清，在这篇文章简单解释一下。

MPI主要是在定义一套通信标准，就像通信网一样，不同进程之间由于是互相不知情的，所以怎么让进程之间相互协作就是MPI最重要解决的问题。由于是基于map-reduce思想，不是所有程序都可以使用MPI，一般来说，使用MPI会使程序发生很大的结构变化。书写MPI程序从顶层结构来讲：主体部分+通信部分。主体部分包含程序不同进程的处理，通信部分包含进程间的协作。具体API可以参考[3]。

OPENMP相对来讲简单很多，VS一类IDE的支持也很好，只需要写一些宏语句，编译器就可以很好地编译。因为是线程之间的并行，不存在人为的设定不同线程之间怎样通信的问题。需要做的主要是找出热点使用OPENMP和线程同步问题。线程同步问题可以参考[4]，OPENMP主要实现了：互斥锁（mutex），临界区域（automic, critical），事件。


代码见Github：[https://github.com/foowaa/miscellaneous_code/tree/master/kmeans_parabic](https://github.com/foowaa/miscellaneous_code/tree/master/kmeans_parabic)

Reference:
[1] [https://code.google.com/archive/p/pspectralclustering/](https://code.google.com/archive/p/pspectralclustering/)
[2] Pelleg, Dan, and Andrew W. Moore. "X-means: Extending K-means with Efficient Estimation of the Number of Clusters." ICML. Vol. 1. 2000.
[3] [http://micro.ustc.edu.cn/Linux/MPI/MPICH/](http://micro.ustc.edu.cn/Linux/MPI/MPICH/)
[4] [http://yoyzhou.github.io/blog/2013/02/28/python-threads-synchronization-locks/](http://yoyzhou.github.io/blog/2013/02/28/python-threads-synchronization-locks/)
