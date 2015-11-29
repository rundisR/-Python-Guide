# 写给身边同学们的Python Guide

## 目录：
[0. 为什么写这篇文 ............................. Why I write this passage](#0)<br>
[1. 谁应该看这篇文 ............................. Who should read this passage](#1)<br>
[2. 准备工作 ........................................ Preparation](#2)<br>
........ [2.1 适合的解释器 ...................... A suitable interpreter](#2.1)<br>
........ [2.2 CPython的安装方式 ............ Installation of CPython](#2.2)<br>
........ [2.3 适合的编辑器 ...................... A suitable editor](#2.3)<br>
........ [2.4 Atom安装+设置 ................... Atom's installation & settings](#2.4)<br>
[3. 什么是Python ................................. Literal meaning of Python](#3)<br>
[4. 重新理解Hello world ........................Re-understand Hello world](#4)<br>
[5. Python的编程范式 .......................... Programming convention of Python](#5)<br>
[6. 控制流 ............................................ Control flow](#6)<br>
[7. 数据结构 ......................................... Data structure](#7)<br>
[8. 模块化 ............................................. Moduling programming](#8)<br>

## 公告
1. 这篇文章如果有Typo可以微信我或者贴issue
2. 如果有任何还是不懂的地方 | 有更深入的问题可以微信我
3. 如果有任何建议（添加内容之类的），请联系我然后等我哪天心情好再说（逃

<h2 id="0">为什么写这篇文</h2>
新学期G2的CS老师换成老D之后开始赶syllabus剩下的大量内容，大部分人都遇到了不少的问题，最大
的应该是：原来靠死记硬背的方式考试已经行不通了。

鉴于湘哥本身也不会多少编程（人家是商科+cs联合学科的= =），本身大部分同学的编程能力就薄弱
，而老D讲东西又不清楚，所以才有写这篇文的打算。本来是打算在project的due之前写的，因为各种
原因拖到现在（逃

老D布置了Project之后，看到很多人在网上看教程补Python。不过网上的教程很多不一定适合，其中
我认为最好的，也就是[廖雪峰的教程](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)
我也很惊喜的发现有部分人找到了它。然而不得不说的是，即使是廖雪峰的教程，也有很多地方讲的
有不清楚，而且其中包含的一些部分很多同学现在最好不要看的东西（可能会陷进去，导致越学越乱
，或者是会因为理解不到位导致的很多问题），譬如 函数式编程 Functional Programming，这个是
不少程序员和国内大学生都没有理解的东西= =

另外也有朋友拿着其他我们同学写的教程问我说看不懂，我看了之后也发现里面有不少误导性的范式
和写法，这篇文也算是为了消除这些负面影响吧

<h2 id="1">谁应该看这篇文</h2>
毫不谦虚的说，这个年级只有我不用看（捂脸逃
本文中会有涉及到与Java和C#的联系，学习过的同学应该会比较容易接受Python的概念。
另外我会稍微提及一丢丢CPython的底层实现帮助理解。
斜体部分不懂的请不要纠结请强跳。

<h2 id="2">准备工作</h2>
写Python前有两个准备工作总是给大部分人忽略，这里我要重新提起来：
0. interpreter
1. a suitable editor

<h3 id="2.1">适合的解释器</h3>
Python本身的确是一门语言，也就是仅仅一纸规范而已，说明语法规则和明明规范。而解释器，也就
正如我们上课教的一样，才是真正运行Python程序的东西。
这里有必要提一下编译器和解释器的区别，这一点龙书Compilers Principles, Techniques, & Tools
已经讲的很清楚了：

Compiler:
>a compiler is a program that can read a program in one language
>— the source language — and translate it into an equivalent program in
>another language — the target language

Interpreter:
>An interpreter is another common kind of language processor. Instead of
>producing a target program as a translation, an interpreter appears to directly
>execute the operations specified in the source program on inputs supplied by
>the user

总结一下就是，编译器是个程序，把你的源码转成另一种语言（譬如二进制的machine language），
而你需要手动运行/用解释器解释运行目标文件才能运行程序。

然而解释器就是个黑盒子，给他源码，然后他就会运行它，并不需要另外双击运行目标文件之类的，
你直接和interpreter做IO就可以。（注：IO = Input & Output）

（前方高能预警

*而Python的解释器，最为常用的，[CPython](https://www.python.org/downloads/)，也就是用C语
言实现的Python解释器，就是我们从官网下载的解释器版本。这个版本最为常用，当然对标准的支持
最为完善，库的数量最多。不过它也有许多不足，其中之一就是性能;首当其冲的GIL对多线程效率的
影响一直为人诟病，然而大量的CPython库已经严重依赖于它，短时间没有去除的可能性。CPython先
将Python源码编译为字节码（.pyc文件），然后再由虚拟机解释执行（不要纠结什么是虚拟机！！！
）。另外一个很大的CPython的缺点是垃圾回收方式，他只支持引用计数（reference counting），也
就意味着大家熟知（才不）的循环引用问题也是它的一大漏洞。*


*除了CPython之外，在CLR(.NET)上实现的IronPython和在JVM上实现的JPython也是著名的Python解释
器实现。对于要在程序中使用C#/Java的相关代码时，这两个都是很好的选择。*

*还有一个PyPy是利用RPython和Tracing JIT技术实现的Python，它可以选择多种垃圾回收方式，譬如
标记清除mark & sweep（javascript使用的方式），标记压缩，分代之类的。性能对于CPython提升非
常明显，然而对第三方模块的支持并不好。*

*还有一个我最近了解到的Pyston，由Dropbox开发的解释器，目测挺牛逼，然而我并不了解就不说了
（捂脸逃*

**然而不要想太多，老老实实用CPython吧（正经**

<h3 id="2.2">CPython安装方式</h3>
Windows:

进入[官网](https://www.python.org/downloads/)下载就是了

Debian/Ubuntu/Linux Mint:

`(sudo) apt-get install python`

Mac OS:

方法一：同Windows<br>
方法二：如果安装了Homebrew：

`brew install python`

<h3 id="2.3">适合的编辑器</h3>
现在全年级有两种主流：
0. 用IDLE
1. 根据大部分教程所介绍的，用sublime text

**然而两种都不推荐**

为什么两种我都不推荐呢，因为：
0. IDLE真的很垃圾我就不解释了，马上你用了我推荐的你就知道了
1. sublime text就如它名字一样，非常好使，然而它的配置对于所有我们年级的人来说都是噩梦（逃
，但如果你不配置那就失去了用它的意义了（配置文件完全手写json）

**所以今天我要强行安利一个配置方法友好，使用的体验不亚于sublime text的编辑器** -- [Github Atom](https://atom.io)

<h3 id="2.4">Atom安装+设置</h3>

0. 进入上面的那个链接
1. 下载安装atom
2. 打开atom
3. 在上边的工具栏选择 Edit - Preference 进入设置页面，将设置调成如下的样子：

4. 随便新建一个Python文件就可以开始编码了

<h2 id="3">什么是Python</h2>
Python是Guido van Rossum开发的一门面向对象的解释型脚本语言。它具备垃圾回收的功能，也就是
自动管理内存 -- 释放不再需要的内存块。Python的特点是简洁易学，不过对于一个人的计算机思维
养成来说并不是最好的选择。Python支持命令式编程，面向对象编程，函数式编程和面向切面编程。
（请不要管这些）。

我们来理解一下Python的介绍，一门面向对象的解释型脚本语言。面向对象表示Python中的所有东西
都是对象，与Java和C#不同，Python中没有primitive data type（主数据类型）的概念，Python中的
所有东西，包括整数和浮点数，都是对象，可以直接调用它们的方法。典型的例子是validation中的
`input_string.isdigit()`。很多人应该都发现了，湘哥教的这个验证方法不适用于其他东西，譬如
整数，举例来说`123.isdigit()`是会报错的。

*这些机制在Java和C#中是通过autoboxing（自动装箱
）实现的，但是在Python中并没有这个概念，所有的东西都是对象。也就是说，任何一个Python里面
的东西实质都是内存块，而垃圾回收就是在它们不再被需要的时候处理掉他们的机制。如果没有垃圾
回收机制，程序就很可能会面对严重的内存泄露。*

<h2 id="4">重新理解Hello world</h2>
Hello world这个梗最先源于C语言之父所著的 《The C Programming Language》一书，因其中的第一
个程序是在控制台输出一句Hello world，而且之后的书大多延续该传统，才得以成梗。后来Hello
World也用来泛指所写的第一个程序（不一定是输出Hello world才算）。

然而看着篇文章的人理论上都应该会写Hello world程序了：

../examples/hello0.py:
```python
print("Hello, world!")
```
然而这个程序如果到此为止就失去了意义。本节的目的是一步一步改善该程序，添加应有的内容，但
并不会改变程序本身的功能。

首先我们先来讨论第一个可以改善的地方：
如果将"Hello, world!"改成"你好，世界"，使用Windows下的IDLE编程的同学一定都会发现保存程序
的时候会弹出一个对话框询问有关字符编码的事情。字符编码是计算机从二进制中解码到字符规范。
譬如一串机器码0x61，用ASCII字符编码解码得到的结果是字符'a'，utf-8也是如此。然而其他字符集
就无法得到相同的结果，这也是为什么有时候txt文件中的内容会变成乱码 -- 撰写者电脑的字符编码
和读者的字符编码并不一样，对于一台使用utf-8的电脑来说0x61是'a'，对于其他字符集可能就不一
样。p.s.: 对于字符集的了解请到此为止，不要试图自行搜更多的东西，再多就是到大学学习的了，
譬如wide character, multibyte, big endian和little endian :)

所以为了解决字符集问题，我们在文件的开头加上注释：

../examples/hello1.py:
```python
# -*- coding=utf8 -*-

print("Hello, world!")
```

这就说明我们的Python源码是以utf-8字符集存储，字符串中的非拉丁字母都会按照utf-8的字符编码
存储。

这时候我们该运行下程序：

Windows：

按下`Win + R`键，在弹出的窗口输入cmd，按下`Enter`进入Windows命令行终端。在终端中输入

`python 你的文件地址`

Mac OS X:

按下`f4`键，在某一个文件夹中找到 **终端** 或者 **terminal** 然后进入，接着输入

`python 你的文件地址`

Debian/Ubuntu/Linux Mint：

按下快捷键 `Ctrl + Alt + T` 打开终端，输入

`python 你的文件地址`

然后就可以在控制台中看到输出。对于Windows用户，如果强行双击运行的话会导致一闪而过，因为计
算机输出字符串的速度极快，来不及我们反应，我们就需要让系统在输出之后pause住，于是代码改为
：

~/examples/hello1_win.py

```python
# -*- coding=utf8 -*-

import os


print("Hello, world!")
os.system("pause")
```

**记住，以上代码只能运行于Windows平台，对于Mac和Linux distros直接控制台运行就够了**

接下来我们发现，即使用控制台运行可以避免一闪而过，然而每次运行程序要运行Python解释器，导致
我们的命令要打`python`几个字，强迫症怎么能忍（摔桌。所以我们要再通过文件注释告诉系统我们的
程序是用Python解释器运行的，不需要我们再写在命令中：

~/examples/hello2.py

```python
#!/usr/bin/python3
# -*- coding=utf-8 -*-

print("Hello, world!")
```

加上之后我们就可以通过以下命令运行我们的文件了：

`./你的文件地址`


