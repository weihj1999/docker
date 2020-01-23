# Python高级

## 函数

编程中的函数不同于数学中的函数，函数在编程中是实打实的代码，而且在上一环节的学习中，我们已经学习了一些Python自带的内置函数，比如说print（）, len(),这些都是设定好的，可以直接拿来用的。眼尖的你肯定发现，函数后面都跟了一个（), 而括号里的东西，就是我们要输入的数据，它在函数中被称作【参数】，参数指向的是函数要接受和处理的数据

函数的实现功能由简单到复杂，这一关让我们好好来学习一下。学完这一关，希望你清楚明白def和return的关系，什么是全局变量和局部变量。

函数基础：（思维导图）
- 定义一个函数
- 函数调用
- 参数传递
- 参数
- 匿名函数
- return函数
- 变量作用域

函数是组织好的，可重复使用的，用来实现单一，或相关联功能的代码段。

函数能提高应用的模块性，和代码的重复利用率。你已经知道Python提供了许多内建函数，比如print()。但你也可以自己创建函数，这被叫做用户自定义函数。

1. 定义一个函数

你可以定义一个由自己想要功能的函数，以下是简单的规则：
- 函数代码块以 def 关键词开头，后接函数标识符名称和圆括号 ()。
- 任何传入参数和自变量必须放在圆括号中间，圆括号之间可以用于定义参数。
- 函数的第一行语句可以选择性地使用文档字符串—用于存放函数说明。
- 函数内容以冒号起始，并且缩进。
- **return [表达式]** 结束函数，选择性地返回一个值给调用方。不带表达式的return相当于返回 None。

2. 语法

Python 定义函数使用 def 关键字，一般格式如下：
```
def 函数名（参数列表）:
函数体
```

默认情况下，参数值和参数名称是按函数声明中定义的顺序匹配起来的。

**实例**

让我们使用函数来输出"Hello World！"：
```python
>>>def hello() :
print("Hello World!")


>>> hello()
Hello World!
>>>
```
更复杂点的应用，函数中带上参数变量:

```python
#!/usr/bin/python3

# 计算面积函数
def area(width, height):
  return width * height
  
  def print_welcome(name):
    print("Welcome", name)
    
    print_welcome("Runoob")
    w = 4
    h = 5
    print("width =", w, " height =", h, " area =", area(w, h))
```
以上实例输出结果：
```
Welcome Runoob
width = 4  height = 5  area = 20
```
3. 函数调用
定义一个函数：给了函数一个名称，指定了函数里包含的参数，和代码块结构。

这个函数的基本结构完成以后，你可以通过另一个函数调用执行，也可以直接从 Python 命令提示符执行。

如下实例调用了 printme() 函数：

```python
#!/usr/bin/python3

# 定义函数
def printme( str ):
  # 打印任何传入的字符串
  print (str)
  return
  
  # 调用函数
  printme("我要调用用户自定义函数!")
  printme("再次调用同一函数")
```
以上实例输出结果：
```
我要调用用户自定义函数!
再次调用同一函数
```
4. 参数传递

在 python 中，类型属于对象，变量是没有类型的：
```
a=[1,2,3]

a="Runoob"
```
以上代码中，[1,2,3] 是 List 类型，"Runoob" 是 String 类型，而变量 a 是没有类型，她仅仅是一个对象的引用（一个指针），可以是指向 List 类型对象，也可以是指向 String 类型对象。

可更改(mutable)与不可更改(immutable)对象

在 python 中，strings, tuples, 和 numbers 是不可更改的对象，而 list,dict 等则是可以修改的对象。

- 不可变类型：变量赋值 a=5 后再赋值 a=10，这里实际是新生成一个 int 值对象 10，再让 a 指向它，而 5 被丢弃，不是改变a的值，相当于新生成了a。

- 可变类型：变量赋值 la=[1,2,3,4] 后再赋值 la[2]=5 则是将 list la 的第三个元素值更改，本身la没有动，只是其内部的一部分值被修改了。

python 函数的参数传递：

- 不可变类型：类似 c++ 的值传递，如 整数、字符串、元组。如fun（a），传递的只是a的值，没有影响a对象本身。比如在 fun（a）内部修改 a 的值，只是修改另一个复制的对象，不会影响 a 本身。

- 可变类型：类似 c++ 的引用传递，如 列表，字典。如 fun（la），则是将 la 真正的传过去，修改后fun外部的la也会受影响

python 中一切都是对象，严格意义我们不能说值传递还是引用传递，我们应该说传不可变对象和传可变对象。

