---
layout: w
title: pytorch关于tensor的基础操作
date: 2020-07-08 14:29:37
categories: tech
tags: 
- 算法
- pytorch
---



## 创建tensor的常见操作

- 把np数据转化为pytorch的tensor,其中**tensor和ndarray共享同一内存空间**

```
torch.from_numpy(ndarray)
```

- arange根据步长来生成tensor，返回的是一维张量

```
torch.arange(start, end, step=1, out=None)
```

- 使用linspace来生成tensor，包含start和end两个端点，被平分成steps个点的数据，返回的是一个一维张量

```
torch.linespace(start, end, steps=n, out=None)
```

- 使用ones，返回某个形状且值全为1的tensor

```
torch.ones(*sizes, out=None)
```

- 使用zeros返回一个全为标量 0 的张量

```
torch.zeros(*sizes, out=None)
```

- rand随机生成某个形状的tensor，随机数满足[0, 1)的均匀分布

```
torch.rand(*sizes, out=None)
```

- randn随机生成某个形状的tensor，随机数满足均值为0、方差为1的正态分布

```
torch.randn(*sizes, out=None)
```

- 得到0到n-1个数的随机排列，参数n表示上界（真实数据不包含n）

```
torch.randperm(n, out=None)
```

## 索引、切片、连接、换位

- 连接，拼凑多个张量为一个tensor

```
torch.cat(inputs, dimension=0)
```

- 把一个tensor沿着某一个维度分成几个张量

```
torch.chunk(tensor, chunks, dim=0)
```

- 在某个维度上，将输入索引张量指定位置上的值进行聚合,index必须是一个LongTensor张量

```
torch.gather(input, dim, index, out=None)
```

- 选取指定位置上的值，index必须是一个LongTensor张量

```
torch.index_select(input, dim, index, out=None)
```

- 根据二元值来获得一个一维的张量, mask是一个ByteTensor张量

```
torch.masked_select(input, mask, out=None)
```

- 得到非 0 值的元素索引

```
torch.nonzero(input, out=None)
```

- 去除形状中的1，即shape=(3,1,2,1)变为shape=(3,2),**返回的张量内存共享的哦~**

```
torch.squeeze(input, dim=None, out=None)
```

- 转置操作，交换维度，**得到的tensor也是内存共享的哦~**

```
torch.transpose(input, dim0, dim1, out=None)
```

- 在指定位置上添加一个维度1，比如shape=(2, 2)增加一个维度，放在第一个位置上shape=(2,1,2)，**返回张量与输入张量共享内存**

```
torch.unsqueeze(input, dim, out=None)
```

