
<!-- TOC -->

- [Pandas 数据结构](#pandas-数据结构)
    - [1、Series](#1series)
        - [Series的一些基本操作：](#series的一些基本操作)
        - [Series的向量化操作：](#series的向量化操作)
    - [2、DataFrame](#2dataframe)
        - [数据访问](#数据访问)

<!-- /TOC -->

# Pandas 数据结构

Pandas 常用的数据结构有两种：**Series** 和 **DataFrame**。这些数据结构构建在Numpy 数组之上，这意味他们有着很高的效率。

## 1、Series
Series 是一个带有**名称**和**索引**的一维数组，既然是数组，肯定要说到的就是数组中的元素类型，在 Series 中包含的数据类型可以是**整数、浮点、字符串、Python对象**等。
创建一个Series的基本用法如下:
```python
>>>import pandas as pd
>>>data=[18, 30, 25, 40]
>>>index =["Tom", "Bob", "Mary", "James"]
>>>pd.Series(data,index)
Tom 18  
Bob 30  
Mary 25  
James 40  
dtype: int64
```
data为数据，index为数据索引，**长度需要与data长度相同**, index参数是可省略的，你可以选择不输入这个参数。如果不带index参数，Pandas 会自动用默认index 进行索引，类似数组，索引值[0,...,len(data) - 1]。

对于参数index，我们可以进一步为它起一个名字:
```python
>>>pd.Series(data,index).index.name = "name"
name  
Tom   18  
Bob   30  
Mary  25  
James 40  
dtype: int64
```
同样了为了方便别人知道我们的Series是干什么的，我们也可以为Series起一个名字：
```python
>>>pd.Series(data,index).name = 'age'
name  
Tom   18  
Bob   30  
Mary  25  
James 40  
Name: age, dtype: int64
```
通过上面一系列的操作，我们对 Series 的结构上有了基本的了解，简单来说，**一个 Series 包括了 data、index 以及 name**。

### Series的一些基本操作：

**对 Series 的算术运算都是基于 index 进行的。我们可以用[加减乘除]（+ - * /）这样的运算符对两个 Series 进行运算，Pandas 将会根据索引index，对响应的数据进行计算，结果将会以浮点数的形式存储，以避免丢失精度。**

Series 包含了 **dict** 的特点，也就意味着可以使用与 dict 类似的一些操作。我们可以将 index 中的元素看成是 **dict** 中的 key。
```python
>>>person = pd.Series(data,index,dtype = float)
>>>person.get('Tom')
18.0
```
**此外，可以通过 **get** 方法来获取。通过这种方式的好处是当索引不存在时，不会抛出异常。**
```python
>>>person.get("Tom")
18.0
```
**Series 除了像 **dict** 外，也非常像 **ndarray**，这也就意味着可以采用切片操作。**
```python
#得到前三个元素
>>>person[:3]
name  
Tom   18.0  
Bob   30.0  
Mary  25.0  
Name: age, dtype: float64
# 获取年龄大于30的元素  
>>>person[person> 30]
name  
James 40.0  
Name: age, dtype: float64
```
### Series的向量化操作：
Series 与 ndarray 一样，也是支持向量化操作的。同时也可以传递给大多数期望 ndarray 的 NumPy 方法。
```python
>>>person +1 
name  
Tom   19.0  
Bob   31.0  
Mary  26.0  
James 41.0  
Name: age, dtype: float64
```
**对两个Series进行操作，如果Pandas 在两个 Series里找不到相同的 index，对应的位置就返回一个空值 NaN**

## 2、DataFrame
DataFrame 是一个带有**索引**的二维数据结构，每列可以有自己的名字，并且可以有不同的数据类型。你可以把它想象成一个 excel 表格或者数据库中的一张表，DataFrame 是最常用的 Pandas 对象。

**基本语法:**
```python
pandas.DataFrame( data, index, columns, dtype, copy)
```

 - data表示要传入的数据 ，包括 ndarray，series，map，lists，dict，constant和另一个DataFrame
	   
  - index和columns 行索引和列索引 
   
-   dtype:每列的类型

 - copy: 从input输入中拷贝数据，默认是false，不拷贝

**通过DataFrame你能很方便地处理数据。常见的操作比如选取、替换行或列的数据，还能重组数据表、修改索引、多重筛选等。**

**一个DataFrame的基本使用方法可以如下:**
```python
>>>index=pd.Index(data["Tom","Bob","Mary","James"],name="name")  
>>>data = {  
"age": [18, 30, 25, 40],  
"city": ["BeiJing", "ShangHai", "GuangZhou", "ShenZhen"]  
}  
>>>user_info = pd.DataFrame(data=data, index=index)  
>>>user_info
```
--|age|city
--|--|--
name|||
Tom|18|BeiJing|
Bob|30|ShangHai|
Mary|25|GuangZhou|
James|40|ShenZhen|

还可以通过另外一种方式来构建。**这种方式是先构建一个二维数组，然后再生成一个列名称列表：**
```python
>>>data = 
[[18, "BeiJing"],  
[30, "ShangHai"],  
[25, "GuangZhou"],  
[40, "ShenZhen"]]  
>>>columns = ["age", "city"]  
>>>user_info = pd.DataFrame(data=data, index=index, columns=columns)  
>>>user_info
```
--|age|city
--|--|--
name|||
Tom|18|BeiJing|
Bob|30|ShangHai|
Mary|25|GuangZhou|
James|40|ShenZhen|
### 数据访问
 **访问行**

在生成了 DataFrame 之后，可以看到，每一行就表示某一个用户的信息，假如想要访问某行，需要借助**loc**方法：
```python
>>>user_info.loc["Tom"]
```
age |18  
-|-
city |BeiJing  
Name: Tom, dtype: object

**除了直接通过索引名来访问某一行数据之外，还可以通过这行所在的位置来选择这一行:**
```python
>>>user_info.iloc[0]
```
age |18  
-|-
city |BeiJing  
Name: Tom, dtype: object

**如果想要访问多行，可以借助切片的方法轻松完成：**
```python
>>>user_info.iloc[1:3]
```
-|age|city
-|-|-
name||
Bob|30|ShangHai
Mary|25|GuangZhou

**访问列**
我们可以通过属性（“.列名”）的方式来访问该列的数据，也可以通过[column]的形式来访问该列的数据。
假如想获取所有用户的年龄，那么可以这样操作。
```python
>>>user_info.age
#或者user_info["age"]
```


name|-
--|--
Tom|18
Bob|30
Mary|25
James|40
Name: age, dtype: int64

**如果想要同时获取年龄和城市**：
```python
# 可以变换列的顺序  
>>>user_info[["city", "age"]]
```
--|city|age
--|--|--
name|||
Tom|BeiJing|18
Bob|ShangHai|30
Mary|GuangZhou|25
James|ShenZhen|40

**新增/删除列**
在生成了 DataFrame 之后，突然你发现好像缺失了用户的**性别**这个信息，那么如何添加呢？
如果所有的性别都一样，我们可以通过传入一个标量，Pandas 会自动帮我们广播来填充所有的位置。
```python
>>>user_info["sex"] = "male"  
>>>user_info
```
--|age|city|sex
--|--|--|--
name||||
Tom|18|BeiJing|male
Bob|30|ShangHai|male
Mary|25|GuangZhou|male
James|40|ShenZhen|male

**如果想要删除某一列，可以使用 pop 方法来完成。**
```python
>>>user_info.pop("sex")  
>>>user_info
```
--|age|city
--|--|--
name|||
Tom|18|BeiJing
Bob|30|ShangHai
Mary|25|GuangZhou
James|40|ShenZhen

>**除了pop方法外，如果想从 DataFrame 删除某一行或一列，可以用 .drop() 函数。在使用这个函数的时候，你需要先指定具体的删除方向，axis=0 对应的是行 row，而 axis=1 对应的是列 column 。**

如果用户的性别不一致的时候，我们可以通过传入一个 **like-list** 来添加新的一列:
```python
>>>user_info["sex"] = ["male", "male", "female", "male"]  
>>>user_info
```
--|age|city|sex
--|--|--|--
name||||
Tom|18|BeiJing|male
Bob|30|ShangHai|male
Mary|25|GuangZhou|female
James|40|ShenZhen|male

**通过上面的例子可以看出，我们创建新的列的时候都是在原有的 DataFrame 上修改的，也就是说如果添加了新的一列之后，原有的 DataFrame 会发生改变。如果想要保证原有的 DataFrame 不改变的话，我们可以通过 assign 方法来创建新的一列。**
```python
>>>user_info.assign(age_add_one = user_info["age"] + 1)
```
--|age|city|sex|age_add_one
--|--|--|--|--
name||||
Tom|18|BeiJing|male|19
Bob|30|ShangHai|male|31
Mary|25|GuangZhou|female|26
James|40|ShenZhen|male|41