python 传不可变对象实例
```python
#!/usr/bin/python3

def ChangeInt( a ):
  a = 10
  
  b = 2
  ChangeInt(b)
  print( b ) # 结果是 2
```
实例中有 int 对象 2，指向它的变量是 b，在传递给 ChangeInt 函数时，按传值的方式复制了变量 b，a 和 b 都指向了同一个 Int 对象，在 a=10 时，则新生成一个 int 值对象 10，并让 a 指向它。

传可变对象实例

可变对象在函数里修改了参数，那么在调用这个函数的函数里，原始的参数也被改变了。例如：

```python
#!/usr/bin/python3

# 可写函数说明
def changeme( mylist ):
  "修改传入的列表"
  mylist.append([1,2,3,4])
  print ("函数内取值: ", mylist)
  return
  
  # 调用changeme函数
  mylist = [10,20,30]
  changeme( mylist )
  print ("函数外取值: ", mylist)
```

传入函数的和在末尾添加新内容的对象用的是同一个引用。故输出结果如下：
```
函数内取值:  [10, 20, 30, [1, 2, 3, 4]]
函数外取值:  [10, 20, 30, [1, 2, 3, 4]]
```
5. 参数

以下是调用函数时可使用的正式参数类型：

- 必需参数
- 关键字参数
- 默认参数
- 不定长参数

**A. 必需参数**

必需参数须以正确的顺序传入函数。调用时的数量必须和声明时的一样。

调用printme()函数，你必须传入一个参数，不然会出现语法错误：

```python
#!/usr/bin/python3

#可写函数说明
def printme( str ):
  "打印任何传入的字符串"
  print (str)
  return
  
  #调用printme函数
  printme()
```
以上实例输出结果：
```
Traceback (most recent call last):
File "test.py", line 10, in <module>
printme()
TypeError: printme() missing 1 required positional argument: 'str'
```
**B. 关键字参数**

关键字参数和函数调用关系紧密，函数调用使用关键字参数来确定传入的参数值。

使用关键字参数允许函数调用时参数的顺序与声明时不一致，因为 Python 解释器能够用参数名匹配参数值。

以下实例在函数 printme() 调用时使用参数名：

```python
#!/usr/bin/python3

#可写函数说明
def printme( str ):
  "打印任何传入的字符串"
  print (str)
  return
  
  #调用printme函数
  printme( str = "菜鸟教程")
```
以上实例输出结果：
```
菜鸟教程
```
以下实例中演示了函数参数的使用不需要使用指定顺序：

```python
#!/usr/bin/python3

#可写函数说明
def printinfo( name, age ):
  "打印任何传入的字符串"
  print ("名字: ", name)
  print ("年龄: ", age)
  return
  
  #调用printinfo函数
  printinfo( age=50, name="runoob" )
```
以上实例输出结果：
```
名字:  runoob
年龄:  50
```
**C. 默认参数**

调用函数时，如果没有传递参数，则会使用默认参数。以下实例中如果没有传入 age 参数，则使用默认值：

```python
#!/usr/bin/python3

#可写函数说明
def printinfo( name, age = 35 ):
  "打印任何传入的字符串"
  print ("名字: ", name)
  print ("年龄: ", age)
  return
  
  #调用printinfo函数
  printinfo( age=50, name="runoob" )
  print ("------------------------")
  printinfo( name="runoob" )
```
以上实例输出结果：
```
名字:  runoob
年龄:  50
------------------------
名字:  runoob
年龄:  35
```
**D. 不定长参数**

你可能需要一个函数能处理比当初声明时更多的参数。这些参数叫做不定长参数，和上述 2 种参数不同，声明时不会命名。基本语法如下：
```python
def functionname([formal_args,] *var_args_tuple ):
  "函数_文档字符串"
  function_suite
  return [expression]
```

加了星号 * 的参数会以元组(tuple)的形式导入，存放所有未命名的变量参数。

