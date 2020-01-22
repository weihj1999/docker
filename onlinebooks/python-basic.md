# 数字化交付编程编排-基础课程（从零开始）

流程简介
1. 基础课程（从零开始）
- Python基础材料学习
- Python演练环境准备
- 7门小测试
2. Python编程学习
- Python基础回顾，TXT/CSV, 正则表达式，Excel， MOP/Word， API/爬虫
- 9个关卡
3. 实战
- 场景选题
- 公共场景实战


学习脉络
1. 第一部分
- 要求与痛点
- 概念和特点
- 安装环境
2. 第二部分
- 基础语法
- 数据类型
- 运算符
- 流程控制
- 数据结构
3. 第三部分
- 函数
- 错误与异常
- 类与对象
- 模块
4. 第四部分
- 文件读写
- 正则表达式
- XML/JSON
- 时间日期，标准库浏览

## 1. python的概念及特点
1. Python的发展历史：

Python 是由 Guido van Rossum 在八十年代末和九十年代初，在荷兰国家数学和计算机科学研究所设计出来的。Python 本身也是由诸多其他语言发展而来的,这包括 ABC、Modula-3、C、C++、Algol-68、SmallTalk、Unix shell和其他的脚本语言等等。

像 Perl 语言一样，Python 源代码同样遵循 GPL(GNU General Public License)协议。

现在 Python 是由一个核心开发团队在维护，Guido van Rossum 仍然占据着至关重要的作用，指导其进展。

2. 那到底什么是python？

- 1. Python是一种解释型、面向对象、动态数据类型的高级程序设计语言。

- 2. Python由Guido van Rossum于1989年底发明，第一个公开发行版发行于1991年。

- 3. 像Perl语言一样, Python 源代码同样遵循 GPL(GNU General Public License)协议。

3. Python的特点：

- 1. 易于学习：Python有相对较少的关键字，结构简单，和一个明确定义的语法，学习起来更加简单。
- 2. 易于阅读：Python代码定义的更清晰。
- 3. 易于维护：Python的成功在于它的源代码是相当容易维护的。
- 4. 一个广泛的标准库：Python的最大的优势之一是丰富的库，跨平台的，在UNIX，Windows和Macintosh兼容很好。
- 5. 互动模式：互动模式的支持，您可以从终端输入执行代码并获得结果的语言，互动的测试和调试代码片断。
- 6. 可移植：基于其开放源代码的特性，Python已经被移植（也就是使其工作）到许多平台。
- 7. 可扩展：如果你需要一段运行很快的关键代码，或者是想要编写一些不愿开放的算法，你可以使用C或C++完成那部分程序，然后从你的Python程序中调用。
- 8. 数据库：Python提供所有主要的商业数据库的接口。
- 9. GUI编程：Python支持GUI可以创建和移植到许多系统调用。
- 10. 可嵌入: 你可以将Python嵌入到C/C++程序，让你的程序的用户获得"脚本化"的能力。

4. Python的概念：

Python 是一个高层次的结合了解释性、编译性、互动性和面向对象的脚本语言。

Python 的设计具有很强的可读性，相比其他语言经常使用英文关键字，其他语言的一些标点符号，它具有比其他语言更有特色语法结构。

Python 是一种解释型语言： 这意味着开发过程中没有了编译这个环节。类似于PHP和Perl语言。

让我们总结一下：
- 1. Python 是交互式语言： 这意味着，您可以在一个 Python 提示符 >>> 后直接执行代码。
- 2. Python 是面向对象语言: 这意味着Python支持面向对象的风格或代码封装在对象的编程技术。
- 3. Python 是初学者的语言：Python 对初级程序员而言，是一种伟大的语言，它支持广泛的应用程序开发，从简单的文字处理到 WWW 浏览器再到游戏。

有的学员皱起了眉头， 班主任，你说了这么多python的好处，难道python一点问题都没有么？

当然有：人无完人，当然，代码也有自己的问题，下面班主任列举几点，你们自己看看是不是缺点还是可以接受的，没你想象的那么完美，所以，你也要为编程贡献一份自己的力量，加油哇。

python 当前的缺点：

- 1. 运行速度，用 C++ 改写关键部分会更快。
- 2. 国内市场较小（国内以 Python 来做主要开发的，目前只有一些 web2.0 公司）。但时间推移，目前很多国内软件公司，尤其是游戏公司，也开始规模使用他。
- 3. 中文资料匮乏（好的 Python 中文资料屈指可数，现在应该变多了）。托社区的福，有几本优秀的教材已经被翻译了，但入门级教材多，高级内容还是只能看英语版。
4. 构架选择太多（没有像 C# 这样的官方 .net 构架，也没有像 ruby 由于历史较短，构架开发的相对集中。Ruby on Rails 构架开发中小型web程序天下无敌）。不过这也从另一个侧面说明，python比较优秀，吸引的人才多，项目也多。

毕竟，知己知彼，百战不殆。讲了这么多，手是不是特别痒？空谈无意，我们上代码，你准备好了么？

## 2. Python IDE环境安装
略，网上的资料很多，自行研究确保用起来顺手，没有错误即可。

## 3. 基本语法

1. 编码

默认情况下，Python 3 源码文件以 UTF-8 编码，所有字符串都是 unicode 字符串。

（上来就看不懂了？没关系，你只需要记住，Unicode与UTF-8的关系，Unicode是内存编码的规范，UTF-8是如何保存于传输Unicode的手段。）

当然你也可以为源码文件指定不同的编码：
```
# -*- coding: cp-1252 -*-
```
上述定义允许在源文件中使用 Windows-1252 字符集中的字符编码，对应适合语言为保加利亚语、白罗斯语、马其顿语、俄语、塞尔维亚语。

在Python3当中，程序处理我们输入的字符串，是默认使用Unicode编码的，所以你什么语言都可以输入，不用担心你的程序出现在别人的电脑上是乱码。

2. 标识符

标识符：开发人员在程序中自定义的一些符号和名称。标示符是自己定义的,如变量名,函数名等。

标识符注意事项：
- 1）第一个字符必须是字母表中字母或下划线 _ 。
- 2）标识符的其他的部分由字母、数字和下划线组成。
- 3）标识符对大小写敏感。

3. PYTHON保留字
保留字即关键字，我们不能把它们用作任何标识符名称。Python 的标准库提供了一个 keyword 模块，可以输出当前版本的所有关键字：

```python
>>> import keyword
>>> keyword.kwlist
['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```

4. 注释
Python中单行注释以 # 开头

这里我请你们思考一个问题，为什么会有注释呢？头脑风暴一下。

其实这样的，我们自己的写的程序会有成千上万行，可能周一写的程序周五就不记得了，所以用注释写一下，可以提醒自己当初是怎么想的，清晰的看到自己的编程思路。又或者，你的同事被派去别的项目组了，他走之前留给了你无数的珍贵代码，你却看不到他们的意思，这里注释就派上了大用场。又或者当你写了一段程序，但是心中忐忑，总觉得哪行代码会报错的话，先把他注释掉，注释掉的代码就不直接运行了，这些都是注释的好处。

