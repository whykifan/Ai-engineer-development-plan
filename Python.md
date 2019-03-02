---


---

<h1 id="ml学习之pythonbr"><strong>ML学习之Python</strong></h1>
<h2 id="font-colorcd00cd一、python基本数据类型fontbr"><font color="#CD00CD">一、Python基本数据类型</font></h2>
<h3 id="font-colorcd4f391.1python当中的字符串font"><font color="#CD4F39">1.1Python当中的字符串</font></h3>
<p>Python字符串要用单引号<code>'</code>或者双引号<code>"</code>括起来表示，使用反斜杠<code>\</code>来进行特殊字符的转义,由于Pthon字符串是以Unicode编码的，因此Python的字符串可以同时支持多种语言。单个字符的编码，Python提供了ord()函数获取字符的整数(ASCII码)表示，chr()函数把编码转换为对应的字符：</p>
<pre class=" language-python"><code class="prism  language-python">    enter code here
    <span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span><span class="token builtin">ord</span><span class="token punctuation">(</span><span class="token string">'A'</span><span class="token punctuation">)</span>
    <span class="token number">65</span>
	<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span><span class="token builtin">ord</span><span class="token punctuation">(</span><span class="token string">'中'</span><span class="token punctuation">)</span>
	<span class="token number">20013</span>
	<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span><span class="token builtin">chr</span><span class="token punctuation">(</span><span class="token number">66</span><span class="token punctuation">)</span>
	B
</code></pre>
<p>Python自带的len()函数可以计算每一个字符串含有多少个字符(不是字节数)</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span><span class="token builtin">len</span><span class="token punctuation">(</span><span class="token string">'ABC'</span><span class="token punctuation">)</span>
<span class="token number">3</span>
</code></pre>
<p>由于Python源代码也是一个文本文件，所以，当你的源代码中包含中文的时候，在保存源代码时，就需要务必指定保存为UTF-8编码。当Python解释器读取源代码时，为了让它按 UTF-8 编码读取，我们通常在文件开头写上这两行：</p>
<pre class=" language-python"><code class="prism  language-python">	<span class="token comment">#!/usr/bin/env python3</span>
	<span class="token comment"># -*- coding: utf-8 -*-</span>
