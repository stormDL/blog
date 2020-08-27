---
title: pandas的频繁操作方法
catalog: true
date: 2020-03-22 23:29:18
tags: 编程
categories: tech
---

pandas是python中经常使用的一个库,使用得最多的就是读取文件后的datafram对象,然后其他的操作基本上就是使用一次查询一次用法...,因此记录下最常见的一些操作方法.按照使用频率分为:查,改,删,增四个方面.

测试的数据文档表格如下:

| a    | b    | c    | d    |
| ---- | ---- | ---- | ---- |
| 1    | 2    | 3    | 啊   |
| 4    | 5    | 6    |      |
| 7    | 8    | 9    | s    |
| 11   | 22   | 33   |      |
| 44   | 55   | 66   | d    |
| 77   | 88   | 99   |      |

```
import pandas as pd
df = pd.readcsv('test_data.csv')
```



## 查

- 展示数据前n行(默认是5行)

```
df.head(n)
```

- 展示数据后n行(默认是5行)

```
df.tail(n)
```

- 得到指定列名(列标签)的列数据

```
# 这是得到a和b列的所有数据,多列必须包装成一个列表
df[['a', 'b']]
# 获取某一列的数据,这个可以不用传入一个列表索引
df['a']
```

- 使用iloc进行索引取值

  > iloc即index locate 索引定位,根据行索引和列索引进行数据的查找,其基本格式为:
  >
  > ```
  > df.iloc[行索引的列表/切片表示, 列索引的列表/切片表示 ]
  > ```

```
# 切片表示,得到所有行和所有列
df.iloc[:, :]
# 列表表示,得到所有行和所有列
df.iloc[[0,1,2,3,4,5, ], [0,1,2,3,]]
# 通过上述索引方法即可得到dataframe对象任意一块区域的数据表示
```

- 使用loc进行取值

  > loc即根据label名称进行取值,同上述的iloc一样,传入要得到需要的字段即可.其基本格式如下:
  >
  > ```
  > df.iloc[行标签的列表, 列标签的列表]
  > ```

```
# 得到列标签为 'a'和'b'的第0行和第1行的数据,注意此时0行和1行的标签就是0和1
df.loc[[0,1], ['a', 'b']]
```

- 查看各个数据列的类型&转换数据列的类型

```
# 查看数据列的类型
df.dtypes
# 转换数据列的类型
df['a'].astype(float)
df[['a', 'b']].astype(float)
```

- 查询满足某列或多列条件的数据

```
# 主要形式为 df[条件1 and 条件2 ...]
# 例子1,得到a列中不能被2整除的数据
df[df['a']%2 == 1]
# 例子2, 得到a列中不能被2整除的数据和c列等于3或者9的数据
df[(df['a']%2 == 1) & (df['c'].isin([3, 9]))]
```

## 改

> 改动数据的前提首先是找到要改动的数据(可见本文的"查"版块),其次是改动数据是否要影响到原有数据还是只改动当前数据. 

df.replace(old_value, new_value, inplace)是改动数据所需要的方法,其中old_value是我们将要替换的值,new_value是替换后的值,inplace是控制更改原有数据还是当前数据的值,默认是为False更改当前的值.

eg1.

```
# 更改当前的数据&更换当前的数据1为1111
new1_df = df.replace(1, 1111, inplace=False)
new2_df = df['a'].replace(4, 4444, inplace=False)
print(df)
print(new1_df)
print(new2_df)
```

结果如下:

从三个结果来看,①df也就是原有数据集并没有改变任何值;②new1_df是改变全局的1为1111,返回的也是改变后的全局数据;③new2_df只改变了a列的值,且只返回了当前列的数据.

```
    a   b   c    d
0   1   2   3    啊
1   4   5   6  NaN
2   7   8   9    s
3  11  22  33  NaN
4  44  55  66    d
5  77  88  99  NaN
--------------------
      a   b   c    d
0  1111   2   3    啊
1     4   5   6  NaN
2     7   8   9    s
3    11  22  33  NaN
4    44  55  66    d
5    77  88  99  NaN
--------------------
0       1
1    4444
2       7
3      11
4      44
5      77
```

eg2.

```
# 更改原有数据& 更换原有数据的1为1111
new1_df = df.replace(1, 1111, inplace=True)
new2_df = df['a'].replace(4, 4444, inplace=True)
print(df)
print('-'*20)
print(new1_df)
print('-'*20)
print(new2_df)
```

结果如下:

可以看到,如果是更改原有数据,那么replace方法就会返回None,在new1_df和new2_df上都能体现出来,在df上能看到只要是更改原来的数据那么df都会做出对应的改变

```
      a   b   c    d
0  1111   2   3    啊
1  4444   5   6  NaN
2     7   8   9    s
3    11  22  33  NaN
4    44  55  66    d
5    77  88  99  NaN
--------------------
None
--------------------
None
```

replace方法可以使用字典传入要更改的值和更改后的值,字典的key表示要更改的值,value表示更改后的值,replace(dict, inplace),这样也不用写额外的逻辑去实现单个值的替换.

```
# df.replace(1, 1111, inplace=True)
# df.replace(4, 4444, inplace=True)
# 上面两条语句可以被替换为如下方式,效果是一样的
replace_dict = {1:1111, 4:4444}
df.replace(replace_dict, inplace=True)
```

## 删

> 删除跟改动数据一样,首先要查找到要删除的数据,其次才是删

drop(labels, axis, index, colums, inplace)

labels:单独的标签或者标签的列表

axis: 0表示控制labels参数表示的是行标签,1表示 控制labels参数表示的是列标签

index: 表示要删除的下标

colums: 表示要删除的列标签和列标签的列表

inplace: 表示是否删除原有的数据

eg1.

```
# 删除a和b列的所有数据
# 方法1
df.drop(labels=['a', 'b'], axis=1, inplace=True)
# 方法2
df.drop(columns=['a', 'b'], inplace=True)
# 删除0行和1行的所有数据
# 方法1
df.drop(labels=[0, 1], axis=0, inplace=True)
# 方法2
df.drop(labels=[0, 1], inplace=True)
```

## 增

> 增加数据首先要保持数据的形状在某个维度(行或者列)要保持一致

- 行级别的数据增加, 即不增加列

eg1.

```
# 模拟一个在列数上相等的dataframe
df1 = df.iloc[:2, :]
df = df.append(df1)
print(df)
```

结果如下:

```
    a   b   c    d
0   1   2   3    啊
1   4   5   6  NaN
2   7   8   9    s
3  11  22  33  NaN
4  44  55  66    d
5  77  88  99  NaN
0   1   2   3    啊
1   4   5   6  NaN
```

eg2.

```
# 模拟一个在列数上相等的dataframe
df1 = df.iloc[:2, :]
df = pd.concat([df, df1], axis=0)
print(df)
```

结果如下:

```
    a   b   c    d
0   1   2   3    啊
1   4   5   6  NaN
2   7   8   9    s
3  11  22  33  NaN
4  44  55  66    d
5  77  88  99  NaN
0   1   2   3    啊
1   4   5   6  NaN
```

- 列级别的数据增加,即不增加行

```
# 模拟一个在行数上相等的dataframe
df1 = df.iloc[:, :2]
df = pd.concat([df, df1], axis=1)
print(df)
```

结果如下:

```
    a   b   c    d   a   b
0   1   2   3    啊   1   2
1   4   5   6  NaN   4   5
2   7   8   9    s   7   8
3  11  22  33  NaN  11  22
4  44  55  66    d  44  55
5  77  88  99  NaN  77  88
```

