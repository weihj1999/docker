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

## 类与对象

一言概括系列：

【类】是【对象】的母版，先有类，才能制造各种对象，就像我们先有了图纸，才能制作各种产品一样。

从图纸变成产品，也就是从【类】变成【实例对象】的过程，叫【实例化】

理解【类】最简单的方式：它就是一个函数包。（脑补一下你有很多东西，需要放进袋子里），类中可以放置函数与变量，然后类中的函数可以很方便的使用类中的变量。

在【类】中被定义的函数被称为【类】的【方法】，描述这个类能做什么，我们用类名.函数名（）的格式表示，就可以让类的方法运行起来。

在【类】中被定义的变量被称为【属性】。使用类名.变量名的格式，可以把类汇总的属性的值提取出来。

来吧，虽然有点难，但是类与对象这节是你必须攻克的难关，我相信你可以的。

Python3 面向对象

Python从设计之初就已经是一门面向对象的语言，正因为如此，在Python中创建一个类和对象是很容易的。本章节我们将详细介绍Python的面向对象编程。

如果你以前没有接触过面向对象的编程语言，那你可能需要先了解一些面向对象语言的一些基本特征，在头脑里头形成一个基本的面向对象的概念，这样有助于你更容易的学习Python的面向对象编程。

接下来我们先来简单的了解下面向对象的一些基本特征。

1. 面向对象技术简介

- 类(Class): 用来描述具有相同的属性和方法的对象的集合。它定义了该集合中每个对象所共有的属性和方法。对象是类的实例。
- 方法：类中定义的函数。
- 类变量：类变量在整个实例化的对象中是公用的。类变量定义在类中且在函数体之外。类变量通常不作为实例变量使用。
- 数据成员：类变量或者实例变量用于处理类及其实例对象的相关的数据。
- 方法重写：如果从父类继承的方法不能满足子类的需求，可以对其进行改写，这个过程叫方法的覆盖（override），也称为方法的重写。
- 局部变量：定义在方法中的变量，只作用于当前实例的类。
- 实例变量：在类的声明中，属性是用变量来表示的。这种变量就称为实例变量，是在类声明的内部但是在类的其他成员方法之外声明的。
- 继承：即一个派生类（derived class）继承基类（base class）的字段和方法。继承也允许把一个派生类的对象作为一个基类对象对待。例如，有这样一个设计：一个Dog类型的对象派生自Animal类，这是模拟"是一个（is-a）"关系（例图，Dog是一个Animal）。
- 实例化：创建一个类的实例，类的具体对象。
- 对象：通过类定义的数据结构实例。对象包括两个数据成员（类变量和实例变量）和方法。

和其它编程语言相比，Python 在尽可能不增加新的语法和语义的情况下加入了类机制。

Python中的类提供了面向对象编程的所有基本功能：类的继承机制允许多个基类，派生类可以覆盖基类中的任何方法，方法中可以调用基类中的同名方法。

对象可以包含任意数量和类型的数据。

2. 类定义

语法格式如下：
```python
class ClassName:
  <statement-1>
  .
  .
  .
  <statement-N>
```
类实例化后，可以使用其属性，实际上，创建一个类之后，可以通过类名访问其属性。

3. 类对象

类对象支持两种操作：属性引用和实例化。

属性引用使用和 Python 中所有的属性引用一样的标准语法：obj.name。

类对象创建后，类命名空间中所有的命名都是有效属性名。所以如果类定义是这样:

```python
#!/usr/bin/python3

class MyClass:
  """一个简单的类实例"""
  i = 12345
  def f(self):
    return 'hello world'

    # 实例化类
    x = MyClass()

    # 访问类的属性和方法
    print("MyClass 类的属性 i 为：", x.i)
    print("MyClass 类的方法 f 输出为：", x.f())
```

以上创建了一个新的类实例并将该对象赋给局部变量 x，x 为空的对象。

执行以上程序输出结果为：
```
MyClass 类的属性 i 为： 12345
MyClass 类的方法 f 输出为： hello world
```

类有一个名为 __init__() 的特殊方法（构造方法），该方法在类实例化时会自动调用，像下面这样：

