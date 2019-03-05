# 文件读写

<!-- TOC -->

- [文件读写](#文件读写)
    - [读文件](#读文件)
    - [写文件](#写文件)
    - [操作文件与目录](#操作文件与目录)
- [waiting for code](#waiting-for-code)

<!-- /TOC -->

读写文件是最常见的IO操作。Python 内置了读写文件的函数，用法和C是兼容的。
读写文件前，我们先必须了解一下，在磁盘上读写文件的功能都是由操作系统提供的，现代操作系统不允许普通的程序直接操作磁盘，所以，读写文件就是请求操作系统打开一个文件对象（通常称为文件描述符），然后，通过操作系统提供的接口从这个文件对象中读取数据（读文件），或者把数据写入这个文件对象（写文件）。
## 读文件
要以读文件的模式打开一个文件对象，使用Python内置的open()函数，传入文件名和标示符：
```python
>>>f = open('/Users/michael/test.txt', 'r')
```
标示符'r'表示读，这样，我们就成功地打开了一个文件。
如果文件不存在， **open**()函数就会抛出一个IOError的错误，并且给出错误码和详细的信息告诉你文件不存在：
```python
>>> f=open('/Users/michael/notfound.txt', 'r')  
Traceback (most recent call last):  
File "<stdin>", line 1, in <module>  
FileNotFoundError: [Errno 2] No such file or directory:  
'/Users/michael/notfound.txt'
```
如果文件打开成功，接下来，调用**read**()方法可以一次读取文件的全部内容，Python把内容读到内存，用一个**str**对象表示： 
```python 
>>> f.read()  
'Hello, world!'
```
最后一步是调用**close**()方法关闭文件。文件使用完毕后必须关闭，因为文件对象会占用操作系统的资源，并且操作系统同一时间能打开的文件数量也是有限的：
```python
>>>f.close()
```
由于文件读写时都有可能产生 IOError，一旦出错，后面的 f.close()就不会调用，为了保证无论是否出错都可以调用close()方法，可以使用try....finally来实现：
```python
try:  
	f = open('/path/to/file', 'r')  
	print(f.read())  
finally:  
	if f:  
		f.close()
```
但是每次都这么写实在太繁琐，所以，Python 引入了with语句来自动帮我们调用close()方法：
```python
with open('/path/to/file', 'r') as f:  
	print(f.read())
```
调用**read**()会一次性读取文件的全部内容，如果文件有 10G，内存就爆了，所以，要保险起见，可以反复调用read(size)方法，每次最多读取size个字节的内容。另外，调用**readline**()可以每次读取一行内容，调用**readlines**()一次读取所有内容并按行返回**list**,**list当中每一个元素代表的是文件当中一行的内容，并且都是字符串类型**。因此，要根据需要决定怎么调用。
如果文件很小， read()一次性读取最方便；如果不能确定文件大小，反复调用read(size)比较保险；如果是配置文件，调用readlines()最方便：
```python  
for line in f.readlines():  
	print(line.strip()) # 把末尾的'\n'删掉
```
前面讲的默认都是读取文本文件，并且是 UTF-8 编码的文本文件。要读取二进制文件，比如图片、视频等等，用'rb'模式打开文件即可：
```python
>>> f = open('/Users/michael/test.jpg', 'rb')  
>>> f.read()  
b'\xff\xd8\xff\xe1\x00\x18Exif\x00\x00...' # 十六进制表示的字节
```
要读取非 UTF-8 编码的文本文件，需要给 open()函数传入encoding参数，例如，读取 GBK 编码的文件：
```python
>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk')  
>>> f.read()  
'测试
```
遇到有些编码不规范的文件，你可能会遇到UnicodeDecodeError，因为在  文本文件中可能夹杂了一些非法编码的字符。遇到这种情况，**open**()函数还接收一个**errors**参数，表示如果遇到编码错误后如何处理。最简单的方式是直接忽略：
```python
>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk',  
errors='ignore')
```
## 写文件
写文件和读文件是一样的，唯一区别是调用open()函数时，传入标识符'**w**'或者'**wb**'表示写文本文件或写二进制文件：  
```python
>>> f = open('/Users/michael/test.txt', 'w')  
>>> f.write('Hello, world!')  
>>> f.close()
```
你可以反复调用write()来写入文件，但是务必要调用 f.close()来关闭文件。当我们写文件时，**操作系统往往不会立刻把数据写入磁盘，而是放到内存缓存起来，空闲的时候再慢慢写入。只有调用close()方法时，操作系统才保证把没有写入的数据全部写入磁盘。忘记调用close()的后果是数据可能只写了一部分到磁盘，剩下的丢失了**。所以，还是用with语句来得保险：
```python
with open('/Users/michael/test.txt', 'w') as f:  
	f.write('Hello, world!')
```
要写入特定编码的文本文件，请给 open()函数传入 encoding 参数，将字符串自动转换成指定编码。
## 操作文件与目录
如果我们要操作文件、目录，可以在命令行下面输入操作系统提供的各种命令来完成。比如 dir、cp 等命令。其实操作系统提供的命令只是简单地调用了操作系统提供的接口函数， Python内置的**os**模块也可以直接调用操作系统提供的接口函数。

操作文件和目录的函数一部分放在 os 模块中，一部分放在 os.path 模块中，这一点要注意一下。查看、创建和删除目录可以这么调用：
```python
# 查看当前目录的绝对路径:  
>>> os.path.abspath('.')  
'/Users/michael'  
# 在某个目录下创建一个新目录，首先把新目录的完整路径表示出来:  
>>> os.path.join('/Users/michael', 'testdir')  
'/Users/michael/testdir'  
# 然后创建一个目录:  
>>> os.mkdir('/Users/michael/testdir')  
# 删掉一个目录:  
>>> os.rmdir('/Users/michael/testdir')
```
把两个路径合成一个时，不要直接拼字符串，而要通过 **os.path.join**()函数，**这样可以正确处理不同操作系统的路径分隔符**。在Linux/Unix/Mac下， **os.path.join**()返回这样的字符串：  
```python
part-1/part-2  
```
而 Windows 下会返回这样的字符串：  
```python
part-1\part-2
```
同样的道理，要拆分路径时，也不要直接去拆字符串，而要通**os.path.split**()函数，这样可以把一个路径拆分为两部分，后一部分总是最后级别的目录或文件名：
```python\
>>> os.path.split('/Users/michael/testdir/file.txt')  
('/Users/michael/testdir', 'file.txt')
```
**os.path.splitext**()可以直接让你得到文件扩展名，很多时候非常方便：
  ```python
>>> os.path.splitext('/path/to/file.txt')  
('/path/to/file', '.txt')
```
最后看看如何利用Python的特性来过滤文件。比如我们要列出当前目录下的所有目录，只需要一行代码：
```python
>>> [x for x in os.listdir('.') if os.path.isdir(x)]  
['.lein', '.local', '.m2', '.npm', '.ssh', '.Trash', '.vim',  
'Applications', 'Desktop', ...]
```
要列出所有的.py 文件，也只需一行代码：
```python
>>> [x for x in os.listdir('.') if os.path.isfile(x) and  
os.path.splitext(x)[1]=='.py']  
['apis.py', 'config.py', 'models.py', 'pymonitor.py', 'test_db.py',  
'urls.py', 'wsgiapp.py']
```

附：
[File文件方法](http://www.runoob.com/python/file-methods.html)
[os文件/目录方法](http://www.runoob.com/python/os-file-methods.html)
[os.path()模块](http://www.runoob.com/python/python-os-path.html)



## 一个常用的遍历目录下所有数据文件的方法：

```python
# waiting for code
```
