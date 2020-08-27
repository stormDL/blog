---
title: pytorch学习笔记
catalog: true
date: 2020-05-18 22:41:55
tags:
- 编程
- 学习
categories: tech
---

## 基础概念

- pytorch是一个深度学习框架，由facebook的人工智能小组开发并开源．

- pytorch是一个可以使用GPU加速运算的深度学习库，而numpy只能在cpu上进行运算．

- 如果pytorch中的tensor是由numpy的数据转化的，这些数据共享一块内存；对于由列表转换成的tensor来说，也是一样的共享内存，即tensor的值改变了之后，列表中元素的值也随之做对应的改变．

- pytorch可以把tensor转化为numpy的类型

  ```python
  # 这里的a是pytorch中的tensor,　调用numpy()得到该tensor对应的numpy
  a.numpy()
  ```

## 自动求导

在pytorch中神经网络主要使用的就是` autograd`包，这个包提供了自动求梯度的功能．自己就不用再自己实现反向传播算法了．

- 对于tensor来说，需要设置` .requires_grad`属性的值为` True`，才会把这个tensor看做一个变量而不是一个常数，通过` backward()`方法会自动求出关于这个tensor中的偏导，并且把偏导的值存放在` .grad`属性当中．
- 如果运行过程当中，需要停止对该参数进行求导，则直接使用`.detach()`方法，这个方法相当于创建了一个副本，不同的是` .requires_grad`属性为` False`．原先的数据仍然存在．
- 为了避免上述出现副本＆对于原先的变量tensor仍然能够进行梯度的求导，可以使用这个上下文管理器` with torch.no_grad()`