```python
#!/usr/bin/python3

# 可写函数说明
def printinfo( arg1, *vartuple ):
  "打印任何传入的参数"
  print ("输出: ")
  print (arg1)
  print (vartuple)
  
  # 调用printinfo 函数
  printinfo( 70, 60, 50 )
```
以上实例输出结果：
```
输出:
70
(60, 50)
```
如果在函数调用时没有指定参数，它就是一个空元组。我们也可以不向函数传递未命名的变量。如下实例：
```python
#!/usr/bin/python3

# 可写函数说明
def printinfo( arg1, *vartuple ):
  "打印任何传入的参数"
  print ("输出: ")
  print (arg1)
  for var in vartuple:
    print (var)
    return
    
    # 调用printinfo 函数
    printinfo( 10 )
    printinfo( 70, 60, 50 )
```
以上实例输出结果：
```
输出:
10
输出:
70
60
50
```
还有一种就是参数带两个星号 \**基本语法如下：
```python
def functionname([formal_args,] **var_args_dict ):
  "函数_文档字符串"
  function_suite
  return [expression]
```
加了两个星号 \** 的参数会以字典的形式导入。

```python
#!/usr/bin/python3

# 可写函数说明
def printinfo( arg1, **vardict ):
  "打印任何传入的参数"
  print ("输出: ")
  print (arg1)
  print (vardict)
  
  # 调用printinfo 函数
  printinfo(1, a=2,b=3)
```
以上实例输出结果：
```
输出:
1
{'a': 2, 'b': 3}
```

声明函数时，参数中星号 \* 可以单独出现，例如:
```python
def f(a,b,*,c):
  return a+b+c
```
如果单独出现星号 * 后的参数必须用关键字传入。
```python
>>> def f(a,b,*,c):
...     return a+b+c
...
>>> f(1,2,3)   # 报错
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: f() takes 2 positional arguments but 3 were given
>>> f(1,2,c=3) # 正常
6
>>>
```
**E. 匿名函数**

python 使用 lambda 来创建匿名函数。

所谓匿名，意即不再使用 def 语句这样标准的形式定义一个函数。

- lambda 只是一个表达式，函数体比 def 简单很多。
- lambda的主体是一个表达式，而不是一个代码块。仅仅能在lambda表达式中封装有限的逻辑进去。
- lambda 函数拥有自己的命名空间，且不能访问自己参数列表之外或全局命名空间里的参数。
- 虽然lambda函数看起来只能写一行，却不等同于C或C++的内联函数，后者的目的是调用小函数时不占用栈内存从而增加运行效率。
**语法**

lambda 函数的语法只包含一个语句，如下：
```python
lambda [arg1 [,arg2,.....argn]]:expression
```
如下实例：

```python
#!/usr/bin/python3

# 可写函数说明
sum = lambda arg1, arg2: arg1 + arg2

# 调用sum函数
print ("相加后的值为 : ", sum( 10, 20 ))
print ("相加后的值为 : ", sum( 20, 20 ))
以上实例输出结果：

相加后的值为 :  30
相加后的值为 :  40
```
6. RETURN语句

return [表达式] 语句用于退出函数，选择性地向调用方返回一个表达式。不带参数值的return语句返回None。之前的例子都没有示范如何返回数值，以下实例演示了 return 语句的用法：

```python
#!/usr/bin/python3

# 可写函数说明
def sum( arg1, arg2 ):
  # 返回2个参数的和."
  total = arg1 + arg2
  print ("函数内 : ", total)
  return total
  
  # 调用sum函数
  total = sum( 10, 20 )
  print ("函数外 : ", total)
```
以上实例输出结果：
```
函数内 :  30
函数外 :  30
```
7. 变量作用域

Python 中，程序的变量并不是在哪个位置都可以访问的，访问权限决定于这个变量是在哪里赋值的。

变量的作用域决定了在哪一部分程序可以访问哪个特定的变量名称。Python的作用域一共有4种，分别是：

- L （Local） 局部作用域
- E （Enclosing） 闭包函数外的函数中
- G （Global） 全局作用域
- B （Built-in） 内置作用域（内置函数所在模块的范围）

以 L –> E –> G –>B 的规则查找，即：在局部找不到，便会去局部外的局部找（例如闭包），再找不到就会去全局找，再者去内置中找。
```
g_count = 0  # 全局作用域
def outer():
o_count = 1  # 闭包函数外的函数中
def inner():
i_count = 2  # 局部作用域
```

内置作用域是通过一个名为 builtin 的标准模块来实现的，但是这个变量名自身并没有放入内置作用域内，所以必须导入这个文件才能够使用它。在Python3.0中，可以使用以下的代码来查看到底预定义了哪些变量:
```python
>>> import builtins
>>> dir(builtins)
```

Python 中只有模块（module），类（class）以及函数（def、lambda）才会引入新的作用域，其它的代码块（如 if/elif/else/、try/except、for/while等）是不会引入新的作用域的，也就是说这些语句内定义的变量，外部也可以访问，如下代码：
```python
>>> if True:
...  msg = 'I am from Runoob'
...
>>> msg
'I am from Runoob'
>>>
```