```python
def __init__(self):
  self.data = []
```

类定义了 __init__() 方法，类的实例化操作会自动调用 __init__() 方法。如下实例化类 MyClass，对应的 __init__() 方法就会被调用:
```
x = MyClass()
```

当然， __init__() 方法可以有参数，参数通过 __init__() 传递到类的实例化操作上。例如:

```python
#!/usr/bin/python3

class Complex:
  def __init__(self, realpart, imagpart):
    self.r = realpart
    self.i = imagpart
    x = Complex(3.0, -4.5)
    print(x.r, x.i)   # 输出结果：3.0 -4.5
```

**self代表类的实例，而非类**

类的方法与普通的函数只有一个特别的区别——它们必须有一个额外的第一个参数名称, 按照惯例它的名称是 self。
```python
class Test:
  def prt(self):
    print(self)
    print(self.__class__)

    t = Test()
    t.prt()
```
以上实例执行结果为：
```
<__main__.Test instance at 0x100771878>
__main__.Test
```

从执行结果可以很明显的看出，self 代表的是类的实例，代表当前对象的地址，而 self.class 则指向类。

self 不是 python 关键字，我们把他换成 runoob 也是可以正常执行的:
```python
class Test:
  def prt(runoob):
    print(runoob)
    print(runoob.__class__)

    t = Test()
    t.prt()
```
以上实例执行结果为：
```
<__main__.Test instance at 0x100771878>
__main__.Test
```

4. 类的方法

在类的内部，使用 def 关键字来定义一个方法，与一般函数定义不同，类方法必须包含参数 self, 且为第一个参数，self 代表的是类的实例。

```python
#!/usr/bin/python3

#类定义
class people:
  #定义基本属性
  name = ''
  age = 0
  #定义私有属性,私有属性在类外部无法直接进行访问
  __weight = 0
  #定义构造方法
  def __init__(self,n,a,w):
    self.name = n
    self.age = a
    self.__weight = w
    def speak(self):
      print("%s 说: 我 %d 岁。" %(self.name,self.age))

      # 实例化类
      p = people('runoob',10,30)
      p.speak()
```
执行以上程序输出结果为：
```
runoob 说: 我 10 岁。
```
5. 继承

Python 同样支持类的继承，如果一种语言不支持继承，类就没有什么意义。派生类的定义如下所示:
```python
class DerivedClassName(BaseClassName1):
  <statement-1>
  .
  .
  .
  <statement-N>
```

需要注意圆括号中基类的顺序，若是基类中有相同的方法名，而在子类使用时未指定，python从左至右搜索 即方法在子类中未找到时，从左到右查找基类中是否包含方法。

BaseClassName（示例中的基类名）必须与派生类定义在一个作用域内。除了类，还可以用表达式，基类定义在另一个模块中时这一点非常有用:
```python
class DerivedClassName(modname.BaseClassName):
```
```python
#!/usr/bin/python3

#类定义
class people:
  #定义基本属性
  name = ''
  age = 0
  #定义私有属性,私有属性在类外部无法直接进行访问
  __weight = 0
  #定义构造方法
  def __init__(self,n,a,w):
    self.name = n
    self.age = a
    self.__weight = w
    def speak(self):
      print("%s 说: 我 %d 岁。" %(self.name,self.age))

#单继承示例
class student(people):
    grade = ''
    def __init__(self,n,a,w,g):
      #调用父类的构函
      people.__init__(self,n,a,w)
      self.grade = g
      #覆写父类的方法
      def speak(self):
          print("%s 说: 我 %d 岁了，我在读 %d 年级"%(self.name,self.age,self.grade))

s = student('ken',10,60,3)
s.speak()
```
执行以上程序输出结果为：
```
ken 说: 我 10 岁了，我在读 3 年级
```
6. 多继承

Python同样有限的支持多继承形式。多继承的类定义形如下例:
```python
class DerivedClassName(Base1, Base2, Base3):
  <statement-1>
  .
  .
  .
  <statement-N>
```

需要注意圆括号中父类的顺序，若是父类中有相同的方法名，而在子类使用时未指定，python从左至右搜索 即方法在子类中未找到时，从左到右查找父类中是否包含方法。

