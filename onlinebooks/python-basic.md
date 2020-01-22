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
