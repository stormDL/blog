---
title: python解释器种类以及特点
catalog: true
date: 2020-03-31 09:32:13
tags: 
- 问题
- 编程
- python
categories: tech
---

要运行python代码就必须要一个python解释器！



python解释器不止一种，主流的有CPython&IPython&Jython&PyPy&IronPython等

**CPython**: 使用c语言开发的解释器，是官方标准实现，拥有良好的生态环境，也是应用最广泛的解释器．

**IPython**: 基于CPython上的一个交互式解释器，在交互方式上比CPython更强，但底层是与CPython一样的．

**Jython**: 是为java语言设计的python解释器，可以直接把python代码编译成java字节码然后运行在虚拟机上．

**PyPy**: 目标是提高python运行速度的解释器，通过采用JIT技术，对Python代码进行动态编译，所以可以显示的提高执行速度．

**IronPython**: 运行在.NET平台上得到Python解释器，可以直接把python代码编译成.NET的字节码文件．



**总结**：

- python解释器使用最广泛的是CPython
- 如果需要跨平台调用功能，那么最好是使用网络调用来交互（确保程序的独立性），而不是通过改变解释器类型来交互．