```python
#!/usr/bin/python3

#类定义
class people:
  #定义基本属性
  name = ''
  age = 0
  #定义私有属性,私有属性在类外部无法直接进行访问
  __weight = 0
  #定义构造方法
  def __init__(self,n,a,w):
    self.name = n
    self.age = a
    self.__weight = w
    def speak(self):
      print("%s 说: 我 %d 岁。" %(self.name,self.age))

#单继承示例
class student(people):
    grade = ''
    def __init__(self,n,a,w,g):
      #调用父类的构函
      people.__init__(self,n,a,w)
      self.grade = g
    #覆写父类的方法
    def speak(self):
        print("%s 说: 我 %d 岁了，我在读 %d 年级"%(self.name,self.age,self.grade))

#另一个类，多重继承之前的准备
class speaker():
    topic = ''
    name = ''
    def __init__(self,n,t):
        self.name = n
        self.topic = t
    def speak(self):
        print("我叫 %s，我是一个演说家，我演讲的主题是 %s"%(self.name,self.topic))

#多重继承
class sample(speaker,student):
    a =''
    def __init__(self,n,a,w,g,t):
        student.__init__(self,n,a,w,g)
        speaker.__init__(self,n,t)

test = sample("Tim",25,80,4,"Python")
test.speak()   #方法名同，默认调用的是在括号中排前地父类的方法
```
执行以上程序输出结果为：
```
我叫 Tim，我是一个演说家，我演讲的主题是 Python
```
7. 方法重写

如果你的父类方法的功能不能满足你的需求，你可以在子类重写你父类的方法，实例如下：

```python
#!/usr/bin/python3

class Parent:        # 定义父类
def myMethod(self):
print ('调用父类方法')

class Child(Parent): # 定义子类
def myMethod(self):
print ('调用子类方法')

c = Child()          # 子类实例
c.myMethod()         # 子类调用重写方法
super(Child,c).myMethod() #用子类对象调用父类已被覆盖的方法
```
super() 函数是用于调用父类(超类)的一个方法。

执行以上程序输出结果为：
```
调用子类方法
调用父类方法
```

8. 类属性与方法

类的私有属性

\__private_attrs：两个下划线开头，声明该属性为私有，不能在类的外部被使用或直接访问。在类内部的方法中使用时 self.\__private_attrs。

**类的方法**

在类的内部，使用 def 关键字来定义一个方法，与一般函数定义不同，类方法必须包含参数 self，且为第一个参数，self 代表的是类的实例。

self 的名字并不是规定死的，也可以使用 this，但是最好还是按照约定是用 self。

类的私有方法

\__private_method：两个下划线开头，声明该方法为私有方法，只能在类的内部调用 ，不能在类的外部调用。self.\__private_methods。

实例

类的私有属性实例如下：

```python
#!/usr/bin/python3

class JustCounter:
  __secretCount = 0  # 私有变量
  publicCount = 0    # 公开变量

  def count(self):
    self.__secretCount += 1
    self.publicCount += 1
    print (self.__secretCount)

    counter = JustCounter()
    counter.count()
    counter.count()
    print (counter.publicCount)
    print (counter.__secretCount)  # 报错，实例不能访问私有变量
```
执行以上程序输出结果为：
```
1
2
2
Traceback (most recent call last):
File "test.py", line 16, in <module>
print (counter.__secretCount)  # 报错，实例不能访问私有变量
AttributeError: 'JustCounter' object has no attribute '__secretCount'
```
类的私有方法实例如下：

```python
#!/usr/bin/python3

class Site:
  def __init__(self, name, url):
    self.name = name       # public
    self.__url = url   # private

    def who(self):
      print('name  : ', self.name)
      print('url : ', self.__url)

      def __foo(self):          # 私有方法
      print('这是私有方法')

      def foo(self):            # 公共方法
      print('这是公共方法')
      self.__foo()

      x = Site('菜鸟教程', 'www.runoob.com')
      x.who()        # 正常输出
      x.foo()        # 正常输出
      x.__foo()      # 报错
```
以上实例执行结果：

自行补充


**类的专有方法：**

