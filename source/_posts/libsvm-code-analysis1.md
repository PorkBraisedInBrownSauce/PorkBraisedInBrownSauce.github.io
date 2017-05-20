---
title: libsvm源码分析(1)
date: 2016-06-07 22:46:05
tags:
---

看一个比较完善的项目的源码是如何统筹安排的，libsvm是一个文件比较少的项目，就它来做研究。

先看一下**Makefile**:

> CXX ?= g++

>CFLAGS = -Wall -Wconversion -O3 -fPIC

>SHVER = 2

>OS = $(shell uname)

>all: svm-train svm-predict svm-scale

>lib: svm.o
>
>	if [ "$(OS)" = "Darwin" ]; then \
	
>	SHARED_LIB_FLAG="-dynamiclib -Wl,-install_name,libsvm.so.$(SHVER)"; \

>	else \

>	SHARED_LIB_FLAG="-shared -Wl,-soname,libsvm.so.$(SHVER)"; \
		
>	fi; \
>	
>	$(CXX) $${SHARED_LIB_FLAG} svm.o -o libsvm.so.$(SHVER)

>svm-predict: svm-predict.c svm.o
>
>	$(CXX) $(CFLAGS) svm-predict.c svm.o -o svm-predict -lm
>	
>svm-train: svm-train.c svm.o
>
>	$(CXX) $(CFLAGS) svm-train.c svm.o -o svm-train -lm
>	
>svm-scale: svm-scale.c
>
>	$(CXX) $(CFLAGS) svm-scale.c -o svm-scale
>	
>svm.o: svm.cpp svm.h
>
>	$(CXX) $(CFLAGS) -c svm.cpp
>	
>clean:
>
>	rm -f *~ svm.o svm-train svm-predict svm-scale libsvm.so.$(SHVER)

可以看出：svm.h为唯一的头文件，svm.cpp做成.o文件供svm-train, svm-scale, svm-predict调用，svm-train, svm-scale, svm-predict分别封装成为可执行文件，svm.cpp还要做成动态链接库文件，以提供Python的接口。


svm.h文件：

由于svm.h是所有源文件(C, CPP)的头文件，所以它的声明没有class。

> \#ifdef __cplusplus

> extern "C" {

> \#endif
> 
> \#ifdef __cplusplus
> 
> }
> 
>\#endif

是典型的C++与C共同编译的头文件形式。定义了svm_node, svm_problem, svm_parameter, svm_model四个结构体。并声明了通用的函数。

svm.cpp文件：

此文件有3000多行。里面定义了kernel和其他一些函数。