实例中 msg 变量定义在 if 语句块中，但外部还是可以访问的。

如果将 msg 定义在函数中，则它就是局部变量，外部不能访问：
```python
>>> def test():
...     msg_inner = 'I am from Runoob'
...
>>> msg_inner
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
NameError: name 'msg_inner' is not defined
>>>
```

从报错的信息上看，说明了 msg_inner 未定义，无法使用，因为它是局部变量，只有在函数内可以使用。

**A. 全局变量和局部变量**

定义在函数内部的变量拥有一个局部作用域，定义在函数外的拥有全局作用域。

局部变量只能在其被声明的函数内部访问，而全局变量可以在整个程序范围内访问。调用函数时，所有在函数内声明的变量名称都将被加入到作用域中。如下实例：

```python
#!/usr/bin/python3

total = 0 # 这是一个全局变量
# 可写函数说明
def sum( arg1, arg2 ):
  #返回2个参数的和."
  total = arg1 + arg2 # total在这里是局部变量.
  print ("函数内是局部变量 : ", total)
  return total
  
  #调用sum函数
  sum( 10, 20 )
  print ("函数外是全局变量 : ", total)
```
以上实例输出结果：
```
函数内是局部变量 :  30
函数外是全局变量 :  0
```
**B. global 和 nonlocal关键字**

当内部作用域想修改外部作用域的变量时，就要用到global和nonlocal关键字了。

以下实例修改全局变量 num：

```python
#!/usr/bin/python3

num = 1
def fun1():
  global num  # 需要使用 global 关键字声明
  print(num)
  num = 123
  print(num)
  fun1()
  print(num)
```
以上实例输出结果：
```
1
123
123
```

如果要修改嵌套作用域（enclosing 作用域，外层非全局作用域）中的变量则需要 nonlocal 关键字了，如下实例：

```python
#!/usr/bin/python3

def outer():
  num = 10
  def inner():
    nonlocal num   # nonlocal关键字声明
    num = 100
    print(num)
    inner()
    print(num)
    outer()
```
以上实例输出结果：
```
100
100
```
另外有一种特殊情况，假设下面这段代码被运行：

```python
#!/usr/bin/python3

a = 10
def test():
  a = a + 1
  print(a)
  test()
```
以上程序执行，报错信息如下：
```
Traceback (most recent call last):
File "test.py", line 7, in <module>
test()
File "test.py", line 5, in test
a = a + 1
UnboundLocalError: local variable 'a' referenced before assignment
```
错误信息为局部作用域引用错误，因为 test 函数中的 a 使用的是局部，未定义，无法修改。

修改 a 为全局变量，通过函数参数传递，可以正常执行输出结果为：

```python
#!/usr/bin/python3

a = 10
def test(a):
  a = a + 1
  print(a)
  test(a)
```
执行输出结果为：
```
11
```

## 错误与异常

我想和你讲个故事，此处放松一分钟。学习任何知识的时候都要追根溯源是不是？想不想知道bug这个经常有的Python问题到底是怎么来的呢？听我讲讲吧。想必在这段学习的过程中，最最让你头疼的是每次计算机发出的红色预警，每次你都信心满满的点击着运行，每次你的计算机都弹出来错误。你巨无奈（这里班主任感同身受，每次看到错误提示都分分钟想敲死计算机，怎么陪伴我这么久了还是那么蠢？）但是失败乃成功之母，班主任要讲故事了！

故事的主人公是被誉为计算机程序之母的格蕾丝.赫伯（Grace Hopper）。时光回到1947年，当时他正在为计算机编写程序，当时的计算机可是超级大的，不像现在摆在你面前的小小的笔记本。（请自行脑补巨大的计算机-马克二号），有一天她正在调试程序，结果老是出现故障。层层排查后，她拆了继电器，结果发现有只飞蛾被夹在了触点中间，从而卡住了计算机的运行，揪出了这只虫以后，格蕾丝幽默的把这只蛾子的尸体夹在了她工作的笔记本中并称它为bug,从此，bug就化身为计算机领域的程序故障的代名词。成为程序员一生如影随形的代名词，从而我们把排除故障的行动叫做debug.

故事讲完了，班主任也总结了千千万万学员程序出现bug的原因，现在总结下来，希望你规避。