- \__init__ : 构造函数，在生成对象时调用
- \__del__ : 析构函数，释放对象时使用
- \__repr__ : 打印，转换
- \__setitem__ : 按照索引赋值
- \__getitem__: 按照索引获取值
- \__len__: 获得长度
- \__cmp__: 比较运算
- \__call__: 函数调用
- \__add__: 加运算
- \__sub__: 减运算
- \__mul__: 乘运算
- \__truediv__: 除运算
- \__mod__: 求余运算
- \__pow__: 乘方

9. 运算符重载

Python同样支持运算符重载，我们可以对类的专有方法进行重载，实例如下：

```python
#!/usr/bin/python3

class Vector:
  def __init__(self, a, b):
    self.a = a
    self.b = b

    def __str__(self):
      return 'Vector (%d, %d)' % (self.a, self.b)

      def __add__(self,other):
        return Vector(self.a + other.a, self.b + other.b)

        v1 = Vector(2,10)
        v2 = Vector(5,-2)
        print (v1 + v2)
```
以上代码执行结果如下所示:
```
Vector(7,8)
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

## 模块

有一本讲Python的书里是这样描述模块的：“模块是高级别的程序组织单元”什么意思呢？其实模块什么都可以封装。在模块中，我们不但可以直接存放变量，还可以存放函数，还可以存放类。我给你总结一下，你研究一下下面的图片，看能不能找到内部逻辑关系？

从主模块导入，模块内包含变量，函数，类，类中间同样包含变量，函数；以及其他可执行语句，仅此而已。

那你和我一起看一下，顺便你也复习一下

- 模块：“.py”后缀的文件就是模块
- 类：使用class语句来封装一个类
- 函数：使用def语句来封装一个函数
- 变量：使用赋值语句来赋值一个变量

这些知识点，我希望你们还记得住。

那么问题来了：为什么要封装模块呢？（班主任希望你们自己想想，想不到再继续学习）

回答：封装模块的目的是为了把程序代码和数据存放起来以便再次利用，如果封装成类和函数，主要是便于自己调用，但是封装成了模块，不仅可以自己使用，文件的方式也很容易共享给其他人使用。

```python
import test  # 导入test模块

print(test.a)  # 使用“模块.变量”调用模块中的变量

test.hi()  # 使用“模块.函数()”调用模块中的函数

print(test.Go1.a)   # 使用“模块.类.变量”调用模块中的类属性
test.Go1.do1()  # 使用“模块.类.函数()”调用模块中的类方法

A = test.Go2()  # 使用“变量 = 模块.类()”实例化模块中的类
print(A.a)  # 实例化后，不再需要“模块.”
A.do2()  # 实例化后，不再需要“模块.”
```

在前面的几个章节中我们脚本上是用 python 解释器来编程，如果你从 Python 解释器退出再进入，那么你定义的所有的方法和变量就都消失了。

为此 Python 提供了一个办法，把这些定义存放在文件中，为一些脚本或者交互式的解释器实例使用，这个文件被称为模块。

模块是一个包含所有你定义的函数和变量的文件，其后缀名是.py。模块可以被别的程序引入，以使用该模块中的函数等功能。这也是使用 python 标准库的方法。

下面是一个使用 python 标准库中模块的例子。

```python
#!/usr/bin/python3
# 文件名: using_sys.py

import sys

print('命令行参数如下:')
for i in sys.argv:
   print(i)

print('\n\nPython 路径为：', sys.path, '\n')
```

执行结果如下所示：
```
$ python using_sys.py 参数1 参数2
命令行参数如下:
using_sys.py
参数1
参数2