实例如下：

```python
#!/usr/bin/python3
# 第一个注释
print ("Hello, Python!") # 第二个注释
```
执行以上代码，输出结果为：（你把上面的代码复制到你的环境中执行一下吧！你的第一段代码，鼓掌，记得这里都要用英文的输入符哦，中文的话会报错的！）
```
Hello, Python!
```
多行注释可以用多个 # 号，还有 ''' 和 """：
```
#!/usr/bin/python3
# 第一个注释
# 第二个注释
''' 第三注释
第四注释
'''
"""
第五注释
第六注释
"""
print ("Hello, Python!")
```

执行以上代码，输出结果为：

```
Hello, Python!
```

5. 行与缩进

python最具特色的就是使用缩进来表示代码块，不需要使用大括号 {} 。

缩进的空格数是可变的，但是同一个代码块的语句必须包含相同的缩进空格数。实例如下：

```python
if True:
 print ("True")
else:
 print ("False")
```
以下代码最后一行语句缩进数的空格数不一致，会导致运行错误：
```python
if True:
    print ("Answer")
    print ("True")
else:
    print ("Answer")
  print ("False")    # 缩进不一致，会导致运行错误
```

以上程序由于缩进不一致，执行后会出现类似以下错误：

```
File "test.py", line 6
   print ("False")    # 缩进不一致，会导致运行错误
                                     ^
IndentationError: unindent does not match any outer indentation level
```
6. 多行语句

Python 通常是一行写完一条语句，但如果语句很长，我们可以使用反斜杠(\)来实现多行语句，例如：

```
total = item_one + \
        item_two + \
        item_three
```
在 [], {}, 或 () 中的多行语句，不需要使用反斜杠(\)，例如：
```
total = ['item_one', 'item_two', 'item_three',
        'item_four', 'item_five']
```

7. 数字类型（NUMBER）

python中数字有四种类型：整数、布尔型、浮点数和复数。

（数字类型到底怎么用，我们后面会详细讲到，这里你先了解一下他们的组成就好了，就当复习一下小学数学）

- int (整数), 如 1, 只有一种整数类型 int，表示为长整型，没有 python2 中的 Long。
- bool (布尔), 如 True。
- float (浮点数), 如 1.23、3E-2
- complex (复数), 如 1 + 2j、 1.1 + 2.2j

8. 字符串（STRING）

