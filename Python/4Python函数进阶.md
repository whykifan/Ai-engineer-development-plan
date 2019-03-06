# Python函数进阶

<!-- TOC -->
- [Python函数进阶](#python函数进阶)
- [1高阶函数](#1高阶函数)
- [2map、reduce与filter(掌握)](#2mapreduce与filter掌握)
- - [2.1map()](#21map)
- - [2.2reduce()](#22reduce)
-  - [2.3filter()](#23filter)
- [3sorted](#3sorted)
- [4匿名函数(掌握)](#4匿名函数掌握)
- [5装饰器(掌握)](#5装饰器掌握)
<!-- /TOC -->


## 1高阶函数
对于Python内置的求绝对值的函数，调用函数可以使用以下代码：
```python
>>> abs(-10)  
10
```
但是，当我们只写`abs`时：
```python
>>> abs  
<built-in function abs>
```
由此可得`abs(-10)`是函数的调用，`abs`才是函数本身，在`Python函数当中`我们有使用把函数本身赋值给变量的方法：
```python
>>> f = abs  
>>> f  
<built-in function abs>
```
结论：函数本身也可以赋值给变量，即：`变量可以指向函数`。
在函数赋值给变量以后，我们便可以通过这个变量来调用这个函数：
```python
>>> f = abs  
>>> f(-10)  
10
```
函数名是什么呢？函数名其实就是指向函数的变量！对于 abs()这个函数，完全可以把函数名`abs`看成变量，它指向一个可以计算绝对值的函数！

>**既然变量可以指向函数，函数的参数能接收变量，那么一个函数就可以  
接收另一个函数作为参数，这种函数就称之为高阶函数。**

## 2map、reduce与filter(掌握)
Python内建了map()、reduce()与filter()函数,这几个函数在对列表的操作方面运用也极为广泛，可以方便的将一个函数作用于列表当中的每一个元素，使用十分高效。
### 2.1map()
map()函数接受两个参数，一个是函数，一个是Iterable，map将传入的函数依次作用到序列的每个元素，并把结果作为新的Iterator返回。

比如我们有一个函数f(x)=x2，要把这个函数作用在一个 list [1, 2, 3, 4, 5, 6, 7, 8, 9]上，就可以用map()实现如下： 
```python
>>> def f(x):  
	 return x * x  
>>> r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])  
>>> list(r)  
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```
在这里说明一下由于map函数返回的是一个Iterator，Iterator是一种惰性序列，如果我们想要得到的结果仍然是一个list的形式，我们便需要使用list()将其整个序列都计算返回一个list。
上述过程当然也可以使用for循环的形式实现：
```python
L = []  
for n in [1, 2, 3, 4, 5, 6, 7, 8, 9]:  
	L.append(f(n))  
print(L)
```
但是相比map()函数，在函数比较复杂的时候，代码量会增加很多，并且不容易看出来循环的目的是什么，而使用map函数会方便很多，例如将数字都转化为字符串，只需要一行代码即可：
```python
>>> list(map(str, [1, 2, 3, 4, 5, 6, 7, 8, 9]))  
['1', '2', '3', '4', '5', '6', '7', '8', '9']
```
### 2.2reduce()
reduce把一个函数作用在一个序列上面，这个函数必要接受两个参数，reduce把结果继续和序列的下一个元素做累计运算，效果即为:
```python
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```
例如对序列求和：
```python
>>> from functools import reduce  
>>> def add(x, y):  
	   return x + y  
>>> reduce(add, [1, 3, 5, 7, 9])  
25
```
### 2.3filter()
filter()函数用于过滤序列，与map函数类似，filter也接收一个函数与一个序列，与map不同的是filter把传入的函数依次作用于每个元素，然后根据返回值是Ture还是Flase决定保留还是丢弃该元素。

例如，在一个 list 中，删掉偶数，只保留奇数，可以这么写：
```python
>>>def is_odd(n):  
	return n % 2 == 1
>>>list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))
[1, 5, 9, 15]
```
把一个序列中的空字符串删掉，可以这么写：
```python
>>>def not_empty(s):  	
	  return s and s.strip()  
>>>list(filter(not_empty, ['A', '', 'B', None, 'C', ' ']))  
['A', 'B', 'C']
```
注意到filter()函数返回的是一个Iterator，也就是一个惰性序列，所以要强迫 filter()完成计算结果，需要用list()函数获得所有结果并返回list。
## 3sorted
排序也是在程序中经常用到的算法。无论使用冒泡排序还是快速排序，排序的核心是比较两个元素的大小。如果是数字，我们可以直接比较，但如果是字符串或者两个 dict 呢？直接比较数学上的大小是没有意义的，因此，比较的过程必须通过函数抽象出来。通常规定，对于两个元素x和y，如果认为 x < y，则返回-1，如果认为 x == y，则返回0，如果认为 x > y，则返回 1，这样，排序算法就不用关心具体的比较过程，而是根据比较结果直接排序。

Python内置的sorted()函数就可以对list进行排序：
```python
>>> sorted([36, 5, -12, 9, -21])  
[-21, -12, 5, 9, 36]
```
此外，sorted()函数也是一个高阶函数，它还可以接收一个key函数来实现自定义的排序，例如按绝对值大小排序：
```python
>>> sorted([36, 5, -12, 9, -21], key=abs)  
[5, 9, -12, -21, 36]
```
key指定的函数将作用于list的每一个元素上，并根据 key 函数返回的结果进行排序。

再看一个字符串排序的例子：  
```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'])  
['Credit', 'Zoo', 'about', 'bob']
```
默认情况下，对字符串排序，是按照ASCII的大小比较的，由于'Z' < 'a'，结果，大写字母Z会排在小写字母a的前面。现在，我们提出排序应该忽略大小写，按照字母序排序。要实现这个算法，不必对现有代码大加改动，只要我们能用一个key函数把字符串映射为忽略大小写排序即可:
```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower)  
['about', 'bob', 'Credit', 'Zoo']
```
要进行反向排序，不必改动key函数，可以传入第三个参数 reverse=True：
```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower,  
reverse=True)  
['Zoo', 'Credit', 'bob', 'about']
```
从上述例子可以看出，高阶函数的抽象能力是非常强大的，而且核心代码可以保持得非常简洁。
## 4匿名函数(掌握)
在 Python 中，对匿名函数提供了有限支持。还是以 map()函数为例，计算 f(x)=x2 时，除了定义一个f(x)的函数外，还可以直接传入匿名函数：
```python
>>> list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))  
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```
通过对比可以看出，匿名函数 lambda x: x * x实际上就是：
```python
def f(x):  
	return x * x
```
关键字 lambda 表示匿名函数，冒号前面的 x 表示函数参数。匿名函数有个限制，就是只能有一个表达式，不用写return，返回值就是该表达式的结果。

用匿名函数有个好处，因为函数没有名字，不必担心函数名冲突。此外，匿名函数也是一个函数对象，也可以把匿名函数赋值给一个变量，再利用变量来调用该函数：
```python
>>> f = lambda x: x * x  
>>> f  
<function <lambda> at 0x101c6ef28>  
>>> f(5)  
25
```
同样，也可以把匿名函数作为返回值返回，比如：
```python
def build(x, y):  
	return lambda: x * x + y * y
```
## 5装饰器(掌握)
由于函数也是一个对象，而且函数对象可以被赋值给变量，所以，通过变量也能调用该函数。
```python
>>> def now():  
	 print('2015-3-25')    
>>> f = now  
>>> f()  
2015-3-25
```
函数对象有一个__name__属性，可以拿到函数的名字：  
```python
>>> now.__name__  
'now'  
>>> f.__name__  
'now
```
**假设我们要增强 now()函数的功能，比如，在函数调用前后自动打印日志，但又不希望修改 now()函数的定义，这种在代码运行期间动态增加功能的方式，称之为“装饰器”(Decorator)。**

**decorator**就是一个返回函数的高阶函数。所以，我们要定义一个能打印日志的 decorator，可以定义如下：
```python
def log(func):  
	def wrapper(*args, **kw):  
		print('call %s():' % func.__name__)  
		return func(*args, **kw)  
	return wrapper
```
观察上面的`log`，因为它是一个`decorator`，所以接受一个**函数**作为参数，并返回一个**函数**。我们要借助Python的`@`语法，把`decorator`置于函数的定义处：
```python
@log  
def now():  
	print('2015-3-25')
```
调用 now()函数，不仅会运行now()函数本身，还会在运行 now()函数前打印一行日志：
```python
>>> now()  
call now():  
2015-3-25
```
把@log放到 now()函数的定义处，相当于执行了语句：  
```python
now = log(now)
```
由于 log()是一个decorator，返回一个函数，所以，**原来的 now()函数仍然存在，只是现在同名的now变量指向了新的函数，于是调用 now()将执行新函数，即在log()函数中返回wrapper()函数**。wrapper()函数的参数定义是(*args, **kw)，因此， wrapper()函数可以接受任意参数的调用。在 wrapper()函数内，首先打印日志，再紧接着调用原始函数。

如果 decorator本身需要传入参数，那就需要编写一个返回decorator的高阶函数，写出来会更复杂。比如，要自定义log的文本：
```python
def log(text):
	def decorator(func):  
		def wrapper(*args, **kw):  
			print('%s %s():' % (text, func.__name__))  
			return func(*args, **kw)  
		return wrapper  
	return decorator
```
这个 3 层嵌套的 decorator 用法如下：  
```python
@log('execute')  
def now():  
	print('2015-3-25')  
```
执行结果如下：  
```python
>>> now()  
execute now():  
2015-3-25  
```
和两层嵌套的 decorator 相比，3层嵌套的效果是这样的：  
```python
>>> now = log('execute')(now)
```
我们来剖析上面的语句，首先执行log('execute')，返回的是decorator函数，再调用返回的函数，参数是now函数，返回值最终是wrapper函数。

以上两种 decorator 的定义都没有问题，但还差最后一步。因为我们讲了函数也是对象，它有__name__等属性，但你去看经过 decorator 装饰之后的函数，它们的__name__已经从原来的'now'变成了'wrapper'：
```python
>>> now.__name__  
'wrapper'
```
因为返回的那个 wrapper()函数名字就是'wrapper'，所以，需要把原始函数的__name__等属性复制到 wrapper()函数中，否则，有些依赖函数签名的代码执行就会出错。不需要编写wrapper.__name__ = func.__name__这样的代码，Python内置的 functools.wraps 就是干这个事的，所以，一个完整的 decorator的写法如下：
```python
import functools  
def log(func):  
	@functools.wraps(func)  
	def wrapper(*args, **kw):  
		print('call %s():' % func.__name__)  
		return func(*args, **kw)  
	return wrapper
```
针对带参数的:
```python
import functools  
def log(text):  
	def decorator(func):  
		@functools.wraps(func)  
		def wrapper(*args, **kw):  
			print('%s %s():' % (text, func.__name__))  
			return func(*args, **kw)  
		return wrapper  
	return decorator
```
**现在，只需记住在定义 wrapper()的前面加上@functools.wraps(func)即可**。