错误产生的常见原因：
- 粗心
  - ==与=混用
  - 缩进错误
  - 误用中文符
  - 没有定义变量
  - 漏掉了语句结尾的；等符号
- 思路不清
  - print函数用起来随时打印
  - 用注释来提醒自己代码含义
- 知识不熟练
  - 多复习，查阅自己的笔记
  - 多去看别人写的代码，对比自己的代码思路
- 被动掉坑
  - try-except

  作为Python初学者，在刚学习Python编程时，经常会看到一些报错信息，在前面我们没有提及，这章节我们会专门介绍。
  
  Python有两种错误很容易辨认：语法错误和异常。

1. 语法错误
  
Python 的语法错误或者称之为解析错，是初学者经常碰到的，如下实例
```python
>>>while True print('Hello world')
File "<stdin>", line 1, in ?
while True print('Hello world')
^
SyntaxError: invalid syntax
```

这个例子中，函数 print() 被检查到有错误，是它前面缺少了一个冒号（:）。

语法分析器指出了出错的一行，并且在最先找到的错误的位置标记了一个小小的箭头。

2. 异常

即便Python程序的语法是正确的，在运行它的时候，也有可能发生错误。运行期检测到的错误被称为异常。

大多数的异常都不会被程序处理，都以错误信息的形式展现在这里:
```python
>>>10 * (1/0)
Traceback (most recent call last):
File "<stdin>", line 1, in ?
ZeroDivisionError: division by zero
>>> 4 + spam*3
Traceback (most recent call last):
File "<stdin>", line 1, in ?
NameError: name 'spam' is not defined
>>> '2' + 2
Traceback (most recent call last):
File "<stdin>", line 1, in ?
TypeError: Can't convert 'int' object to str implicitly
```

异常以不同的类型出现，这些类型都作为信息的一部分打印出来: 例子中的类型有 ZeroDivisionError，NameError 和 TypeError。

错误信息的前面部分显示了异常发生的上下文，并以调用栈的形式显示具体信息。

3. 异常处理

以下例子中，让用户输入一个合法的整数，但是允许用户中断这个程序（使用 Control-C 或者操作系统提供的方法）。用户中断的信息会引发一个 KeyboardInterrupt 异常。
```python
>>>while True:
try:
  x = int(input("Please enter a number: "))
  break
except ValueError:
  print("Oops!  That was no valid number.  Try again   ")
```
try语句按照如下方式工作；

- 首先，执行try子句（在关键字try和关键字except之间的语句）
- 如果没有异常发生，忽略except子句，try子句执行后结束。
- 如果在执行try子句的过程中发生了异常，那么try子句余下的部分将被忽略。如果异常的类型和 except 之后的名称相符，那么对应的except子句将被执行。最后执行 try 语句之后的代码。
- 如果一个异常没有与任何的except匹配，那么这个异常将会传递给上层的try中。

一个 try 语句可能包含多个except子句，分别来处理不同的特定的异常。最多只有一个分支会被执行。

处理程序将只针对对应的try子句中的异常进行处理，而不是其他的 try 的处理程序中的异常。

一个except子句可以同时处理多个异常，这些异常将被放在一个括号里成为一个元组，例如:
```python
except (RuntimeError, TypeError, NameError):
  pass
```

最后一个except子句可以忽略异常的名称，它将被当作通配符使用。你可以使用这种方法打印一个错误信息，然后再次把异常抛出。
```python
import sys

try:
  f = open('myfile.txt')
  s = f.readline()
  i = int(s.strip())
except OSError as err:
  print("OS error: {0}".format(err))
except ValueError:
  print("Could not convert data to an integer.")
except:
  print("Unexpected error:", sys.exc_info()[0])
  raise
```

try except 语句还有一个可选的else子句，如果使用这个子句，那么必须放在所有的except子句之后。这个子句将在try子句没有发生任何异常的时候执行。例如:
```python
for arg in sys.argv[1:]:
  try:
    f = open(arg, 'r')
  except IOError:
    print('cannot open', arg)
  else:
    print(arg, 'has', len(f.readlines()), 'lines')
    f.close()
```

使用 else 子句比把所有的语句都放在 try 子句里面要好，这样可以避免一些意想不到的、而except又没有捕获的异常。

异常处理并不仅仅处理那些直接发生在try子句中的异常，而且还能处理子句中调用的函数（甚至间接调用的函数）里抛出的异常。例如:
```python
>>>def this_fails():
x = 1/0

>>> try:
this_fails()
except ZeroDivisionError as err:
  print('Handling run-time error:', err)
  
  Handling run-time error: int division or modulo by zero
```