- python中单引号和双引号使用完全相同（注意，我这里说的都是英文的字符哦）。（还是老规矩，这里你简单了解就好，以后会详细讲到！只是对他们有一个基本的了解就可以。）
- 使用三引号('''或""")可以指定一个多行字符串。
- 转义符 '\'， 反斜杠可以用来转义，使用r可以让反斜杠不发生转义。。 如 r"this is a line with \n" 则\n会显示，并不是换行。
- 按字面意义级联字符串，如"this " "is " "string"会被自动转换为this is string。
- 字符串可以用 + 运算符连接在一起，用 * 运算符重复。
- Python 中的字符串有两种索引方式，从左往右以 0 开始，从右往左以 -1 开始。（从现在开始，你要记得，编程里所有的规则都是从0开始，不是从1开始的。）
- Python中的字符串不能改变。
- Python 没有单独的字符类型，一个字符就是长度为 1 的字符串。
- 字符串的截取的语法格式如下：变量[头下标:尾下标:步长]

```python
word = '字符串'
sentence = "这是一个句子。"
paragraph = """这是一个段落，
可以由多行组成"""
```

```python
#!/usr/bin/python3
str='Runoob'
print(str) # 输出字符串
print(str[0:-1]) # 输出第一个到倒数第二个的所有字符
print(str[0]) # 输出字符串第一个字符
print(str[2:5]) # 输出从第三个开始到第五个的字符
print(str[2:]) # 输出从第三个开始的后的所有字符
print(str * 2) # 输出字符串两次
print(str + '你好') # 连接字符串
print('------------------------------')
print('hello\nrunoob') # 使用反斜杠(\)+n转义特殊字符
print(r'hello\nrunoob') # 在字符串前面添加一个 r，表示原始字符串，不会发生转义
```

这里的 r 指 raw，即 raw string。

输出结果为：

```
Runoob
Runoo
R
noo
noob
RunoobRunoob
Runoob你好
------------------------------
hello
runoob
hello\nrunoob
```

9. 空行

函数之间或类的方法之间用空行分隔，表示一段新的代码的开始。类和函数入口之间也用一行空行分隔，以突出函数入口的开始。

空行与代码缩进不同，空行并不是Python语法的一部分。书写时不插入空行，Python解释器运行也不会出错。但是空行的作用在于分隔两段不同功能或含义的代码，便于日后代码的维护或重构。

这里我想要你们记住：空行也是程序代码的一部分。

10. 等待用户输入

执行下面的程序在按回车键后就会等待用户输入：
```python
#!/usr/bin/python3
input("\n\n按下 enter 键后退出。")
```

以上代码中 ，"\n\n"在结果输出前会输出两个新的空行。一旦用户按下 enter 键时，程序将退出。

11. 同一行显示多条语句

Python可以在同一行中使用多条语句，语句之间使用分号(;)分割，以下是一个简单的实例：
```python
#!/usr/bin/python3
import sys; x = 'runoob'; sys.stdout.write(x + '\n')
```
使用脚本执行以上代码，输出结果为：
```
runoob
```

使用交互式命令行执行，输出结果为：
```
>>> import sys; x = 'runoob'; sys.stdout.write(x + '\n')
runoob
7
```
此处的 7 表示字符数。

12. 多行语句构成代码组

缩进相同的一组语句构成一个代码块，我们称之代码组。

像if、while、def和class这样的复合语句，首行以关键字开始，以冒号( : )结束，该行之后的一行或多行代码构成代码组。

我们将首行及后面的代码组称为一个子句(clause)。

如下实例：
```python
if expression :
  suite
elif expression :
  suite
else :
  suite
```
13. PRINT输出

print 默认输出是换行的，如果要实现不换行需要在变量末尾加上 end=""：

```

#!/usr/bin/python3
x="a"
y="b"
# 换行输出
print( x )
print( y )
print('---------')
# 不换行输出
print( x, end=" " )
print( y, end=" " )
print()
```
以上实例执行结果为：
```
a
b
---------
a b
```

14. IMPORT与FROM..IMPORT

在 python 用 import 或者 from...import 来导入相应的模块。

将整个模块(somemodule)导入，格式为： import somemodule

从某个模块中导入某个函数,格式为： from somemodule import somefunction

从某个模块中导入多个函数,格式为： from somemodule import firstfunc, secondfunc, thirdfunc

将某个模块中的全部函数导入，格式为： from somemodule import *

导入 SYS 模块
```python

import sys
print('================Python import mode==========================');
print ('命令行参数为:')
for i in sys.argv:
  print (i)
  print ('\n python 路径为',sys.path)
```
导入 SYS 模块的 ARGV,PATH 成员
```python

from sys import argv,path # 导入特定的成员
print('================python from import===================================')
print('path:',path) # 因为已经导入path成员，所以此处引用时不需要加sys.path
```
15. 命令参数行

很多程序可以执行一些操作来查看一些基本信息，Python可以使用-h参数查看各参数帮助信息：
```

$ python -h
usage: python [option] ... [-c cmd | -m mod | file | -] [arg] ...
Options and arguments (and corresponding environment variables):
-c cmd : program passed in as string (terminates option list)
-d     : debug output from parser (also PYTHONDEBUG=x)
-E     : ignore environment variables (such as PYTHONPATH)
-h     : print this help message and exit

[ etc. ]
```

## 数据

计算机有三种方式可以和数据打交道
- 直接使用数据
- 用数据做判断
- 加工数据

1. 直接使用数据

比如print()语句，可以直接把我们提供的数据打印出来，通常所见即所得。还记得我们学过的print么？他是一个很重要的函数。

那现在在你的环境中敲一下代码吧，我们看看程序的结果：

请将此段代码复制粘贴并且运行一下：
```python

print(3)

print('欢迎来到第4关')
```

输出的结果应该是这样的：     
```

3

欢迎来到第4关
```

2. 计算和加工数据

让我们看个例子：

请将此段代码复制粘贴并且运行一下：
```python3

print(3+2*3)

print('欢迎来到'+'Python世界')
```

输出的结果应该是这样的：
```

9

欢迎来到Python世界
```

这两个print语句，计算机都是先把数据加工了一下，再把print()括号里的数据打印到屏幕里的，在这里计算机对数据进行了加工和处理，用自己的逻辑把你输入的命令进行了加工。

3. 对数据做判断

不用着急，你直接把代码复制过去运行就好，看看其中的逻辑，有的时候看别人的代码也是学习的一种有效方式，多看看，你就会了。
```python

a = int(input('请输入你的学习时间：'))
if a<0:
  print('你还没开始学习。')
elif a == 0:
  print('欢迎来到Python世界。')
elif a < 18:
  print('你学习的还不够')
else:
  print('你已经是个Python高手了。')
```

在这里，计算机就在用自己的逻辑做一个数据的判断，其中if，elif, else就是条件判断的语句。你可想而知，数据和计算机是一种水乳交融的关系，所以掌握数据类型对与我们来说至关重要。这一关呢？我们就来学习一下几个重要的数据类型。

## 数据类型的基本概念

先总结一下标准数据类型：
- Number 数字
- String 字符串
- List 列表
- Tuple 元组
- Set 集合
- Dictionary 字典

1. Python3基本数据类型

Python 中的变量不需要声明。每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建。

在 Python 中，变量就是变量，它没有类型，我们所说的"类型"是变量所指的内存中对象的类型。

等号（=）用来给变量赋值。

等号（=）运算符左边是一个变量名,等号（=）运算符右边是存储在变量中的值。（请答应我，你不要把赋值和等于弄混了）

```python3
#!/usr/bin/python3
counter = 100 # 整型变量
miles = 1000.0 # 浮点型变量
name = "runoob" # 字符串
print (counter)
print (miles)
print (name)
```

执行以上程序会输出如下结果：
```
100
1000.0
runoob
```
2. 多个变量赋值

Python允许你同时为多个变量赋值。例如：
```
a = b = c = 1
```
以上实例，创建一个整型对象，值为 1，从后向前赋值，三个变量被赋予相同的数值。

您也可以为多个对象指定多个变量。例如：
```
a, b, c = 1, 2, "runoob"
```
以上实例，两个整型对象 1 和 2 的分配给变量 a 和 b，字符串对象 "runoob" 分配给变量 c。

3. 标准数据类型
Python3 中有六个标准的数据类型：

- Number（数字）
- String（字符串）
- List（列表）
- Tuple（元组）
- Set（集合）
- Dictionary（字典）

Python3 的六个标准数据类型中：

- 不可变数据（3 个）：Number（数字）、String（字符串）、Tuple（元组）；
- 可变数据（3 个）：List（列表）、Dictionary（字典）、Set（集合）。

4. （NUMBER数字）

Python3 支持 int、float、bool、complex（复数）。

在Python 3里，只有一种整数类型 int，表示为长整型，没有 python2 中的 Long。

像大多数语言一样，数值类型的赋值和计算都是很直观的。

内置的 type() 函数可以用来查询变量所指的对象类型。
```
>>> a, b, c, d = 20, 5.5, True, 4+3j
>>> print(type(a), type(b), type(c), type(d))
<class 'int'> <class 'float'> <class 'bool'> <class 'complex'>
```
此外还可以用 isinstance 来判断：

实例
```
>>>a = 111
>>> isinstance(a, int)
True
>>>
```
isinstance 和 type 的区别在于：

- type()不会认为子类是一种父类类型。
- isinstance()会认为子类是一种父类类型。
```
>>> class A:
...     pass
...
>>> class B(A):
...     pass
...
>>> isinstance(A(), A)
True
>>> type(A()) == A
True
>>> isinstance(B(), A)
True
>>> type(B()) == A
False
```

>注意：<br>
在 Python2 中是没有布尔型的，它用数字 0 表示 False，用 1 表示 True。到 Python3 中，把 True 和 False 定义成关键字了，但它们的值还是 1 和 0，它们可以和数字相加。

当你指定一个值时，Number 对象就会被创建：
```
var1 = 1
var2 = 10
```
您也可以使用del语句删除一些对象引用。

del语句的语法是：
```
del var1[,var2[,var3[....,varN]]]
```
您可以通过使用del语句删除单个或多个对象。例如：
```
del var
del var_a, var_b
```

数值运算

实例
```
>>>5 + 4 # 加法
9
>>> 4.3 - 2 # 减法
2.3
>>> 3 * 7 # 乘法
21
>>> 2 / 4 # 除法，得到一个浮点数
0.5
>>> 2 // 4 # 除法，得到一个整数
0
>>> 17 % 3 # 取余  
2
>>> 2 ** 5 # 乘方
32
```
注意：

- 1、Python可以同时为多个变量赋值，如a, b = 1, 2。
- 2、一个变量可以通过赋值指向不同类型的对象。
- 3、数值的除法包含两个运算符：/ 返回一个浮点数，// 返回一个整数。
- 4、在混合计算时，Python会把整型转换成为浮点数。

数值类型实例

| int | float | complex |
| --- | --- | --- |
| 10 | 	0.0 | 3.14j |
| 100	| 15.20	| 45.j |
| -786 | -21.9	| 9.322e-36j |
| 080	| 32.3e+18 | 	.876j |
| -0490	| -90.	| -.6545+0J |
| -0x260 | 	-32.54e100 | 	3e+26J |
| 0x69	| 70.2E-12 | 	4.53e-7j |

Python还支持复数，复数由实数部分和虚数部分构成，可以用a + bj,或者complex(a,b)表示， 复数的实部a和虚部b都是浮点型

5. （STRING字符串）

Python中的字符串用单引号 ' 或双引号 " 括起来，同时使用反斜杠 \ 转义特殊字符。

字符串的截取的语法格式如下：
```
变量[头下标:尾下标]
```
索引值以 0 为开始值，-1 为从末尾的开始位置。

|  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- |
| 从后面索引：| -6 | -5 | -4 | -3 | -2 | -1 |
| 从前面索引：| 0 | 1 | 2| 3 | 4 | 5 |
|   | a | b | c | d | e | f |

加号 + 是字符串的连接符， 星号 * 表示复制当前字符串，紧跟的数字为复制的次数。实例如下：

实例
```python
#!/usr/bin/python3
str = 'Runoob'
print (str) # 输出字符串
print (str[0:-1]) # 输出第一个到倒数第二个的所有字符
print (str[0]) # 输出字符串第一个字符
print (str[2:5]) # 输出从第三个开始到第五个的字符
print (str[2:]) # 输出从第三个开始的后的所有字符
print (str * 2) # 输出字符串两次
print (str + "TEST") # 连接字符串
```
执行以上程序会输出如下结果：（还记得我说的话么？编码是从0开始）
```
Runoob
Runoo
R
noo
noob
RunoobRunoob
RunoobTEST
```
Python 使用反斜杠(\)转义特殊字符，如果你不想让反斜杠发生转义，可以在字符串前面添加一个 r，表示原始字符串：

```
>>> print('Ru\noob')
Ru
oob
>>> print(r'Ru\noob')
Ru\noob
>>>
```

另外，反斜杠(\)可以作为续行符，表示下一行是上一行的延续。也可以使用 """...""" 或者 '''...''' 跨越多行。

注意，Python 没有单独的字符类型，一个字符就是长度为1的字符串。

实例
```
>>>word = 'Python'
>>> print(word[0], word[5])
P n
>>> print(word[-1], word[-6])
n P
```

与 C 字符串不同的是，Python 字符串不能被改变。向一个索引位置赋值，比如word[0] = 'm'会导致错误。

注意：

- 1、反斜杠可以用来转义，使用r可以让反斜杠不发生转义。
- 2、字符串可以用+运算符连接在一起，用*运算符重复。
- 3、Python中的字符串有两种索引方式，从左往右以0开始，从右往左以-1开始。
- 4、Python中的字符串不能改变。

6. （LIST列表）

List（列表） 是 Python 中使用最频繁的数据类型。所以请你们认真学习，这个将是陪伴大家最常见的数据类型。

列表可以完成大多数集合类的数据结构实现。列表中元素的类型可以不相同，它支持数字，字符串甚至可以包含列表（所谓嵌套）。

列表是写在方括号 [] 之间、用逗号分隔开的元素列表。

和字符串一样，列表同样可以被索引和截取，列表被截取后返回一个包含所需元素的新列表。

列表截取的语法格式如下：
```
变量[头下标:尾下标]
```
索引值以 0 为开始值，-1 为从末尾的开始位置。

互联网上找一个python列表截取的操作图

加号 + 是列表连接运算符，星号 * 是重复操作。如下实例：

实例
```python
#!/usr/bin/python3
list = [ 'abcd', 786 , 2.23, 'runoob', 70.2 ]
tinylist = [123, 'runoob'] print (list) # 输出完整列表
print (list[0]) # 输出列表第一个元素
print (list[1:3]) # 从第二个开始输出到第三个元素
print (list[2:]) # 输出从第三个元素开始的所有元素
print (tinylist * 2) # 输出两次列表
print (list + tinylist) # 连接列表
```
以上实例输出结果：
```
['abcd', 786, 2.23, 'runoob', 70.2]
abcd
[786, 2.23]
[2.23, 'runoob', 70.2]
[123, 'runoob', 123, 'runoob']
['abcd', 786, 2.23, 'runoob', 70.2, 123, 'runoob']
```
与Python字符串不一样的是，列表中的元素是可以改变的：

实例
```
>>>a = [1, 2, 3, 4, 5, 6]
>>> a[0] = 9
>>> a[2:5] = [13, 14, 15]
>>> a
[9, 2, 13, 14, 15, 6]
>>> a[2:5] = [] # 将对应的元素值设置为 []  
>>> a
[9, 2, 6]
```

List 内置了有很多方法，例如 append()、pop() 等等，这在后面会讲到。

注意：

- 1、List写在方括号之间，元素用逗号隔开。
- 2、和字符串一样，list可以被索引和切片。
- 3、List可以使用+操作符进行拼接。
- 4、List中的元素是可以改变的。

Python 列表截取可以接收第三个参数，参数作用是截取的步长，以下实例在索引 1 到索引 4 的位置并设置为步长为 2（间隔一个位置）来截取字符串：

网上找一个list的操作示意图

7. （TUPLE元组）

元组（tuple）与列表类似，不同之处在于元组的元素不能修改。元组写在小括号 () 里，元素之间用逗号隔开。

元组中的元素类型也可以不相同：

实例
```python
#!/usr/bin/python3
tuple = ( 'abcd', 786 , 2.23, 'runoob', 70.2 )
tinytuple = (123, 'runoob')
print (tuple) # 输出完整元组
print (tuple[0]) # 输出元组的第一个元素
print (tuple[1:3]) # 输出从第二个元素开始到第三个元素
print (tuple[2:]) # 输出从第三个元素开始的所有元素
print (tinytuple * 2) # 输出两次元组
print (tuple + tinytuple) # 连接元组
```
以上实例输出结果：
```
('abcd', 786, 2.23, 'runoob', 70.2)
abcd
(786, 2.23)
(2.23, 'runoob', 70.2)
(123, 'runoob', 123, 'runoob')
('abcd', 786, 2.23, 'runoob', 70.2, 123, 'runoob')
```
元组与字符串类似，可以被索引且下标索引从0开始，-1 为从末尾开始的位置。也可以进行截取（看上面，这里不再赘述）。

其实，可以把字符串看作一种特殊的元组。

实例
```python
>>>tup = (1, 2, 3, 4, 5, 6)
>>> print(tup[0])
1
>>> print(tup[1:5])
(2, 3, 4, 5)
>>> tup[0] = 11 # 修改元组元素的操作是非法的
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>>
```
虽然tuple的元素不可改变，但它可以包含可变的对象，比如list列表。

构造包含 0 个或 1 个元素的元组比较特殊，所以有一些额外的语法规则：
```
tup1 = ()    # 空元组
tup2 = (20,) # 一个元素，需要在元素后添加逗号
```
string、list 和 tuple 都属于 sequence（序列）。

学完元组，我想让你了解一下“切片”的含义，你去网上搜搜看，相信我，集训的时候你会听到这个名词的！

注意：

- 1、与字符串一样，元组的元素不能修改。
- 2、元组也可以被索引和切片，方法一样。
- 3、注意构造包含 0 或 1 个元素的元组的特殊语法规则。
- 4、元组也可以使用+操作符进行拼接。

8. （SET集合）

集合（set）是由一个或数个形态各异的大小整体组成的，构成集合的事物或对象称作元素或是成员。

基本功能是进行成员关系测试和删除重复元素。

可以使用大括号 { } 或者 set() 函数创建集合，注意：创建一个空集合必须用 set() 而不是 { }，因为 { } 是用来创建一个空字典。

创建格式：
```
parame = {value01,value02,...}
或者
set(value)
```
实例
```python
#!/usr/bin/python3
student = {'Tom', 'Jim', 'Mary', 'Tom', 'Jack', 'Rose'}
print(student) # 输出集合，重复的元素被自动去掉
# 成员测试
if 'Rose' in student :
print('Rose 在集合中')
else :
print('Rose 不在集合中')
# set可以进行集合运算
a = set('abracadabra')
b = set('alacazam')
print(a)
print(a - b) # a 和 b 的差集
print(a | b) # a 和 b 的并集
print(a & b) # a 和 b 的交集
print(a ^ b) # a 和 b 中不同时存在的元素
```
以上实例输出结果：
```
{'Mary', 'Jim', 'Rose', 'Jack', 'Tom'}
Rose 在集合中
{'b', 'a', 'c', 'r', 'd'}
{'b', 'd', 'r'}
{'l', 'r', 'a', 'c', 'z', 'm', 'b', 'd'}
{'a', 'c'}
{'l', 'r', 'z', 'm', 'b', 'd'}
```
9. （DICTIONARY字典）

字典（dictionary）是Python中另一个非常有用的内置数据类型。

列表是有序的对象集合，字典是无序的对象集合。两者之间的区别在于：字典当中的元素是通过键来存取的，而不是通过偏移存取。

字典是一种映射类型，字典用 { } 标识，它是一个无序的 键(key) : 值(value) 的集合。

键(key)必须使用不可变类型。

在同一个字典中，键(key)必须是唯一的。

实例
```python
#!/usr/bin/python3
dict = {}
  dict['one'] = "1 - 菜鸟教程"
  dict[2] = "2 - 菜鸟工具"
  tinydict = {'name': 'runoob','code':1, 'site': 'www.runoob.com'}
  print (dict['one']) # 输出键为 'one' 的值
  print (dict[2]) # 输出键为 2 的值
  print (tinydict) # 输出完整的字典
  print (tinydict.keys()) # 输出所有键
  print (tinydict.values()) # 输出所有值
```
以上实例输出结果：
```
1 - 菜鸟教程
2 - 菜鸟工具
{'name': 'runoob', 'code': 1, 'site': 'www.runoob.com'}
dict_keys(['name', 'code', 'site'])
dict_values(['runoob', 1, 'www.runoob.com'])
```
构造函数 dict() 可以直接从键值对序列中构建字典如下：

实例
```python
>>>dict([('Runoob', 1), ('Google', 2), ('Taobao', 3)])
{'Taobao': 3, 'Runoob': 1, 'Google': 2}
>>> {x: x**2 for x in (2, 4, 6)}
{2: 4, 4: 16, 6: 36}
>>> dict(Runoob=1, Google=2, Taobao=3)
{'Runoob': 1, 'Google': 2, 'Taobao': 3}
```
另外，字典类型也有一些内置的函数，例如clear()、keys()、values()等。

注意：

- 1、字典是一种映射类型，它的元素是键值对。
- 2、字典的关键字必须为不可变类型，且不能重复。
- 3、创建空字典使用 { }。

10. Python数据类型转换

有时候，我们需要对数据内置的类型进行转换，数据类型的转换，你只需要将数据类型作为函数名即可。

以下几个内置的函数可以执行数据类型之间的转换。这些函数返回一个新的对象，表示转换的值。

| 函数 | 	描述 |
| --- | --- |
| int(x [,base]) | 将x转换为一个整数 |
| float(x) | 将x转换到一个浮点数 |
| complex(real [,imag]) |创建一个复数 |
| str(x) | 将对象 x 转换为字符串 |
| repr(x)| 将对象 x 转换为表达式字符串 |
| eval(str) | 用来计算在字符串中的有效Python表达式,并返回一个对象 |
| tuple(s) | 将序列 s 转换为一个元组 |
| list(s) | 将序列 s 转换为一个列表 |
| set(s) | 转换为可变集合 |
| dict(d) | 创建一个字典。d 必须是一个序列 (key,value)元组。|
| frozenset(s) | 转换为不可变集合 |
| chr(x) | 将一个整数转换为一个字符 |
| ord(x) | 将一个字符转换为它的整数值 |
| hex(x) | 将一个整数转换为一个十六进制字符串 |
| oct(x) | 将一个整数转换为一个八进制字符串 |




## Python3数字

Python 数字数据类型用于存储数值。

数据类型是不允许改变的,这就意味着如果改变数字数据类型的值，将重新分配内存空间。

以下实例在变量赋值时 Number 对象将被创建：
```
var1 = 1
var2 = 10
```
您也可以使用del语句删除一些数字对象的引用（delete是全拼，掌握少量英语单词也可以助力你学习Python）。

del语句的语法是：
```
del var1[,var2[,var3[....,varN]]]]
```
您可以通过使用del语句删除单个或多个对象的引用，例如：
```
del var
del var_a, var_b
```
Python 支持三种不同的数值类型：
- 整型(Int) - 通常被称为是整型或整数，是正或负整数，不带小数点。Python3 整型是没有限制大小的，可以当作 Long 类型使用，所以 Python3 没有 Python2 的 Long 类型。
- 浮点型(float) - 浮点型由整数部分与小数部分组成，浮点型也可以使用科学计数法表示（2.5e2 = 2.5 x 102 = 250）
- 复数( (complex)) - 复数由实数部分和虚数部分构成，可以用a + bj,或者complex(a,b)表示， 复数的实部a和虚部b都是浮点型。

我们可以使用十六进制和八进制来代表整数：
```
>>> number = 0xA0F # 十六进制
>>> number
2575

>>> number=0o37 # 八进制
>>> number
31
```

| int	| float	| complex |
| --- | --- | --- |
| 10	| 0.0	| 3.14j |
| 100	| 15.20	| 45.j |
| -786 | 	-21.9	| 9.322e-36j |
| 080	| 32.3+e18	| .876j |
| -0490	| -90.	| -.6545+0J | 
| -0x260	| -32.54e100 | 	3e+26J | 
| 0x69	| 70.2-E12	| 4.53e-7j | 

- Python支持复数，复数由实数部分和虚数部分构成，可以用a + bj,或者complex(a,b)表示， 复数的实部a和虚部b都是浮点型。

1. PYTHON数字类型转化

有时候，我们需要对数据内置的类型进行转换，数据类型的转换，你只需要将数据类型作为函数名即可。

- int(x) 将x转换为一个整数。
- float(x) 将x转换到一个浮点数。
- complex(x) 将x转换到一个复数，实数部分为 x，虚数部分为 0。
- complex(x, y) 将 x 和 y 转换到一个复数，实数部分为 x，虚数部分为 y。x 和 y 是数字表达式。

以下实例将浮点数变量 a 转换为整数：
```
>>> a = 1.0
>>> int(a)
1
```

2. 编码PYTHON数字运算

Python 解释器可以作为一个简单的计算器，您可以在解释器里输入一个表达式，它将输出表达式的值。

表达式的语法很直白： +, -, * 和 /, 和其它语言（如Pascal或C）里一样。例如：

（请你在你的环境中也打下数学题吧）
```
>>> 2 + 2
4
>>> 50 - 5*6
20
>>> (50 - 5*6) / 4
5.0
>>> 8 / 5  # 总是返回一个浮点数
1.6
```
注意：在不同的机器上浮点运算的结果可能会不一样。

在整数除法中，除法 / 总是返回一个浮点数，如果只想得到整数的结果，丢弃可能的分数部分，可以使用运算符 // ：
```
>>> 17 / 3  # 整数除法返回浮点型
5.666666666666667
>>>
>>> 17 // 3  # 整数除法返回向下取整后的结果
5
>>> 17 % 3  # ％操作符返回除法的余数
2
>>> 5 * 3 + 2
17
```

注意：// 得到的并不一定是整数类型的数，它与分母分子的数据类型有关系。
```
>>> 7//2
3
>>> 7.0//2
3.0
>>> 7//2.0
3.0
>>>
```

等号 = 用于给变量赋值。赋值之后，除了下一个提示符，解释器不会显示任何结果。
```
>>> width = 20
>>> height = 5*9
>>> width * height
900
```

Python 可以使用 ** 操作来进行幂运算：
```
>>> 5 ** 2  # 5 的平方
25
>>> 2 ** 7  # 2的7次方
128
```

变量在使用前必须先"定义"（即赋予变量一个值），否则会出现错误：
```
>>> n   # 尝试访问一个未定义的变量
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
NameError: name 'n' is not defined
```

不同类型的数混合运算时会将整数转换为浮点数：
```
>>> 3 * 3.75 / 1.5
7.5
>>> 7.0 / 2
3.5
```

在交互模式中，最后被输出的表达式结果被赋值给变量 _ 。例如：
```
>>> tax = 12.5 / 100
>>> price = 100.50
>>> price * tax
12.5625
>>> price + _
113.0625
>>> round(_, 2)
113.06
```
此处， _ 变量应被用户视为只读变量。

3. 数字函数

| 函数	| 返回值 ( 描述 ) | 
| --- | --- | 
| abs(x)	| 返回数字的绝对值，如abs(-10) 返回 10| 
| ceil(x)	| 返回数字的上入整数，如math.ceil(4.1) 返回 5| 
| cmp(x, y)| 如果 x < y 返回 -1, 如果 x == y 返回 0, 如果 x > y 返回 1。 Python 3 已废弃 。使用 使用 (x>y)-(x<y) 替换。| 
| exp(x)	| 返回e的x次幂(ex),如math.exp(1) 返回2.718281828459045| 
| fabs(x)	| 返回数字的绝对值，如math.fabs(-10) 返回10.0| 
| floor(x)	| 返回数字的下舍整数，如math.floor(4.9)返回 4 | 
| log(x)	| 如math.log(math.e)返回1.0,math.log(100,10)返回2.0 | 
| log10(x)	| 返回以10为基数的x的对数，如math.log10(100)返回 2.0 | 
| max(x1, x2,...)	| 返回给定参数的最大值，参数可以为序列。| 
| min(x1, x2,...)	| 返回给定参数的最小值，参数可以为序列。| 
| modf(x)	| 返回x的整数部分与小数部分，两部分的数值符号与x相同，整数部分以浮点型表示。| 
| pow(x, y)	| x**y  运算后的值。| 
| round(x [,n])	| 返回浮点数x的四舍五入值，如给出n值，则代表舍入到小数点后的位数。| 
| sqrt(x)	| 返回数字x的平方根。| 

4. 随机数函数

随机数可以用于数学，游戏，安全等领域中，还经常被嵌入到算法中，用以提高算法效率，并提高程序的安全性。

Python包含以下常用随机数函数：

| 函数	| 描述 | 
| --- | --- | 
| choice(seq)	| 从序列的元素中随机挑选一个元素，比如random.choice(range(10))，从0到9中随机挑选一个整数。| 
| randrange ([start,] stop [,step])	| 从指定范围内，按指定基数递增的集合中获取一个随机数，基数缺省值为1 | 
| random()	| 随机生成下一个实数，它在[0,1)范围内。| 
| seed([x])	| 改变随机数生成器的种子seed。如果你不了解其原理，你不必特别去设定seed，Python会帮你选择seed。| 
| shuffle(lst)	| 将序列的所有元素随机排序 | 
| uniform(x, y)	| 随机生成下一个实数，它在[x,y]范围内。| 

5. 三角函数

Python包括以下三角函数：

| 函数	| 描述 | 
| --- | --- | 
| acos(x)	| 返回x的反余弦弧度值。| 
| asin(x)	| 返回x的反正弦弧度值。| 
| atan(x)	| 返回x的反正切弧度值。| 
| atan2(y, x)	| 返回给定的 X 及 Y 坐标值的反正切值。
| cos(x)	| 返回x的弧度的余弦值。| 
| hypot(x, y)	| 返回欧几里德范数 sqrt(x*x + y*y)。| 
| sin(x)	| 返回的x弧度的正弦值。| 
| tan(x)	| 返回x弧度的正切值。| 
| degrees(x)	| 将弧度转换为角度,如degrees(math.pi/2) ， 返回90.0| 
| radians(x)	| 将角度转换为弧度 | 

6. 数学常量

| 常量	| 描述 | 
| --- | --- | 
| pi	| 数学常量 pi（圆周率，一般以π来表示）| 
| e	| 数学常量 e，e即自然常数（自然常数）。| 

上述具体的语句的具体用法，你了解就行，在你需要编码的时候，记得上网搜索一下具体的知识点的应用，记住，编程不是为了把每个知识点都背下来，只需要你在用的时候，会查找，会应用就可以了。这才是学习语言的重要方法。

我给你个例子，比如我今天要用到max(x1, x2,...) ， 给你一个网站，你自己去学习一下。加油！
http://www.runoob.com/python3/python3-func-number-max.html


# Python3字符串

字符串是 Python 中最常用的数据类型。我们可以使用引号( ' 或 " )来创建字符串。

创建字符串很简单，只要为变量分配一个值即可。例如：
```
var1 = 'Hello World!'
var2 = "Runoob"
```

1. Python访问字符串中的值

Python 不支持单字符类型，单字符在 Python 中也是作为一个字符串使用。

Python 访问子字符串，可以使用方括号来截取字符串，如下实例：

```
#!/usr/bin/python3
var1 = 'Hello World!'
var2 = "Runoob"
print ("var1[0]: ", var1[0])
print ("var2[1:5]: ", var2[1:5])
```
以上实例执行结果：
```
var1[0]:  H
var2[1:5]:  unoo
```

2. Python字符串更新

你可以截取字符串的一部分并与其他字段拼接，如下实例：

```
#!/usr/bin/python3
var1 = 'Hello World!'
print ("已更新字符串 : ", var1[:6] + 'Runoob!')
```
以上实例执行结果
```
已更新字符串 :  Hello Runoob!
```

3. Python转义字符

在需要在字符中使用特殊字符时，python用反斜杠(\)转义字符。如下表：

| 转义字符	| 描述 | 
| --- | --- | 
| \\(在行尾时)	| 续行符 | 
| \\\\	| 反斜杠符号 | 
| \\\'	| 单引号 | 
| \\\"	| 双引号 | 
| \a	| 响铃 | 
| \b	| 退格(Backspace) | 
| \e	| 转义| 
| \000	| 空| 
| \n	| 换行| 
| \v	| 纵向制表符| 
| \t	| 横向制表符| 
| \r	| 回车| 
| \f	| 换页| 
| \oyy	| 八进制数，yy代表的字符，例如：\o12代表换行| 
| \xyy	| 十六进制数，yy代表的字符，例如：\x0a代表换行| 
| \other	| 其它的字符以普通格式输出| 

4. Python字符串运算符

下表实例变量a值为字符串 "Hello"，b变量值为 "Python"：

| 操作符	| 描述	| 实例 | 
| --- | --- | --- | 
| +	| 字符串连接	| a + b 输出结果： HelloPython | 
| *	| 重复输出字符串| 	a*2 输出结果：HelloHello | 
| []	| 通过索引获取字符串中字符	| a[1] 输出结果 e | 
| [ : ]	| 截取字符串中的一部分，遵循左闭右开原则，str[0,2] 是不包含第 3 个字符的。| 	a[1:4] 输出结果 ell | 
| in	| 成员运算符 - 如果字符串中包含给定的字符返回 True | 	'H' in a 输出结果 True | 
| not in	|  成员运算符 - 如果字符串中不包含给定的字符返回 True | 	'M' not in a 输出结果 True | 
| r/R	| 原始字符串 - 原始字符串：所有的字符串都是直接按照字面的意思来使用，没有转义特殊或不能打印的字符。 原始字符串除在字符串的第一个引号前加上字母 r（可以大小写）以外，与普通字符串有着几乎完全相同的语法。| print( r'\n' )<br>print( R'\n' ) | 
| %	| 格式字符串	| 请看下一节内容。| 

```
#!/usr/bin/python3
a = "Hello"
b = "Python"
print("a + b 输出结果：", a + b)
print("a * 2 输出结果：", a * 2)
print("a[1] 输出结果：", a[1])
print("a[1:4] 输出结果：", a[1:4])
if( "H" in a) :
print("H 在变量 a 中")
else :
print("H 不在变量 a 中")
if( "M" not in a) :
print("M 不在变量 a 中")
else :
print("M 在变量 a 中")
print (r'\n')
print (R'\n')
```
以上实例输出结果为：
```
a + b 输出结果： HelloPython
a * 2 输出结果： HelloHello
a[1] 输出结果： e
a[1:4] 输出结果： ell
H 在变量 a 中
M 不在变量 a 中
\n
\n
```

5. Python字符串格式化

Python 支持格式化字符串的输出 。尽管这样可能会用到非常复杂的表达式，但最基本的用法是将一个值插入到一个有字符串格式符 %s 的字符串中。

在 Python 中，字符串格式化使用与 C 中 sprintf 函数一样的语法。

```python
#!/usr/bin/python3
print ("我叫 %s 今年 %d 岁!" % ('小明', 10))
```
以上实例输出结果：
```
我叫 小明 今年 10 岁!
```

python字符串格式化符号:

| 符   号	| 描述 | 
| --- | --- | 
|     %c	 |  格式化字符及其ASCII码| 
|     %s	 | 格式化字符串 | 
|     %d	 | 格式化整数| 
|     %u	 | 格式化无符号整型| 
|     %o	 | 格式化无符号八进制数
|     %x	 | 格式化无符号十六进制数| 
|     %X	 | 格式化无符号十六进制数（大写）| 
|     %f	 | 格式化浮点数字，可指定小数点后的精度| 
|     %e	 | 用科学计数法格式化浮点数 | 
|     %E	 | 作用同%e，用科学计数法格式化浮点数 | 
|     %g	 | %f和%e的简写 | 
|     %G	 | %f 和 %E 的简写 | 
|     %p	 | 用十六进制数格式化变量的地址| 

格式化操作符辅助指令:

| 符号 | 	功能 | 
| --- | --- | 
| *	| 定义宽度或者小数点精度 | 
| -	| 用做左对齐 | 
| +	| 在正数前面显示加号( + )| 
| <sp>	| 在正数前面显示空格 | 
| #	| 在八进制数前面显示零('0')，在十六进制前面显示'0x'或者'0X'(取决于用的是'x'还是'X') | 
| 0	| 显示的数字前面填充'0'而不是默认的空格| 
| %	| '%%'输出一个单一的'%' | 
| (var)	| 映射变量(字典参数) | 
| m.n.	| m 是显示的最小总宽度,n 是小数点后的位数(如果可用的话) | 

Python2.6 开始，新增了一种格式化字符串的函数 str.format()，它增强了字符串格式化的功能。


6. Python三引号

python三引号允许一个字符串跨多行，字符串中可以包含换行符、制表符以及其他特殊字符。实例如下

```
#!/usr/bin/python3
para_str = """这是一个多行字符串的实例
多行字符串可以使用制表符
TAB ( \t )。
也可以使用换行符 [ \n ]。 """
print (para_str)
```
以上实例执行结果为：
```
这是一个多行字符串的实例
多行字符串可以使用制表符
TAB (    )。
也可以使用换行符 [
]。
```

三引号让程序员从引号和特殊字符串的泥潭里面解脱出来，自始至终保持一小块字符串的格式是所谓的WYSIWYG（所见即所得）格式的。

一个典型的用例是，当你需要一块HTML或者SQL时，这时用字符串组合，特殊字符串转义将会非常的繁琐。
```
errHTML = '''
<HTML><HEAD><TITLE>
Friends CGI Demo</TITLE></HEAD>
<BODY><H3>ERROR</H3>
<B>%s</B><P>
<FORM><INPUT TYPE=button VALUE=Back
ONCLICK="window.history.back()"></FORM>
</BODY></HTML>
'''
cursor.execute('''
CREATE TABLE users (
  login VARCHAR(8),
  uid INTEGER,
  prid INTEGER)
  ''')
```

8. UNICODE字符串

在Python2中，普通字符串是以8位ASCII码进行存储的，而Unicode字符串则存储为16位unicode字符串，这样能够表示更多的字符集。使用的语法是在字符串前面加上前缀 u。

在Python3中，所有的字符串都是Unicode字符串。

9. Python的字符串内建函数

Python 的字符串常用内建函数如下：

| 序号	| 方法及描述 | 
| --- | --- | 
| 1 | capitalize() 将字符串的第一个字符转换为大写<br>请你在你的环境中复制一下，看看结果是什么：<br>#!/usr/bin/python3<br>str = "this is string example from runoob....wow!!!"<br>print ("str.capitalize() : ", str.capitalize()) | 
| 2 | center(width, fillchar)<br><br>返回一个指定的宽度 width 居中的字符串，fillchar 为填充的字符，默认为空格。| 
| 3 | count(str, beg= 0,end=len(string))<br><br>返回 str 在 string 里面出现的次数，如果 beg 或者 end 指定则返回指定范围内 str 出现的次数 | 
| 4 | bytes.decode(encoding="utf-8", errors="strict")<br><br>Python3 中没有 decode 方法，但我们可以使用 bytes 对象的 decode() 方法来解码给定的 bytes 对象，这个 bytes 对象可以由 str.encode() 来编码返回。| 
| 5 | encode(encoding='UTF-8',errors='strict')<br><br>以 encoding 指定的编码格式编码字符串，如果出错默认报一个ValueError 的异常，除非 errors 指定的是'ignore'或者'replace' | 
| 6 | endswith(suffix, beg=0, end=len(string))<br><br>检查字符串是否以 obj 结束，如果beg 或者 end 指定则检查指定的范围内是否以 obj 结束，如果是，返回 True,否则返回 False.| 
| 7 | expandtabs(tabsize=8) <br><br>把字符串 string 中的 tab 符号转为空格，tab 符号默认的空格数是 8 。| 
| 8 | find(str, beg=0, end=len(string)) <br><br>检测 str 是否包含在字符串中，如果指定范围 beg 和 end ，则检查是否包含在指定范围内，如果包含返回开始的索引值，否则返回-1 | 
| 9 | index(str, beg=0, end=len(string)) <br><br>跟find()方法一样，只不过如果str不在字符串中会报一个异常.| 
| 10 | isalnum()<br><br>如果字符串至少有一个字符并且所有字符都是字母或数字则返 回 True,否则返回 False| 
| 11 | isalpha() <br><br>如果字符串至少有一个字符并且所有字符都是字母则返回 True, 否则返回 False | 
| 12 | isdigit()<br><br>如果字符串只包含数字则返回 True 否则返回 False..| 
| 13 | islower() <br><br> 如果字符串中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是小写，则返回 True，否则返回 False | 
| 14 | isnumeric() <br><br> 如果字符串中只包含数字字符，则返回 True，否则返回 False | 
| 15 | isspace() <br><br> 如果字符串中只包含空白，则返回 True，否则返回 False. | 
| 16 | istitle() <br><br> 如果字符串是标题化的(见 title())则返回 True，否则返回 False | 
| 17 | isupper() <br><br> 如果字符串中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是大写，则返回 True，否则返回 False | 
| 18 | join(seq) <br><br>以指定字符串作为分隔符，将 seq 中所有的元素(的字符串表示)合并为一个新的字符串 | 
| 19 | len(string) <br><br>返回字符串长度 | 
| 20 | ljust(width[, fillchar]) <br><br>返回一个原字符串左对齐,并使用 fillchar 填充至长度 width 的新字符串，fillchar 默认为空格。| 
| 21 | lower()  <br><br>转换字符串中所有大写字符为小写.| 
| 22 | lstrip() <br><br> 截掉字符串左边的空格或指定字符。 | 
| 23 | maketrans() <br><br>创建字符映射的转换表，对于接受两个参数的最简单的调用方式，第一个参数是字符串，表示需要转换的字符，第二个参数也是字符串表示转换的目标。| 
| 24 | max(str)<br><br> 返回字符串 str 中最大的字母。| 
| 25 | min(str) <br><br>返回字符串 str 中最小的字母。| 
| 26 | replace(old, new [, max])  <br><br>把 将字符串中的 str1 替换成 str2,如果 max 指定，则替换不超过 max 次。 | 
| 27 | rfind(str, beg=0,end=len(string))  <br><br>类似于 find()函数，不过是从右边开始查找. | 
| 28 | rindex( str, beg=0, end=len(string))  <br><br>类似于 index()，不过是从右边开始. | 
| 29 | rjust(width,[, fillchar])  <br><br>返回一个原字符串右对齐,并使用fillchar(默认空格）填充至长度 width 的新字符串 | 
| 30 | rstrip()  <br><br>删除字符串字符串末尾的空格. | 
| 31 | split(str="", num=string.count(str)) <br><br> num=string.count(str)) 以 str 为分隔符截取字符串，如果 num 有指定值，则仅截取 num+1 个子字符串 | 
| 32 | splitlines([keepends])  <br><br>按照行('\r', '\r\n', \n')分隔，返回一个包含各行作为元素的列表，如果参数 keepends 为 False，不包含换行符，如果为 True，则保留换行符。|  
| 33 | startswith(substr, beg=0,end=len(string))  <br><br>检查字符串是否是以指定子字符串 substr 开头，是则返回 True，否则返回 False。如果beg 和 end 指定值，则在指定范围内检查。| 
| 34 | strip([chars]) <br><br> 在字符串上执行 lstrip()和 rstrip() | 
| 35 | swapcase()  <br><br>将字符串中大写转换为小写，小写转换为大写| 
| 36 | title()  <br><br>返回"标题化"的字符串,就是说所有单词都是以大写开始，其余字母均为小写(见 istitle()) | 
| 37 | translate(table, deletechars="")  <br><br>根据 str 给出的表(包含 256 个字符)转换 string 的字符, 要过滤掉的字符放到 deletechars 参数中 | 
| 38 | upper() <br><br>转换字符串中的小写字母为大写（我们集训课堂会讲到哦，这里你先做预习） | 
| 39 | zfill (width) <br><br> 返回长度为 width 的字符串，原字符串右对齐，前面填充0 | 
| 40 | isdecimal() <br><br>检查字符串是否只包含十进制字符，如果是返回 true，否则返回 false。| 

还是老样子，你不需要都记住，你只需要记住，你要用的时候，可以回来找到他，上网搜索起来吧，我也给你做了一个example， 比如len()函数，点一下，你就去看看具体怎么解释的吧。真棒，这一关又学完了。