Python 路径为： ['/root', '/usr/lib/python3.4', '/usr/lib/python3.4/plat-x86_64-linux-gnu', '/usr/lib/python3.4/lib-dynload', '/usr/local/lib/python3.4/dist-packages', '/usr/lib/python3/dist-packages']
```
- 1、import sys 引入 python 标准库中的 sys.py 模块；这是引入某一模块的方法。
- 2、sys.argv 是一个包含命令行参数的列表。
- 3、sys.path 包含了一个 Python 解释器自动查找所需模块的路径的列表。

1. IMPORT 语句

想使用 Python 源文件，只需在另一个源文件里执行 import 语句，语法如下：
```
import module1[, module2[,... moduleN]
```
当解释器遇到 import 语句，如果模块在当前的搜索路径就会被导入。

搜索路径是一个解释器会先进行搜索的所有目录的列表。如想要导入模块 support，需要把命令放在脚本的顶端：

SUPPORT.PY 文件代码
```python
#!/usr/bin/python3
# Filename: support.py

def print_func( par ):
  print ("Hello : ", par)
  return
```
test.py 引入 support 模块：

TEST.PY 文件代码
```python
#!/usr/bin/python3
# Filename: test.py

# 导入模块
import support

# 现在可以调用模块里包含的函数了
support.print_func("Runoob")
```
以上实例输出结果：
```
$ python3 test.py
Hello :  Runoob
```
一个模块只会被导入一次，不管你执行了多少次import。这样可以防止导入模块被一遍又一遍地执行。

当我们使用import语句的时候，Python解释器是怎样找到对应的文件的呢？

这就涉及到Python的搜索路径，搜索路径是由一系列目录名组成的，Python解释器就依次从这些目录中去寻找所引入的模块。

这看起来很像环境变量，事实上，也可以通过定义环境变量的方式来确定搜索路径。

搜索路径是在Python编译或安装的时候确定的，安装新的库应该也会修改。搜索路径被存储在sys模块中的path变量，做一个简单的实验，在交互式解释器中，输入以下代码：
```python

>>> import sys
>>> sys.path
['', '/usr/lib/python3.4', '/usr/lib/python3.4/plat-x86_64-linux-gnu', '/usr/lib/python3.4/lib-dynload', '/usr/local/lib/python3.4/dist-packages', '/usr/lib/python3/dist-packages']
>>>
```
sys.path 输出是一个列表，其中第一项是空串''，代表当前目录（若是从一个脚本中打印出来的话，可以更清楚地看出是哪个目录），亦即我们执行python解释器的目录（对于脚本的话就是运行的脚本所在的目录）。

因此若像我一样在当前目录下存在与要引入模块同名的文件，就会把要引入的模块屏蔽掉。

了解了搜索路径的概念，就可以在脚本中修改sys.path来引入一些不在搜索路径中的模块。

现在，在解释器的当前目录或者 sys.path 中的一个目录里面来创建一个fibo.py的文件，代码如下：

实例
```python
# 斐波那契(fibonacci)数列模块

def fib(n):    # 定义到 n 的斐波那契数列
a, b = 0, 1
while b < n:
  print(b, end=' ')
  a, b = b, a+b
  print()

  def fib2(n): # 返回到 n 的斐波那契数列
  result = []
  a, b = 0, 1
  while b < n:
    result.append(b)
    a, b = b, a+b
    return result
```
然后进入Python解释器，使用下面的命令导入这个模块：
```python
>>> import fibo
```
这样做并没有把直接定义在fibo中的函数名称写入到当前符号表里，只是把模块fibo的名字写到了那里。

可以使用模块名称来访问函数：

实例
```python
>>>fibo.fib(1000)
1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987
>>> fibo.fib2(100)
[1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
>>> fibo.__name__
'fibo'
```
如果你打算经常使用一个函数，你可以把它赋给一个本地的名称：
```python
>>> fib = fibo.fib
>>> fib(500)
1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

2. FROM … IMPORT 语句

Python 的 from 语句让你从模块中导入一个指定的部分到当前命名空间中，语法如下：
```python
from modname import name1[, name2[, ... nameN]]
```
例如，要导入模块 fibo 的 fib 函数，使用如下语句：
```python
>>> from fibo import fib, fib2
>>> fib(500)
1 1 2 3 5 8 13 21 34 55 89 144 233 377
```
这个声明不会把整个fibo模块导入到当前的命名空间中，它只会将fibo里的fib函数引入进来。


3. FROM … IMPORT * 语句

把一个模块的所有内容全都导入到当前的命名空间也是可行的，只需使用如下声明：
```python
from modname import *
```
这提供了一个简单的方法来导入一个模块中的所有项目。然而这种声明不该被过多地使用。

4. 深入模块

模块除了方法定义，还可以包括可执行的代码。这些代码一般用来初始化这个模块。这些代码只有在第一次被导入时才会被执行。

每个模块有各自独立的符号表，在模块内部为所有的函数当作全局符号表来使用。

所以，模块的作者可以放心大胆的在模块内部使用这些全局变量，而不用担心把其他用户的全局变量搞花。

从另一个方面，当你确实知道你在做什么的话，你也可以通过 modname.itemname 这样的表示法来访问模块内的函数。

模块是可以导入其他模块的。在一个模块（或者脚本，或者其他地方）的最前面使用 import 来导入一个模块，当然这只是一个惯例，而不是强制的。被导入的模块的名称将被放入当前操作的模块的符号表中。

还有一种导入的方法，可以使用 import 直接把模块内（函数，变量的）名称导入到当前操作模块。比如:
```python
>>> from fibo import fib, fib2
>>> fib(500)
1 1 2 3 5 8 13 21 34 55 89 144 233 377
```
这种导入的方法不会把被导入的模块的名称放在当前的字符表中（所以在这个例子里面，fibo 这个名称是没有定义的）。

这还有一种方法，可以一次性的把模块中的所有（函数，变量）名称都导入到当前模块的字符表:
```python
>>> from fibo import *
>>> fib(500)
1 1 2 3 5 8 13 21 34 55 89 144 233 377
```
这将把所有的名字都导入进来，但是那些由单一下划线（\_）开头的名字不在此例。大多数情况， Python程序员不使用这种方法，因为引入的其它来源的命名，很可能覆盖了已有的定义。

5. \__NAME__属性

一个模块被另一个程序第一次引入时，其主程序将运行。如果我们想在模块被引入时，模块中的某一程序块不执行，我们可以用__name__属性来使该程序块仅在该模块自身运行时执行。
```python
#!/usr/bin/python3
# Filename: using_name.py

if __name__ == '__main__':
  print('程序自身在运行')
else:
  print('我来自另一模块')
```
运行输出如下：
```
$ python using_name.py
程序自身在运行
```
```
$ python
>>> import using_name
我来自另一模块
>>>
```
说明： 每个模块都有一个__name__属性，当其值是'\__main__'时，表明该模块自身在运行，否则是被引入。

说明：\__name__ 与 \__main__ 底下是双下划线， _ _ 是这样去掉中间的那个空格。

6. DIR() 函数

内置的函数 dir() 可以找到模块内定义的所有名称。以一个字符串列表的形式返回:
```python
</p>
<pre>
>>> import fibo, sys
>>> dir(fibo)
['__name__', 'fib', 'fib2']
>>> dir(sys)  
['__displayhook__', '__doc__', '__excepthook__', '__loader__', '__name__',
'__package__', '__stderr__', '__stdin__', '__stdout__',
'_clear_type_cache', '_current_frames', '_debugmallocstats', '_getframe',
'_home', '_mercurial', '_xoptions', 'abiflags', 'api_version', 'argv',
'base_exec_prefix', 'base_prefix', 'builtin_module_names', 'byteorder',
'call_tracing', 'callstats', 'copyright', 'displayhook',
'dont_write_bytecode', 'exc_info', 'excepthook', 'exec_prefix',
'executable', 'exit', 'flags', 'float_info', 'float_repr_style',
'getcheckinterval', 'getdefaultencoding', 'getdlopenflags',
'getfilesystemencoding', 'getobjects', 'getprofile', 'getrecursionlimit',
'getrefcount', 'getsizeof', 'getswitchinterval', 'gettotalrefcount',
'gettrace', 'hash_info', 'hexversion', 'implementation', 'int_info',
'intern', 'maxsize', 'maxunicode', 'meta_path', 'modules', 'path',
'path_hooks', 'path_importer_cache', 'platform', 'prefix', 'ps1',
'setcheckinterval', 'setdlopenflags', 'setprofile', 'setrecursionlimit',
'setswitchinterval', 'settrace', 'stderr', 'stdin', 'stdout',
'thread_info', 'version', 'version_info', 'warnoptions']
```
如果没有给定参数，那么 dir() 函数会罗列出当前定义的所有名称:
```python
>>> a = [1, 2, 3, 4, 5]
>>> import fibo
>>> fib = fibo.fib
>>> dir() # 得到一个当前模块中定义的属性列表
['__builtins__', '__name__', 'a', 'fib', 'fibo', 'sys']
>>> a = 5 # 建立一个新的变量 'a'
>>> dir()
['__builtins__', '__doc__', '__name__', 'a', 'sys']
>>>
>>> del a # 删除变量名a
>>>
>>> dir()
['__builtins__', '__doc__', '__name__', 'sys']
>>>
```
7. 标准模块

Python 本身带着一些标准的模块库，在 Python 库参考文档中将会介绍到（就是后面的"库参考文档"）。

有些模块直接被构建在解析器里，这些虽然不是一些语言内置的功能，但是他却能很高效的使用，甚至是系统级调用也没问题。

这些组件会根据不同的操作系统进行不同形式的配置，比如 winreg 这个模块就只会提供给 Windows 系统。

应该注意到这有一个特别的模块 sys ，它内置在每一个 Python 解析器中。变量 sys.ps1 和 sys.ps2 定义了主提示符和副提示符所对应的字符串:
```python
>>> import sys
>>> sys.ps1
'>>> '
>>> sys.ps2
'... '
>>> sys.ps1 = 'C> '
C> print('Yuck!')
Yuck!
C>
```
8. 包

包是一种管理 Python 模块命名空间的形式，采用"点模块名称"。

比如一个模块的名称是 A.B， 那么他表示一个包 A中的子模块 B 。

就好像使用模块的时候，你不用担心不同模块之间的全局变量相互影响一样，采用点模块名称这种形式也不用担心不同库之间的模块重名的情况。

这样不同的作者都可以提供 NumPy 模块，或者是 Python 图形库。

不妨假设你想设计一套统一处理声音文件和数据的模块（或者称之为一个"包"）。

现存很多种不同的音频文件格式（基本上都是通过后缀名区分的，例如： .wav，:file:.aiff，:file:.au，），所以你需要有一组不断增加的模块，用来在不同的格式之间转换。

并且针对这些音频数据，还有很多不同的操作（比如混音，添加回声，增加均衡器功能，创建人造立体声效果），所以你还需要一组怎么也写不完的模块来处理这些操作。

这里给出了一种可能的包结构（在分层的文件系统中）:
```
sound/                          顶层包
      __init__.py               初始化 sound 包
      formats/                  文件格式转换子包
              __init__.py
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/                  声音效果子包
              __init__.py
              echo.py
              surround.py
              reverse.py
              ...
      filters/                  filters 子包
              __init__.py
              equalizer.py
              vocoder.py
              karaoke.py
              ...
```

在导入一个包的时候，Python 会根据 sys.path 中的目录来寻找这个包中包含的子目录。

目录只有包含一个叫做 \__init__.py 的文件才会被认作是一个包，主要是为了避免一些滥俗的名字（比如叫做 string）不小心的影响搜索路径中的有效模块。

最简单的情况，放一个空的 :file:\__init__.py就可以了。当然这个文件中也可以包含一些初始化代码或者为（将在后面介绍的） \__all__变量赋值。

用户可以每次只导入一个包里面的特定模块，比如:
```python
import sound.effects.echo
```
这将会导入子模块:sound.effects.echo。 他必须使用全名去访问:
```python
sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)
```
还有一种导入子模块的方法是:
```python
from sound.effects import echo
```
这同样会导入子模块: echo，并且他不需要那些冗长的前缀，所以他可以这样使用:
```python
echo.echofilter(input, output, delay=0.7, atten=4)
```
还有一种变化就是直接导入一个函数或者变量:
```python
from sound.effects.echo import echofilter
```
同样的，这种方法会导入子模块: echo，并且可以直接使用他的 echofilter() 函数:
```python
echofilter(input, output, delay=0.7, atten=4)
```

注意当使用from package import item这种形式的时候，对应的item既可以是包里面的子模块（子包），或者包里面定义的其他名称，比如函数，类或者变量。

import语法会首先把item当作一个包定义的名称，如果没找到，再试图按照一个模块去导入。如果还没找到，恭喜，一个:exc:ImportError 异常被抛出了。

反之，如果使用形如import item.subitem.subsubitem这种导入形式，除了最后一项，都必须是包，而最后一项则可以是模块或者是包，但是不可以是类，函数或者变量的名字。

9. 从一个包中导入*

设想一下，如果我们使用 from sound.effects import \*会发生什么？

Python 会进入文件系统，找到这个包里面所有的子模块，一个一个的把它们都导入进来。

但是很不幸，这个方法在 Windows平台上工作的就不是非常好，因为Windows是一个大小写不区分的系统。

在这类平台上，没有人敢担保一个叫做 ECHO.py 的文件导入为模块 echo 还是 Echo 甚至 ECHO。

（例如，Windows 95就很讨厌的把每一个文件的首字母大写显示）而且 DOS 的 8+3 命名规则对长模块名称的处理会把问题搞得更纠结。

为了解决这个问题，只能烦劳包作者提供一个精确的包的索引了。

导入语句遵循如下规则：如果包定义文件 \__init__.py 存在一个叫做 \__all__ 的列表变量，那么在使用 from package import * 的时候就把这个列表中的所有名字作为包内容导入。

作为包的作者，可别忘了在更新包之后保证 \__all__ 也更新了啊。你说我就不这么做，我就不使用导入*这种用法，好吧，没问题，谁让你是老板呢。这里有一个例子，在:file:sounds/effects/\__init__.py中包含如下代码:
```python
__all__ = ["echo", "surround", "reverse"]
```
这表示当你使用from sound.effects import \*这种用法时，你只会导入包里面这三个子模块。

如果 \__all__ 真的没有定义，那么使用from sound.effects import \*这种语法的时候，就不会导入包 sound.effects 里的任何子模块。他只是把包sound.effects和它里面定义的所有内容导入进来（可能运行__init__.py里定义的初始化代码）。

这会把 \__init__.py 里面定义的所有名字导入进来。并且他不会破坏掉我们在这句话之前导入的所有明确指定的模块。看下这部分代码:
```python
import sound.effects.echo
import sound.effects.surround
from sound.effects import *
```
这个例子中，在执行from...import前，包sound.effects中的echo和surround模块都被导入到当前的命名空间中了。（当然如果定义了\__all__就更没问题了）

通常我们并不主张使用\*这种方法来导入模块，因为这种方法经常会导致代码的可读性降低。不过这样倒的确是可以省去不少敲键的功夫，而且一些模块都设计成了只能通过特定的方法导入。

记住，使用from Package import specific_submodule这种方法永远不会有错。事实上，这也是推荐的方法。除非是你要导入的子模块有可能和其他包的子模块重名。

如果在结构中包是一个子包（比如这个例子中对于包sound来说），而你又想导入兄弟包（同级别的包）你就得使用导入绝对的路径来导入。比如，如果模块sound.filters.vocoder 要使用包sound.effects中的模块echo，你就要写成 from sound.effects import echo。
```python
from . import echo
from .. import formats
from ..filters import equalizer
```
无论是隐式的还是显式的相对导入都是从当前模块开始的。主模块的名字永远是"\__main__"，一个Python应用程序的主模块，应当总是使用绝对路径引用。

包还提供一个额外的属性\__path__。这是一个目录列表，里面每一个包含的目录都有为这个包服务的\__init__.py，你得在其他\__init__.py被执行前定义哦。可以修改这个变量，用来影响包含在包里面的模块和子包。

这个功能并不常用，一般用来扩展包里面的模块。

这里想提醒大家的是，比较小的模块，例如time， random等，我们可以通过这样的方式自学，但是大星模块的学习是比较困难的，除非你有相关的基础知识，例如数据分析需要用到的pandas和NumPy模块，网页开发需要的Django模块等等，这些大型模块你需要自己去进行学习了，但是我在这里还是想和大家强调一下。当你需要查看模块中有哪些函数可用的时候，不要忘记dir()！

在这里，有一个模块叫CSV， 即简单又好用，如果你有时间的话，可以去学习一下：
https://yiyibooks.cn/xx/python_352/library/csv.html#module-csv


