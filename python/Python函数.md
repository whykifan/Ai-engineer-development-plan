# Python函数式编程

<!-- TOC -->

- [Python函数式编程](#python函数式编程)
    - [1函数的调用](#1函数的调用)
    - [2函数的定义](#2函数的定义)
        - [2.1函数的返回值](#21函数的返回值)
    - [3函数的参数(重要!!!!)](#3函数的参数重要)
        - [3.1位置参数](#31位置参数)
        - [3.2默认参数](#32默认参数)
            - [<font size =7> *默认参数一个很大的坑    !!!*</font>](#font-size-7-默认参数一个很大的坑----font)
        - [3.3可变参数](#33可变参数)
        - [3.4关键字参数](#34关键字参数)
        - [3.5命名关键字参数](#35命名关键字参数)
        - [3.5参数组合](#35参数组合)

<!-- /TOC -->

## 1函数的调用
与其他编程语言相同，Python内置了许多的函数供我们直接调用，调用函数需要我们知道函数名与参数，比如`abs`函数:
```python
>>>abs(-10)
10
```
<font size=4>调用函数的时候，如果传入的参数数量不对或者参数的数据类型不对，会报 `TypeError`的错误，并且会明确告诉你传入参数错误：</font>
```python
>>>abs(10,1)
TypeError: abs() takes exactly one argument (2 given)
>>>abs('a')
TypeError: bad operand type for abs(): 'str'
```
Python当中，函数名其实就是指向一个函数对象的引用，完全可以把函数名赋给一个  
变量，相当于给这个函数起了一个“别名”：
```python
>>> a = abs			 # 变量a指向abs函数  
>>> a(-1) 		     # 所以也可以通过a调用abs函数  
1
```
## 2函数的定义
在Python中，定义一个函数要使用`def`语句，依次写出函数名、括号、括号中的参数和冒号，然后在缩进块中编写函数体，函数的返回值用`return`语句返回。
```python
def my_abs(x):  
	if x >= 0:  
		return x  
	else:  
		return -x
```
一旦执行到return时，函数就执 行完毕，并将结果返回。因此，函数内部通过条件判断和循环可以实现非常复杂的逻辑。如果没有`return`语句，函数执行完毕后也会返回结果，只是结果为 None。**`return None`可以简写为 `return`**。
>**如果你已经把`my_abs()`的函数定义保存为`abstest.py`文件了，那么可 以在该文件的当前目录下启动Python 解释器，用`from abstest import  my_abs`来导入`my_abs()`函数，注意 `abstest`是文件名（不含.py扩展名）**。

如果想定义一个什么事也不做的空函数,可以使用`pass`语句：
```python
def nop():  
	pass
```
pass 语句什么都不做，那有什么用？实际上pass可以用来作为占位符,比如现在还没想好怎么写函数的代码,就可以先放一个 pass，让代码能运行起来。
### 2.1函数的返回值
Python的函数可以同时返回多个返回值：
```python
import math  
def move(x, y, step, angle=0):  
	nx = x + step * math.cos(angle)  
	ny = y - step * math.sin(angle)  
	return nx, ny
```
同时获取两个返回值：
```python
>>> x, y = move(100, 100, 60, math.pi / 6)  
>>> print(x, y)  
151.96152422706632 70.0
```
**但其实这只是一种假象，Python 函数返回的仍然是单一值**：
```python
>>> r = move(100, 100, 60, math.pi / 6)  
>>> print(r)  
(151.96152422706632, 70.0)
```
原来返回值是一个`tuple`！但是，在语法上，返回一个`tuple`可以省略括 号，而多个变量可以同时接收一个`tuple`，按位置赋给对应的值，所以，Python 的函数返回多值其实就返回一个`tuple`，但写起来更方便。
## 3函数的参数(重要!!!!)
Python的函数定义非常简单，但灵活度却非常大。除了正常定义的必选参数外，还可以使用默认参数、可变参数和关键字参数，使得函数定义出来的接口，不但能处理复杂的参数，还可以简化调用者的代码。
### 3.1位置参数
我们先写一个计算x^2的函数：
```python
def power(x):  
	return x * x
```
对于power(x)函数，参数x就是一个位置参数。当我们调用power函数时，必须传入有且仅的一个参数x。当我们想要计算x的n次方时，我们可以将函数改为:
```python
def power(x, n):  
	s = 1  
	while n > 0:  
		n = n - 1  
		s = s * x  
	return s
```
修改后的power(x, n)函数有两个参数:x和n，这两个参数都是位置参数，调用函数时，传入的两个值按照位置顺序依次赋给参数x和n,这就是位置参数。
### 3.2默认参数
新的power(x, n)函数定义没有问题，但是旧的调用代码失败了，原因是我们增加了一个参数，导致旧的代码因为缺少一个参数而无法正常调用：
```python
>>> power(5)  
Traceback (most recent call last):  
File "<stdin>", line 1, in <module>  
TypeError: power() missing 1 required positional argument: 'n
```
Python的错误信息很明确:调用函数 power()缺少了一个位置参数n。这个时候，默认参数就排上用场了。由于我们经常计算$x^2$，所以完全可以把第二个参数n的默认值设定为 2：
```python
def power(x, n=2):  
	s = 1
	while n > 0:  
		n = n - 1  
		s = s * x  
	return s
```
这样当我们调用 power(5)时，相当于调用power(5, 2)：
```python
>>> power(5)  
25  
>>> power(5, 2)  
25
```
而对于n > 2的其他情况就必须明确地传入n，比如 power(5, 3)。设置默认参数时，有几点要注意：

 1. **必选参数在前，默认参数在后，否则 Python 的解释器会报错（思考一下为什么默认参数不能放在必选参数前面）；**
 2. **如何设置默认参数: 当函数有多个参数时，把变化大的参数放前面，变化小的参数放后面。 变化小的参数就可作为默认参数。**
 
 使用默认参数有什么好处？最大的好处是能降低调用函数的难度。
#### <font size =7> *默认参数一个很大的坑    !!!*</font>
**默认参数很有用，但使用不当，也会掉坑里。默认参数有个最大的坑：**
eg:
```python
def add_end(L=[]):  
	L.append('END')  
	return L
```
当你正常调用时，结果似乎不错：
```python
>>> add_end([1, 2, 3])  
[1, 2, 3, 'END']  
>>> add_end(['x', 'y', 'z'])  
['x', 'y', 'z', 'END']
```
当你使用默认参数调用时，一开始结果也是对的：
```python
>>> add_end()  
['END'] 
```
但是，再次调用 add_end()时，结果就不对了：
```python
>>> add_end()  
['END', 'END']  
>>> add_end()  
['END', 'END', 'END']
```
很多初学者很疑惑，默认参数是[]，但是函数似乎每次都“记住了”上次添加了'END'后的 list。

**原因解释如下:**
**Python函数在定义的时候，默认参数L的值就被计算出来了，即[]，因为默认参数L也是一个变量，它指向对象[]，每次调用该函数，如果改变了L的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的[]了。**

**所以，定义默认参数要牢记一点：默认参数必须指向不变对象！**

要修改上面的例子，我们可以用 None 这个不变对象来实现：
```python
def add_end(L=None):  
	if L is None:  
	L = []  
	L.append('END')  
	return L
```
现在，无论调用多少次，都不会有问题：  
```python
>>> add_end()  
['END']  
>>> add_end()  
['END']
```
### 3.3可变参数
在 Python 函数中，还可以定义可变参数。顾名思义，可变参数就是传入的参数个数是可变的，可以是 1 个、2 个到任意个，还可以是 0 个。

我们以数学题为例子，给定一组数字 a,b,c……请计算$a^2 + b^2 + c^2 + ……$
要定义出这个函数，我们必须确定输入的参数。由于参数个数不确定，  我们首先想到可以把 a,b,c……作为一个list或tuple传进来,这样,函数可以定义如下：
```python
def calc(numbers):  
	sum = 0  
	for n in numbers:  
		sum = sum + n * n  	
	return sum
```
但是调用的时候，需要先组装出一个list或tuple：  
```python
>>> calc([1, 2, 3])  
14
```
如果利用可变参数，调用函数的方式可以简化成这样：  
```python
>>> calc(1, 2, 3)
```
定义可变参数和定义一个list或tuple参数相比，仅仅在参数前面加了一 个*号。在函数内部,参数numbers接收到的是一个tuple，因此，函数代码完全不变。但是，调用该函数时，可以传入任意个参数，包括0个参数:
```python
def calc(*numbers):  
	sum = 0  
	for n in numbers:  
		sum = sum + n * n  
	return sum
>>> calc(1, 2)  
5
```
如果已经有一个list 或者 tuple，要调用一个可变参数怎么办？可以这样做：
```python
>>> nums = [1, 2, 3]  
>>> calc(nums[0], nums[1], nums[2])  
14
```
这种写法当然是可行的，问题是太繁琐，所以 Python 允许你在list或tuple前面加一个*号，把 list或 tuple的元素变成可变参数传进去：  
```python
>>> nums = [1, 2, 3]  
>>> calc(*nums)  
14
```
### 3.4关键字参数
关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict:
```python
def person(name, age, **kw):  
	print('name:', name, 'age:', age, 'other:', kw)
```
函数person除了必选参数name和age外，还接受关键字参数kw。在调用该函数时，可以只传入必选参数,也可以传入任意个数的关键字参数 ：
```python
>>> person('Michael', 30)  
name: Michael age: 30 other: {}
>>> person('Bob', 35, city='Beijing')  
name: Bob age: 35 other: {'city': 'Beijing'}
```
关键字参数有什么用？它可以扩展函数的功能。比如在 person 函数里， 我们保证能接收name和age这两个参数，但是，如果调用者愿意提供更多的参数，我们也能收到。
>和可变参数类似，也可以先组装出一个 dict，然后，把该dict 转换为关键字参数传进去：
>```python
>>>> extra = {'city': 'Beijing', 'job': 'Engineer'}  
>>>>person('Jack', 24, **extra)  
		>name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
>```
### 3.5命名关键字参数
对于关键字参数，函数的调用者可以传入任意不受限制的关键字参数。至于到底传入了哪些，就需要在函数内部通过kw检查，如果要限制关键字参数的名字，就可以用命名关键字参数，例如，只接收city和job作为关键字参数，这种方式定义的函数如下:
```python
def person(name, age, *, city, job):  
	print(name, age, city, job)
```
和关键字参数**kw 不同，命名关键字参数需要一个特殊分隔符*， *后面的参数被视为命名关键字参数。命名关键字参数必须传入参数名，这和位置参数不同。如果没有传入参数名，调用将报错：
```python
>>> person('Jack', 24, city='Beijing', job='Engineer')  
Jack 24 Beijing Engineer
>>> person('Jack', 24, 'Beijing', 'Engineer')  
Traceback (most recent call last):  
File "<stdin>", line 1, in <module>  
TypeError: person() takes 2 positional arguments but 4 were given
```
由于调用时缺少参数名city和job，Python 解释器把这 4 个参数均视为位置参数，但 person()函数仅接受 2 个位置参数。使用命名关键字参数时，要特别注意， \* 不是参数，而是特殊分隔符。 如果缺少 \*，Python 解释器将无法识别位置参数和命名关键字参数
### 3.5参数组合
在 Python 中定义函数，可以用必选参数、默认参数、可变参数、关键字参数和命名关键字参数，这5种参数都可以组合使用，除了可变参数无法和命名关键字参数混合。但是请注意，参数定义的顺序必须是：必选参数、默认参数、可变参数/命名关键字参数和关键字参数。