</code></pre>
<p>第一行注释是为了告诉Linux/OS X系统，这是一个Python可执行程序，Windows系统会忽略这个注释。第二行注释是为了告诉Python解释器，按照UTF-8编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。</p>
<blockquote>
<p><font color="#8B4513">字符串的格式化</font></p>
</blockquote>
<p>我们经常会输出类似’亲爱的xxx你好！你xx月的话费是xx，余额是’xx’之类的字符串，而xxx的内容都是根据变量变化的，所以，需要一种简便的格式化字符串的方式。<strong>在Python中，采用的格式化方式和C语言是一致的，用%实现</strong>：</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span><span class="token string">'Hello, %s'</span> <span class="token operator">%</span> <span class="token string">'world'</span>
Hello<span class="token punctuation">,</span>world
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> <span class="token string">'Hi, %s, you have $%d.'</span> <span class="token operator">%</span> <span class="token punctuation">(</span><span class="token string">'Michael'</span><span class="token punctuation">,</span> <span class="token number">1000000</span><span class="token punctuation">)</span>
Hi<span class="token punctuation">,</span> Michael<span class="token punctuation">,</span> you have $<span class="token number">1000000</span><span class="token punctuation">.</span>
</code></pre>
<p><strong>在字符串内部，%s表示用字符串替换，%d表示用整数替换，有几个%占位符，后面就跟几个变量或者值，顺序要对应好。</strong></p>
<blockquote>
<p>常用的占位符：<br>
<font color="#8B4513"><br>
%d 整数<br>
%f 浮点数<br>
%s 字符串<br>
%x 十六进制整数<br>
</font></p>
</blockquote>
<h3 id="font-colorcd4f391.2python的listfont"><font color="#CD4F39">1.2Python的list:</font></h3>
<p>Python内置的一种数据类型是列表，其也是Python当中使用最多的一种数据类型。list是一种有序的集合，可以随时添加和删除其中的元素,<strong>访问元素是从索引0开始，也可以只用使用负数做索引(由-1开始，访问最后一个元素)，超出索引范围会报一个IndexError错误</strong>。</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span>data <span class="token operator">=</span> <span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">,</span><span class="token number">2</span><span class="token punctuation">,</span><span class="token number">3</span><span class="token punctuation">,</span><span class="token string">'hello'</span><span class="token punctuation">]</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span><span class="token keyword">print</span><span class="token punctuation">(</span>data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">,</span>data<span class="token punctuation">[</span><span class="token operator">-</span><span class="token number">1</span><span class="token punctuation">]</span><span class="token punctuation">)</span>
<span class="token number">1</span><span class="token punctuation">,</span>hello
</code></pre>
<p>list 是一个可变的有序表，所以，可以往list中追加元素到末尾：</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> data<span class="token punctuation">.</span>append<span class="token punctuation">(</span><span class="token string">'Adam'</span><span class="token punctuation">)</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> data
<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">,</span><span class="token number">2</span><span class="token punctuation">,</span><span class="token number">3</span><span class="token punctuation">,</span><span class="token string">'hello'</span><span class="token punctuation">,</span><span class="token string">'Adam'</span><span class="token punctuation">]</span>
</code></pre>
<p>也可以把元素插入到指定的位置，比如索引号为1的位置：</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> data<span class="token punctuation">.</span>insert<span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token string">'3'</span><span class="token punctuation">)</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> data
<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">,</span><span class="token number">3</span><span class="token punctuation">,</span><span class="token number">2</span><span class="token punctuation">,</span><span class="token number">3</span><span class="token punctuation">,</span><span class="token string">'hello'</span><span class="token punctuation">,</span><span class="token string">'Adam'</span><span class="token punctuation">]</span>
</code></pre>
<p>要删除list末尾的元素，用pop()方法,要删除指定位置的元素，用pop(i)方法:</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> data<span class="token punctuation">.</span>pop<span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">)</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> data
<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">,</span><span class="token number">2</span><span class="token punctuation">,</span><span class="token number">3</span><span class="token punctuation">,</span><span class="token string">'hello'</span><span class="token punctuation">,</span><span class="token string">'Adam'</span><span class="token punctuation">]</span>
</code></pre>
<h3 id="font-colorcd4f391.3python的tuple元组font"><font color="#CD4F39">1.3Python的tuple(元组):</font></h3>
<p>tuple也是Python当中的一种有序列表，与list非常类似，但是tuple一旦创立便不可以更改，并且没有append()， insert()这样的方法，元素的获取方法与list一样，但是当中元素不可以被赋值。当你定义一个tuple时，其中的元素就必须要确定下来:</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span>t <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">,</span><span class="token number">2</span><span class="token punctuation">)</span>
<span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">,</span><span class="token number">2</span><span class="token punctuation">)</span>
</code></pre>
<p>但是当定义只有一个元素的tuple时：</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span>t <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">)</span>
<span class="token number">1</span>
</code></pre>
<p>**此时定义的表示一个tuple，而是1这个数！**这是因为括号()既可以表示tuple又可以表示数学公式当中的小括号，因此Python规定，<font color="#CD2990"><strong>定义只有一个元素的tuple时，必须加上一个逗号以消除歧义</strong></font>：</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span>t <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">,</span><span class="token punctuation">)</span>
<span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">,</span><span class="token punctuation">)</span>
</code></pre>
<blockquote>
<p><font color="#CD2990">tuple的特殊情况</font>：<br>
当tuple中含有list元素时，list当中的元素时可以改变的：</p>
</blockquote>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span>t <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">,</span><span class="token number">3</span><span class="token punctuation">,</span><span class="token punctuation">[</span><span class="token string">"a"</span><span class="token punctuation">,</span><span class="token string">"b"</span><span class="token punctuation">]</span><span class="token punctuation">)</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span>t<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token string">"c"</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span>t
<span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">,</span><span class="token number">3</span><span class="token punctuation">,</span><span class="token punctuation">[</span><span class="token string">"c"</span><span class="token punctuation">,</span><span class="token string">"b"</span><span class="token punctuation">]</span><span class="token punctuation">)</span>
</code></pre>
<blockquote>
<p><font color="#9B30FF">tuple的不变应该理解为tuple元素指向的不变，虽然list当中元素变了，但是仍为指向的这一个list。</font></p>
</blockquote>
<h3 id="font-colorcd4f391.4python的dict字典font"><font color="#CD4F39">1.4Python的dict(字典):</font></h3>
<p>Python内置了字典：dict的支持，dict全称dictionary，在其他语言中也称为map，使用键-值(key-value)存储，具有极快的查找速度。</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> d <span class="token operator">=</span> <span class="token punctuation">{</span><span class="token string">'Michael'</span><span class="token punctuation">:</span> <span class="token number">95</span><span class="token punctuation">,</span> <span class="token string">'Bob'</span><span class="token punctuation">:</span> <span class="token number">75</span><span class="token punctuation">,</span> <span class="token string">'Tracy'</span><span class="token punctuation">:</span> <span class="token number">85</span><span class="token punctuation">}</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> d<span class="token punctuation">[</span><span class="token string">'Michael'</span><span class="token punctuation">]</span>
<span class="token number">95</span>
</code></pre>
<p>将数据放入字典的方法除了初始化指定意外，也可以通过key放入，并且字典中元素的值是可以改变的：</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> d<span class="token punctuation">[</span><span class="token string">'Jack'</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">88</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> <span class="token keyword">print</span><span class="token punctuation">(</span>d<span class="token punctuation">)</span>
<span class="token punctuation">{</span><span class="token string">'Michael'</span><span class="token punctuation">:</span> <span class="token number">95</span><span class="token punctuation">,</span> <span class="token string">'Bob'</span><span class="token punctuation">:</span> <span class="token number">75</span><span class="token punctuation">,</span> <span class="token string">'Tracy'</span><span class="token punctuation">:</span> <span class="token number">85</span><span class="token punctuation">,</span> <span class="token string">'Jack'</span><span class="token punctuation">:</span> <span class="token number">88</span><span class="token punctuation">}</span>
</code></pre>
<p>判断key是否存在，可以通过in判断，或者用dict提供的get方法，get方法可以指定key不存在时的返回值：</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> <span class="token string">'Thomas'</span> <span class="token keyword">in</span> d
<span class="token boolean">False</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> d<span class="token punctuation">.</span>get<span class="token punctuation">(</span><span class="token string">'Thomas'</span><span class="token punctuation">)</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> d<span class="token punctuation">.</span>get<span class="token punctuation">(</span><span class="token string">'Thomas'</span><span class="token punctuation">,</span> <span class="token operator">-</span><span class="token number">1</span><span class="token punctuation">)</span>
<span class="token operator">-</span><span class="token number">1</span>
</code></pre>
<p>删除一个key可以使用pop(key)方法，对应的value也会从dict中删除：</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> d<span class="token punctuation">.</span>pop<span class="token punctuation">(</span><span class="token string">'Bob'</span><span class="token punctuation">)</span>
<span class="token number">75</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> d
<span class="token punctuation">{</span><span class="token string">'Michael'</span><span class="token punctuation">:</span> <span class="token number">95</span><span class="token punctuation">,</span> <span class="token string">'Tracy'</span><span class="token punctuation">:</span> <span class="token number">85</span><span class="token punctuation">}</span>
</code></pre>
<blockquote>
<p><font color="#CD3333"><strong>Special Warning:字典当中内容的存放顺序，与一开始的key的顺序无关</strong></font></p>
<blockquote>
<p>与list相比字典的特点：</p>
</blockquote>
</blockquote>
<ol>
<li>查找和插入的速度极快，不会随着 key 的增加而增加；</li>
<li>需要占用大量的内存，内存浪费多。</li>
</ol>
<p><strong>dict可以用在需要高速查找的很多地方，在Python代码中几乎无处不在，正确使用dict非常重要，需要牢记的第一条就是dict的key必须是不可变对象。这是因为dict根据key来计算value的存储位置，如果每次计算相同的key得出的结果不同，那dict内部就完全混乱了。这个通过key计算位置的算法称为哈希算法（ Hash）。要保证hash的正确性，作为key的对象就不能变。在Python中，字符串、整数等都是不可变的，因此，可以放心地作为key。而list是可变<br>
的，就不能作为key</strong></p>
<h3 id="font-colorcd4f391.5python的set集合font"><font color="#CD4F39">1.5Python的set(集合):</font></h3>
<p>set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以在set中，没有重复的key。<br>
要创建一个 set，需要提供一个 list 作为输入集合：</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> s <span class="token operator">=</span> <span class="token builtin">set</span><span class="token punctuation">(</span><span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">]</span><span class="token punctuation">)</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> s
<span class="token punctuation">{</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">}</span>
</code></pre>
<blockquote>
<p>传入的参数[1, 2, 3]是一个 list，而显示的{1, 2, 3}只是告诉你这个set内部有1，2，3这3个元素，显示的顺序也不表示set是有序的重复的元素在set当中会自动被过滤。</p>
</blockquote>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> s <span class="token operator">=</span> <span class="token builtin">set</span><span class="token punctuation">(</span><span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">]</span><span class="token punctuation">)</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> s
<span class="token punctuation">{</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">}</span>
</code></pre>
<p>通过**add(key)**方法可以添加元素到 set 中，可以重复添加，但不会有效果</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> s<span class="token punctuation">.</span>add<span class="token punctuation">(</span><span class="token number">4</span><span class="token punctuation">)</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> s
<span class="token punctuation">{</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">,</span> <span class="token number">4</span><span class="token punctuation">}</span>
</code></pre>
<p>set可以看成数学意义上的无序和无重复元素的集合，因此两个set可以做数学意义上的交集、并集等操作：</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> s1 <span class="token operator">=</span> <span class="token builtin">set</span><span class="token punctuation">(</span><span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">]</span><span class="token punctuation">)</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> s2 <span class="token operator">=</span> <span class="token builtin">set</span><span class="token punctuation">(</span><span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">,</span> <span class="token number">4</span><span class="token punctuation">]</span><span class="token punctuation">)</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> s1 <span class="token operator">&amp;</span> s2
<span class="token punctuation">{</span><span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">}</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> s1 <span class="token operator">|</span> s2
<span class="token punctuation">{</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">,</span> <span class="token number">4</span><span class="token punctuation">}</span>
</code></pre>
<h3 id="font-colorcd2626再议可变与不可变对象font"><font color="#CD2626"><strong>再议可变与不可变对象:</strong></font></h3>
<p>对于可变对象，比如list，对list进行操作，list内部的内容是会变化的，比如：</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> a <span class="token operator">=</span> <span class="token punctuation">[</span><span class="token string">'c'</span><span class="token punctuation">,</span> <span class="token string">'b'</span><span class="token punctuation">,</span> <span class="token string">'a'</span><span class="token punctuation">]</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> a<span class="token punctuation">.</span>sort<span class="token punctuation">(</span><span class="token punctuation">)</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> a
<span class="token punctuation">[</span><span class="token string">'a'</span><span class="token punctuation">,</span> <span class="token string">'b'</span><span class="token punctuation">,</span> <span class="token string">'c'</span><span class="token punctuation">]</span>
</code></pre>
<p>而对于不可变对象，比如str，对str进行操作呢：</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> a <span class="token operator">=</span> <span class="token string">'abc'</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> a<span class="token punctuation">.</span>replace<span class="token punctuation">(</span><span class="token string">'a'</span><span class="token punctuation">,</span> <span class="token string">'A'</span><span class="token punctuation">)</span>
<span class="token string">'Abc'</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> a
<span class="token string">'abc'</span>
</code></pre>
<p>虽然字符串有个replace()方法，也确实变出了’Abc’，但变量a最后仍是’abc’，应该怎么理解呢？</p>
<blockquote>
<p><font color="#B452CD">要始终牢记的是， a 是变量，而’abc’才是字符串对象！当我们调用 a.replace(‘a’, ‘A’)时，实际上调用方法 replace是作用在字符串对象’abc’上的，而这个方法虽然名字叫 replace，但却没有改变字符串’abc’的内容。相反，replace方法创建了一个新字符串’Abc’并返回，如果我们用变量b指向该新字符串，就容易理解了，变量a仍指向原有的字符串’abc’，但变量b却指向新字符串’Abc’了:</font></p>
</blockquote>
<pre class=" language-python"><code class="prism  language-python"><span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> a <span class="token operator">=</span> <span class="token string">'abc'</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> b <span class="token operator">=</span> a<span class="token punctuation">.</span>replace<span class="token punctuation">(</span><span class="token string">'a'</span><span class="token punctuation">,</span> <span class="token string">'A'</span><span class="token punctuation">)</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> b
<span class="token string">'Abc'</span>
<span class="token operator">&gt;&gt;</span><span class="token operator">&gt;</span> a
'abc
</code></pre>
<blockquote>
<p><font color="#B452CD">所以，对于不变对象来说，调用对象自身的任意方法，也不会改变该对象自身的内容。相反，这些方法会创建新的对象并返回，这样，就保证了不可变对象本身永远是不可变的。</font></p>
</blockquote>
<p><strong>附：常用的Python数据类型转换</strong><br>
<img src="https://i.imgur.com/L2Y0qvr.png" alt="Python数据类型转换"></p>