4. 抛出异常

Python 使用 raise 语句抛出一个指定的异常。例如:
```python
>>>raise NameError('HiThere')
Traceback (most recent call last):
File "<stdin>", line 1, in ?
NameError: HiThere
```

raise 唯一的一个参数指定了要被抛出的异常。它必须是一个异常的实例或者是异常的类（也就是 Exception 的子类）。

如果你只想知道这是否抛出了一个异常，并不想去处理它，那么一个简单的 raise 语句就可以再次把它抛出。
```python
>>>try:
raise NameError('HiThere')
except NameError:
  print('An exception flew by!')
  raise
  
  An exception flew by!
  Traceback (most recent call last):
  File "<stdin>", line 2, in ?
  NameError: HiThere
```

5. 用户自定义异常

你可以通过创建一个新的异常类来拥有自己的异常。异常类继承自 Exception 类，可以直接继承，或者间接继承，例如:
```python
>>>class MyError(Exception):
def __init__(self, value):
  self.value = value
  def __str__(self):
    return repr(self.value)
    
    >>> try:
    raise MyError(2*2)
  except MyError as e:
    print('My exception occurred, value:', e.value)
    
    My exception occurred, value: 4
    >>> raise MyError('oops!')
    Traceback (most recent call last):
    File "<stdin>", line 1, in ?
    __main__.MyError: 'oops!'
```

在这个例子中，类 Exception 默认的 __init__() 被覆盖。

当创建一个模块有可能抛出多种不同的异常时，一种通常的做法是为这个包建立一个基础异常类，然后基于这个基础类为不同的错误情况创建不同的子类:
```python
class Error(Exception):
  """Base class for exceptions in this module."""
  pass
  
  class InputError(Error):
    """Exception raised for errors in the input.
    
    Attributes:
    expression -- input expression in which the error occurred
    message -- explanation of the error
    """
    
    def __init__(self, expression, message):
      self.expression = expression
      self.message = message
      
      class TransitionError(Error):
        """Raised when an operation attempts a state transition that's not
        allowed.
        
        Attributes:
        previous -- state at beginning of transition
        next -- attempted new state
        message -- explanation of why the specific transition is not allowed
        """
        
        def __init__(self, previous, next, message):
          self.previous = previous
          self.next = next
          self.message = message
```

大多数的异常的名字都以"Error"结尾，就跟标准的异常命名一样。

6. 定义清理行为

try 语句还有另外一个可选的子句，它定义了无论在任何情况下都会执行的清理行为。 例如:
```python
>>>try:
...     raise KeyboardInterrupt
... finally:
...     print('Goodbye, world!')
... 
Goodbye, world!
Traceback (most recent call last):
File "<stdin>", line 2, in <module>
KeyboardInterrupt
```

以上例子不管 try 子句里面有没有发生异常，finally 子句都会执行。

如果一个异常在 try 子句里（或者在 except 和 else 子句里）被抛出，而又没有任何的 except 把它截住，那么这个异常会在 finally 子句执行后再次被抛出。

下面是一个更加复杂的例子（在同一个 try 语句里包含 except 和 finally 子句）:
```python
>>>def divide(x, y):
try:
  result = x / y
except ZeroDivisionError:
  print("division by zero!")
else:
  print("result is", result)
finally:
  print("executing finally clause")
  
  >>> divide(2, 1)
  result is 2.0
  executing finally clause
  >>> divide(2, 0)
  division by zero!
  executing finally clause
  >>> divide("2", "1")
  executing finally clause
  Traceback (most recent call last):
  File "<stdin>", line 1, in ?
  File "<stdin>", line 3, in divide
  TypeError: unsupported operand type(s) for /: 'str' and 'str'
```
7. 预定义的清理行为

一些对象定义了标准的清理行为，无论系统是否成功的使用了它，一旦不需要它了，那么这个标准的清理行为就会执行。

这面这个例子展示了尝试打开一个文件，然后把内容打印到屏幕上:
```python
for line in open("myfile.txt"):
  print(line, end="")
```

以上这段代码的问题是，当执行完毕后，文件会保持打开状态，并没有被关闭。

关键词 with 语句就可以保证诸如文件之类的对象在使用完之后一定会正确的执行他的清理方法:
```python
with open("myfile.txt") as f:
  for line in f:
    print(line, end="")
```
以上这段代码执行完毕后，就算在处理过程中出问题了，文件 f 总是会关闭。

