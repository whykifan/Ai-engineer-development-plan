
<!-- TOC -->

- [Numpy ndarray对象](#numpy-ndarray对象)
- [创建ndarray](#创建ndarray)
    - [1、基本的方法](#1基本的方法)
    - [2、创建数值范围的数组](#2创建数值范围的数组)
    - [3、通过random方式创建](#3通过random方式创建)

<!-- /TOC -->

# Numpy ndarray对象
NumPy 最重要的一个特点是其 N 维数组对象 ndarray，它是一系列同类型数据的集合，以 0 下标为开始进行集合中元素的索引。

ndarray 对象是用于存放同类型元素的多维数组。

ndarray 中的每个元素在内存中都有相同存储大小的区域。

ndarray 内部由以下内容组成：

-   **一个指向数据（内存或内存映射文件中的一块数据）的指针。**
    
-   **数据类型或 dtype，描述在数组中的固定大小值的格子。**
    
-   **一个表示数组形状（shape）的元组，表示各维度大小的元组**。
    
-   **一个跨度元组（stride），其中的整数指的是为了前进到当前维度下一个元素需要"跨过"的字节数**。
    
ndarray 的内部结构:
![ndarray 的内部结构](http://www.runoob.com/wp-content/uploads/2018/10/ndarray.png)
跨度可以是负数，这样会使数组在内存中后向移动，切片中 obj[::-1] 或 obj[:,::-1] 就是如此。

# 创建ndarray
Numpy数组常见参数说明:

|shape|数组形状 |
|:--:|:--:|
|dtype|数据类型，可选|
|order|有"C"和"F"两个选项,分别代表，行优先和列优先，在计算机内存中的存储元素的顺序。|

## 1、基本的方法

创建numpy数组最简单的方法就是直接创建:
```python
>>>  import numpy as np
>>>> a = np.array([0,  1,  2,  3])  # 1-D  
>>> a
array([0,  1,  2,  3])  

>>> b = np.array([[0,  1,  2],  [3,  4,  5]]) #2-D，2rowsx3cols
>>> b
array([[0,  1,  2],  [3,  4,  5]])
```
或者可以使用 **numpy.asarray()** 函数，通过列表，元组方式创建:
```python
numpy.asarray(a, dtype =  None, order =  None)
"""
a:任意形式的输入参数，可以是，列表, 列表的元组, 元组, 元组的元组, 元组的列表，多维数组
dtype:数据类型，可选
order:可选，有"C"和"F"两个选项,分别代表，行优先和列优先，在计算机内存中的存储元素的顺序。
"""
#列表来创建
>>>x = [1,2,3]  
>>>np.asarray(x) 
[1  2  3]
#元组来创建
>>>x =  (1,2,3) 
>>>np.asarray(x) 
[1  2  3]
#元组列表创建
>>>x = [(1,2,3),(4,5)]  
>>>np.asarray(x)
[(1,  2,  3)  (4,  5)]
```
## 2、创建数值范围的数组
上面的方式是最基本的方法，但是对于复杂数组的创建就需要使用一些比较“鸡贼”的方法，numpy给我们提供了很多有关的函数，常用的函数有:
>1、创建指定大小的数组，数组元素以 0 来填充：
```python
numpy.zeros(shape, dtype =  float, order =  'C')
```
>2、创建指定形状的数组，数组元素以 1 来填充：
```python
·numpy.ones(shape, dtype =  None, order =  'C')
```
>3、根据start 与 stop 指定的范围以及 step 设定的步长，生成一个 ndarray。
```python
numpy.arange(start, stop, step, dtype)
"""
start：起始值，默认为`0`
stop：终止值（不包含）
step：步长，默认为`1`
dtype：返回`ndarray`的数据类型，如果没有提供，则会使用输入数据的类型。
"""
>>>np.arange(5)
[0 1 2 3 4]
>>>np.arange(10,20,2)
[10  12  14  16  18]
```
>4、numpy.linspace 函数用于创建一个一维数组，数组是一个等差数列构成的，格式如下：
```python
np.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)
"""
start:序列的起始值
stop:序列的终止值，如果`endpoint`为`true`，该值包含于数列中
num:要生成的等步长的样本数量，默认为`50`
endpoint:该值为 `ture` 时，数列中中包含`stop`值，反之不包含，默认True。
retstep:如果为True时，生成的数组中会显示间距，反之不显示。
dtype:ndarray的数据类型
"""
>>>np.linspace(1,10,10)
[  1.  2.  3.  4.  5.  6.  7.  8.  9.  10.]
>>>np.linspace(1,1,10)
[1.  1.  1.  1.  1.  1.  1.  1.  1.  1.]
```
## 3、通过random方式创建
>1、 numpy.random.rand()
```python
numpy.random.rand(d0,d1,…,dn)
"""
rand函数根据给定维度生成[0,1)之间的数据，包含0，不包含1
dn表格每个维度
返回值为指定维度的array
"""
>>>np.random.rand(4,2)
array([[ 0.02173903, 0.44376568],
       [ 0.25309942, 0.85259262],
       [ 0.56465709, 0.95135013],
       [ 0.14145746, 0.55389458]])
```
>2、numpy.random.randn()
```python
numpy.random.randn(d0,d1,…,dn)
"""
randn函数返回一个或一组样本，具有标准正态分布。
dn表格每个维度
返回值为指定维度的array
"""
>>>np.random.randn() # 当没有参数时，返回单个数据
-1.1241580894939212
```
>3、numpy.random.randint()
```python
numpy.random.randint(low, high=None, size=None, dtype=’l’)
"""
返回随机整数，范围区间为[low,high），包含low，不包含high
参数：low为最小值，high为最大值，size为数组维度大小，dtype为数据类型，默认的数据类型是np.int
high没有填写时，默认生成随机数的范围是[0，low)
"""
>>>np.random.randint(1,size=5) # 返回[0,1)之间的整数，所以只有0	
array([0, 0, 0, 0, 0])
>>>np.random.randint(-5,5,size=(2,2))
array([[ 2, -1],    
	   [ 2, 0]])
```
>5、numpy.random.choice()
```python
numpy.random.choice(a, size=None, replace=True, p=None)
"""
从给定的一维数组中生成随机数
参数： a为一维数组类似数据或整数；size为数组维度；p为数组中的数据出现的概率
a为整数时，对应的一维数组为np.arange(a)
"""	
>>>np.random.choice(5,3)
array([4, 1, 4])
  
# 当replace为False时，生成的随机数不能有重复的数值
>>>np.random.choice(5, 3, replace=False)

"""
参数p的长度与参数a的长度需要一致；
参数p为概率，p里的数据之和应为1
"""
>>>demo_list = ['lenovo', 'sansumg','moto','xiaomi', 'iphone']
>>>np.random.choice(demo_list,size=(3,3), p=[0.1,0.6,0.1,0.1,0.1])
array([['sansumg', 'sansumg', 'sansumg'],
       ['sansumg', 'sansumg', 'sansumg'],
	   ['sansumg', 'xiaomi', 'iphone']],
	   dtype='<U7')
```
>6、numpy.random.seed()
```python
"""
np.random.seed()的作用：使得随机数据可预测。
当我们设置相同的seed，每次生成的随机数相同。如果不设置seed，则每次会生成不同的随机数
"""
>>>np.random.seed(1676)
>>>np.random.rand(5)
array([ 0.39983389,  0.29426895,  0.89541728,  0.71807369,  0.3531823 ])
>>>np.random.seed(0)
>>>np.random.rand(5)
array([ 0.5488135 , 0.71518937, 0.60276338, 0.54488318, 0.4236548 ])
#随机种子相同，生成的数据相同
>>>np.random.seed(1676)
>>>np.random.rand(5)
array([ 0.39983389,  0.29426895,  0.89541728,  0.71807369,  0.3531823 ])
```
