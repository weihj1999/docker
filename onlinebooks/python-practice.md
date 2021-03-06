# 更接近实战的编程练习
所有高级实战代码都可以在https://www.runoob.com/python3/python3-tutorial.html找到，这里摘录一部分供参考理解。
## 文件/目录方法

os 模块提供了非常丰富的方法用来处理文件和目录。常用的方法如下表所示：

| 序号	| 方法及描述 |
| --- | --- |
| 1	| os.access(path, mode) <br>检验权限模式 |
| 2	| os.chdir(path) <br>改变当前工作目录|
| 3	| os.chflags(path, flags) <br>设置路径的标记为数字标记。|
| 4	| os.chmod(path, mode) <br>更改权限|
| 5	| os.chown(path, uid, gid) <br>更改文件所有者|
| 6	| os.chroot(path) <br> 改变当前进程的根目录|
| 7	| os.close(fd) <br> 关闭文件描述符 fd|
| 8	| os.closerange(fd_low, fd_high) <br>关闭所有文件描述符，从 fd_low (包含) 到 fd_high (不包含), 错误会忽略|
| 9	| os.dup(fd) <br>复制文件描述符 fd|
| 10	| os.dup2(fd, fd2) <br>将一个文件描述符 fd 复制到另一个 fd2|
| 11	| os.fchdir(fd) <br> 通过文件描述符改变当前工作目录|
| 12	| os.fchmod(fd, mode) <br>改变一个文件的访问权限，该文件由参数fd指定，参数mode是Unix下的文件访问权限。|
| 13	| os.fchown(fd, uid, gid) <br>修改一个文件的所有权，这个函数修改一个文件的用户ID和用户组ID，该文件由文件描述符fd指定。|
| 14	| os.fdatasync(fd) <br> 强制将文件写入磁盘，该文件由文件描述符fd指定，但是不强制更新文件的状态信息。|
| 15	| os.fdopen(fd[, mode[, bufsize]]) <br>通过文件描述符 fd 创建一个文件对象，并返回这个文件对象|
| 16	| os.fpathconf(fd, name) <br> 返回一个打开的文件的系统配置信息。name为检索的系统配置的值，它也许是一个定义系统值的字符串，这些名字在很多标准中指定（POSIX.1, Unix 95, Unix 98, 和其它）。|
| 17	| os.fstat(fd) <br> 返回文件描述符fd的状态，像stat()。|
| 18	| os.fstatvfs(fd) <br> 返回包含文件描述符fd的文件的文件系统的信息，像 statvfs()|
| 19	| os.fsync(fd) <br> 强制将文件描述符为fd的文件写入硬盘。|
| 20	| os.ftruncate(fd, length) <br> 裁剪文件描述符fd对应的文件, 所以它最大不能超过文件大小。|
| 21	| os.getcwd() <br> 返回当前工作目录|
| 22	| os.getcwdu() <br> 返回一个当前工作目录的Unicode对象|
| 23	| os.isatty(fd) <br> 如果文件描述符fd是打开的，同时与tty(-like)设备相连，则返回true, 否则False。|
| 24	| os.lchflags(path, flags) <br> 设置路径的标记为数字标记，类似 chflags()，但是没有软链接|
| 25	| os.lchmod(path, mode) <br> 修改连接文件权限|
| 26	| os.lchown(path, uid, gid) <br> 更改文件所有者，类似 chown，但是不追踪链接。|
| 27	| os.link(src, dst) <br> 创建硬链接，名为参数 dst，指向参数 src|
| 28	| os.listdir(path) <br> 返回path指定的文件夹包含的文件或文件夹的名字的列表。|
| 29	| os.lseek(fd, pos, how) <br> 设置文件描述符 fd当前位置为pos, how方式修改: SEEK_SET 或者 0 设置从文件开始的计算的pos; SEEK_CUR或者 1 则从当前位置计算; os.SEEK_END或者2则从文件尾部开始. 在unix，Windows中有效|
| 30	| os.lstat(path) <br> 像stat(),但是没有软链接|
| 31	| os.major(device) <br> 从原始的设备号中提取设备major号码 (使用stat中的st_dev或者st_rdev field)。|
| 32	| os.makedev(major, minor) <br>以major和minor设备号组成一个原始设备号|
| 33	| os.makedirs(path[, mode]) <br> 递归文件夹创建函数。像mkdir(), 但创建的所有intermediate-level文件夹需要包含子文件夹。|
| 34	| os.minor(device) <br> 从原始的设备号中提取设备minor号码 (使用stat中的st_dev或者st_rdev field )。|
| 35	| os.mkdir(path[, mode]) <br> 以数字mode的mode创建一个名为path的文件夹.默认的 mode 是 0777 (八进制)。|
| 36	| os.mkfifo(path[, mode]) <br> 创建命名管道，mode 为数字，默认为 0666 (八进制)|
| 37	| os.mknod(filename[, mode=0600, device]) <br> 创建一个名为filename文件系统节点（文件，设备特别文件或者命名pipe）。|
| 38	| os.open(file, flags[, mode]) <br>打开一个文件，并且设置需要的打开选项，mode参数是可选的
| 39	| os.openpty() <br> 打开一个新的伪终端对。返回 pty 和 tty的文件描述符。|
| 40	| os.pathconf(path, name) <br>返回相关文件的系统配置信息。|
| 41	| os.pipe() <br> 创建一个管道. 返回一对文件描述符(r, w) 分别为读和写|
| 42	| os.popen(command[, mode[, bufsize]]) <br> 从一个 command 打开一个管道|
| 43	| os.read(fd, n) <br> 从文件描述符 fd 中读取最多 n 个字节，返回包含读取字节的字符串，文件描述符 fd对应文件已达到结尾, 返回一个空字符串。|
| 44	| os.readlink(path) <br> 返回软链接所指向的文件|
| 45	| os.remove(path) <br> 删除路径为path的文件。如果path 是一个文件夹，将抛出OSError; 查看下面的rmdir()删除一个 directory。|
| 46	| os.removedirs(path) <br>递归删除目录。|
| 47	| os.rename(src, dst) <br> 重命名文件或目录，从 src 到 dst|
| 48	| os.renames(old, new) <br> 递归地对目录进行更名，也可以对文件进行更名。|
| 49	| os.rmdir(path) <br> 删除path指定的空目录，如果目录非空，则抛出一个OSError异常。|
| 50	| os.stat(path) <br> 获取path指定的路径的信息，功能等同于C API中的stat()系统调用。|
| 51	| os.stat_float_times([newvalue]) <br> 决定stat_result是否以float对象显示时间戳|
| 52	| os.statvfs(path) <br> 获取指定路径的文件系统统计信息|
| 53	| os.symlink(src, dst) <br> 创建一个软链接|
| 54	| os.tcgetpgrp(fd) <br> 返回与终端fd（一个由os.open()返回的打开的文件描述符）关联的进程组|
| 55	| os.tcsetpgrp(fd, pg) <br> 设置与终端fd（一个由os.open()返回的打开的文件描述符）关联的进程组为pg。|
| 56	| os.tempnam([dir[, prefix]]) <br> Python3 中已删除。返回唯一的路径名用于创建临时文件。|
| 57	| os.tmpfile() <br> Python3 中已删除。返回一个打开的模式为(w+b)的文件对象 .这文件对象没有文件夹入口，没有文件描述符，将会自动删除。|
| 58	| os.tmpnam() <br>Python3 中已删除。为创建一个临时文件返回一个唯一的路径|
| 59	| os.ttyname(fd) <br> 返回一个字符串，它表示与文件描述符fd 关联的终端设备。如果fd 没有与终端设备关联，则引发一个异常。|
| 60	| os.unlink(path) <br>删除文件路径|
| 61	| os.utime(path, times) <br> 返回指定的path文件的访问和修改的时间。|
| 62	| os.walk(top[, topdown=True[, onerror=None[, followlinks=False]]]) <br> 输出在文件夹中的文件名通过在树中游走，向上或者向下。|
| 63	| os.write(fd, str) <br> 写入字符串到文件描述符 fd中. 返回实际写入的字符串长度|
| 64	| os.path 模块 <br> 获取文件的属性信息。|

文件实战用例：

1. os.access(path, mode)  检验权限模式

os.access() 方法使用当前的uid/gid尝试访问路径。大部分操作使用有效的 uid/gid, 因此运行环境可以在 suid/sgid 环境尝试。

```python
#!/usr/bin/python3

import os, sys

# 假定 /tmp/foo.txt 文件存在，并有读写权限

ret = os.access("/tmp/foo.txt", os.F_OK)
print ("F_OK - 返回值 %s"% ret)

ret = os.access("/tmp/foo.txt", os.R_OK)
print ("R_OK - 返回值 %s"% ret)

ret = os.access("/tmp/foo.txt", os.W_OK)
print ("W_OK - 返回值 %s"% ret)

ret = os.access("/tmp/foo.txt", os.X_OK)
print ("X_OK - 返回值 %s"% ret)
```
输出如下
```
F_OK - 返回值 True
R_OK - 返回值 True
W_OK - 返回值 True
X_OK - 返回值 False
```
2. os.chdir() 方法

os.chdir() 方法用于改变当前工作目录到指定的路径。
```python
#!/usr/bin/python3

import os, sys

path = "/tmp"

# 查看当前工作目录
retval = os.getcwd()
print ("当前工作目录为 %s" % retval)

# 修改当前工作目录
os.chdir( path )

# 查看修改后的工作目录
retval = os.getcwd()

print ("目录修改成功 %s" % retval)
```
输出如下
```
当前工作目录为 /www
目录修改成功 /tmp
```
3. os.chflags() 方法

os.chflags() 方法用于设置路径的标记为数字标记。多个标记可以使用 OR 来组合起来。

只支持在 Unix 下使用。
```python
#!/usr/bin/python3

import os,stat

path = "/tmp/foo.txt"

# 为文件设置标记，使得它不能被重命名和删除
flags = stat.SF_NOUNLINK
retval = os.chflags( path, flags )
print ("返回值: %s" % retval)
```
输出如下
```
返回值: None
```
4. os.chmod() 方法

os.chmod() 方法用于更改文件或目录的权限。

Unix 系统可用。
```python
#!/usr/bin/python3

import os, sys, stat

# 假定 /tmp/foo.txt 文件存在，设置文件可以通过用户组执行

os.chmod("/tmp/foo.txt", stat.S_IXGRP)

# 设置文件可以被其他用户写入
os.chmod("/tmp/foo.txt", stat.S_IWOTH)

print ("修改成功!!")
```
输出如下
```
修改成功!!
```
5. os.chown() 方法

用于更改文件所有者，如果不修改可以设置为 -1, 你需要超级用户权限来执行权限修改操作。

只支持在 Unix 下使用。
```python
#!/usr/bin/python3

import os, sys

# 假定 /tmp/foo.txt 文件存在.
# 设置所有者 ID 为 100
os.chown("/tmp/foo.txt", 100, -1)

print ("修改权限成功!!")
```
输出如下
```
修改权限成功!!
```
6. os.chroot() 方法

用于更改当前进程的根目录为指定的目录，使用该函数需要管理员权限。

在 unix 中有效。
```python
#!/usr/bin/python3

import os, sys

# 设置根目录为 /tmp

os.chroot("/tmp")

print ("修改根目录成功!!")
```
输出如下
```
修改根目录成功!!
```
7. os.close() 方法

用于关闭指定的文件描述符 fd。
```python
#!/usr/bin/python3

import os, sys

# 打开文件
fd = os.open( "foo.txt", os.O_RDWR|os.O_CREAT )

#  写入字符串
os.write(fd, "This is test")

# 关闭文件
os.close( fd )

print ("关闭文件成功!!")
```
输出如下
```
关闭文件成功!!
```

其他方法的代码参考runbook

## 文件读写

是Python代码调用电脑文件的主要功能，可以被用于读取，写入等电脑上的东西。（eg：音频，excel，邮件）

但是这里你可能要问：为什么我要用Python打开文件?我直接双击点开不是更方便么？

一般来说，当你要打开不多的文件时， 是没问题的。但是当你有一项工作，需要打开100个文档的数据，然后把他们合并成一个文件时，我想问问你是怎么做的？当你有数千个基站的数据需要合并的时候，而每一个基站又很头疼的占用了你的一个文件，这时候你还一个一个打开么？是不是打到地10个的时候就已经崩溃了，或者说电脑开始不运行了呢？这样，你是不是就会想到Python呢？直接用Python帮你把数据一次性的存入文件那是相当方便（班主任傲娇脸，还不快去学习？）这就是，Python帮我们从复杂的工作环境中解放出来。那我们今天就来学习关于文件的处理这块吧。file 处理无非就是读写，下面我为你总结一下，编程尽量用英语哦，因为很多很多的字母都是英文的缩写，所以你要熟悉一下我用英文写的代码。具体你自己学习一下：

- Open : r, a, w, x, t, b
- read : readline()
- close() :
- write : append, write, create
- delete : remove, osrmdir()

1. File处理

open() 方法

Python open() 方法用于打开一个文件，并返回文件对象，在对文件进行处理过程都需要使用到这个函数，如果该文件无法被打开，会抛出 OSError。

注意：使用 open() 方法一定要保证关闭文件对象，即调用 close() 方法。

open() 函数常用形式是接收两个参数：文件名(file)和模式(mode)。
```python
open(file, mode='r')
```
完整的语法格式为：（不会解析的看请你看参数说明）
```python
open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)
```
参数说明:

- file: 必需，文件路径（相对或者绝对路径）。
- mode: 可选，文件打开模式
- buffering: 设置缓冲
- encoding: 一般使用utf8
- errors: 报错级别
- newline: 区分换行符
- closefd: 传入的file参数类型
- opener:

mode 参数有：（我们一般用的是r, w.他们是read 和write 的缩写）

| 模式	| 描述 |
| --- | --- |
| t	| 文本模式 (默认)。|
| x	| 写模式，新建一个文件，如果该文件已存在则会报错。|
| b	| 二进制模式。|
| +	| 打开一个文件进行更新(可读可写)。|
| U	| 通用换行模式（不推荐）。|
| r	| 以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。|
| rb	| 以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是默认模式。一般用于非文本文件如图片等。|
| r+	| 打开一个文件用于读写。文件指针将会放在文件的开头。
| rb+	| 以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。一般用于非文本文件如图片等。|
| w	| 打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。|
| wb	| 以二进制格式打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。一般用于非文本文件如图片等。|
| w+	| 打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。|
| wb+	| 以二进制格式打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。一般用于非文本文件如图片等。
| a	| 打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。|
| ab	| 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。|
| a+	| 打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写。|
| ab+	| 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写。|

默认为文本模式，如果要以二进制模式打开，加上 b 。

2. file 对象

file 对象使用 open 函数来创建，下表列出了 file 对象常用的函数：

| 序号	| 方法及描述|
| --- | --- |
| 1	 | file.close() <br>关闭文件。关闭后文件不能再进行读写操作。 <br> 我在这里给你举个例子，我假设你已经有了这样一个文件，现在需要逐行阅读并关闭它：<br>f = open("demofile.txt", "r")<br>print(f.readline())<br>f.close() |
| 2	| file.flush()<br>刷新文件内部缓冲，直接把内部缓冲区的数据立刻写入文件, 而不是被动的等待输出缓冲区写入。|
| 3	| file.fileno() <br>返回一个整型的文件描述符(file descriptor FD 整型), 可以用在如os模块的read方法等一些底层操作上。|
| 4	| file.isatty() <br>如果文件连接到一个终端设备返回 True，否则返回 False。|
| 5	| file.next() <br>返回文件下一行。|
| 6	| file.read([size]) <br>从文件读取指定的字节数，如果未给定或为负则读取所有。|
| 7	| file.readline([size]) <br>读取整行，包括 "\n" 字符。|
| 8	| file.readlines([sizeint]) <br>读取所有行并返回列表，若给定sizeint>0，返回总和大约为sizeint字节的行, 实际读取值可能比 sizeint 较大, 因为需要填充缓冲区。|
| 9	| file.seek(offset[, whence]) <br>设置文件当前位置|
| 10	|file.tell() <br>返回文件当前位置。|
| 11	|file.truncate([size])<br>从文件的首行首字符开始截断，截断文件为 size 个字符，无 size 表示从当前位置截断；截断之后后面的所有字符被删除，其中 Widnows 系统下的换行代表2个字符大小。|
| 12	|file.write(str) <br>将字符串写入文件，返回的是写入的字符长度。我们看一下例子，你自己运行一下看看结果好不好？我在这里假设你已经有demofile2的文件了。<br>f = open("demofile2.txt", "a")<br>f.write("Now the file has more content!")<br>f.close()<br>#open and read the file after the appending:<br>f = open("demofile2.txt", "r")<br>print(f.read())<br>|
| 13	| file.writelines(sequence)<br> 向文件写入一个序列字符串列表，如果需要换行则要自己加入每行的换行符。|


## 正则表达式

正则表达式是一个特殊的字符序列，它能帮助你方便的检查一个字符串是否与某种模式匹配。

Python 自1.5版本起增加了re 模块，它提供 Perl 风格的正则表达式模式。

re 模块使 Python 语言拥有全部的正则表达式功能。

compile 函数根据一个模式字符串和可选的标志参数生成一个正则表达式对象。该对象拥有一系列方法用于正则表达式匹配和替换。

re 模块也提供了与这些方法功能完全一致的函数，这些函数使用一个模式字符串做为它们的第一个参数。

1. RE.MATCH函数

re.match 尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match()就返回none。

函数语法：
```python
re.match(pattern, string, flags=0)
```
函数参数说明：

| 参数	| 描述 |
| --- | --- |
| pattern	| 匹配的正则表达式|
| string	| 要匹配的字符串。|
| flags	| 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。可选标志|

匹配成功re.match方法返回一个匹配的对象，否则返回None。

我们可以使用group(num) 或 groups() 匹配对象函数来获取匹配表达式。

| 匹配对象方法	| 描述 |
| --- | --- |
| group(num=0)	| 匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。|
| groups()	| 返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。|

实例
```python
#!/usr/bin/python

import re
print(re.match('www', 'www.runoob.com').span())  # 在起始位置匹配
print(re.match('com', 'www.runoob.com'))         # 不在起始位置匹配
```

以上实例运行输出结果为：
```
(0, 3)
None
```
实例
```python
#!/usr/bin/python3
import re

line = "Cats are smarter than dogs"
# .* 表示任意匹配除换行符（\n、\r）之外的任何单个或多个字符
matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I)

if matchObj:
print ("matchObj.group() : ", matchObj.group())
print ("matchObj.group(1) : ", matchObj.group(1))
print ("matchObj.group(2) : ", matchObj.group(2))
else:
print ("No match!!")
```
以上实例执行结果如下：
```
matchObj.group() :  Cats are smarter than dogs
matchObj.group(1) :  Cats
matchObj.group(2) :  smarter
```
2. RE.SEARCH方法

re.search 扫描整个字符串并返回第一个成功的匹配。

函数语法：
```python
re.search(pattern, string, flags=0)
```
函数参数说明：

| 参数	| 描述 |
| --- | --- |
| pattern	| 匹配的正则表达式|
| string	| 要匹配的字符串。|
| flags	| 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。 可选标志|  

匹配成功re.search方法返回一个匹配的对象，否则返回None。

我们可以使用group(num) 或 groups() 匹配对象函数来获取匹配表达式。

| 匹配对象方法	| 描述 |
| --- | --- |
| group(num=0)	| 匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。|
| groups()	| 返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。|

实例
```python
#!/usr/bin/python3

import re

print(re.search('www', 'www.runoob.com').span())  # 在起始位置匹配
print(re.search('com', 'www.runoob.com').span())         # 不在起始位置匹配
```
以上实例运行输出结果为：
```
(0, 3)
(11, 14)
```
实例
```python
#!/usr/bin/python3

import re

line = "Cats are smarter than dogs";

searchObj = re.search( r'(.*) are (.*?) .*', line, re.M|re.I)

if searchObj:
  print ("searchObj.group() : ", searchObj.group())
  print ("searchObj.group(1) : ", searchObj.group(1))
  print ("searchObj.group(2) : ", searchObj.group(2))
else:
  print ("Nothing found!!")
```
以上实例执行结果如下：
```
searchObj.group() :  Cats are smarter than dogs
searchObj.group(1) :  Cats
searchObj.group(2) :  smarter
```

3. RE.MATCH与RE.SEARCH的区别

re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。

实例
```python
#!/usr/bin/python3

import re

line = "Cats are smarter than dogs";

matchObj = re.match( r'dogs', line, re.M|re.I)
if matchObj:
  print ("match --> matchObj.group() : ", matchObj.group())
else:
  print ("No match!!")

  matchObj = re.search( r'dogs', line, re.M|re.I)
  if matchObj:
    print ("search --> matchObj.group() : ", matchObj.group())
  else:
    print ("No match!!")
```
以上实例运行结果如下：
```
No match!!
search --> matchObj.group() :  dogs
```
4. 检索和替换

Python 的re模块提供了re.sub用于替换字符串中的匹配项。

语法：
```python
re.sub(pattern, repl, string, count=0)
```
参数：

- pattern : 正则中的模式字符串。
- repl : 替换的字符串，也可为一个函数。
- string : 要被查找替换的原始字符串。
- count : 模式匹配后替换的最大次数，默认 0 表示替换所有的匹配。

实例
```python
#!/usr/bin/python3
import re

phone = "2004-959-559 # 这是一个电话号码"

# 删除注释
num = re.sub(r'#.*$', "", phone)
print ("电话号码 : ", num)

# 移除非数字的内容
num = re.sub(r'\D', "", phone)
print ("电话号码 : ", num)
```
以上实例执行结果如下：
```
电话号码 :  2004-959-559
电话号码 :  2004959559
```
5. repl 参数是一个函数

以下实例中将字符串中的匹配的数字乘于 2：

实例
```python
#!/usr/bin/python

import re

# 将匹配的数字乘于 2
def double(matched):
  value = int(matched.group('value'))
  return str(value * 2)

  s = 'A23G4HFD567'
  print(re.sub('(?P<value>\d+)', double, s))
```
执行输出结果为：
```
A46G8HFD1134
```
6. compile 函数

compile 函数用于编译正则表达式，生成一个正则表达式（ Pattern ）对象，供 match() 和 search() 这两个函数使用。

语法格式为：
```python
re.compile(pattern[, flags])
```
参数：

- pattern : 一个字符串形式的正则表达式
- flags 可选，表示匹配模式，比如忽略大小写，多行模式等，具体参数为：
- re.I 忽略大小写
- re.L 表示特殊字符集 \w, \W, \b, \B, \s, \S 依赖于当前环境
- re.M 多行模式
- re.S 即为' . '并且包括换行符在内的任意字符（' . '不包括换行符）
- re.U 表示特殊字符集 \w, \W, \b, \B, \d, \D, \s, \S 依赖于 Unicode 字符属性数据库
- re.X 为了增加可读性，忽略空格和' # '后面的注释

实例
```python
>>>import re
>>> pattern = re.compile(r'\d+')                    # 用于匹配至少一个数字
>>> m = pattern.match('one12twothree34four')        # 查找头部，没有匹配
>>> print(m)
None
>>> m = pattern.match('one12twothree34four', 2, 10) # 从'e'的位置开始匹配，没有匹配
>>> print(m)
None
>>> m = pattern.match('one12twothree34four', 3, 10) # 从'1'的位置开始匹配，正好匹配
>>> print(m)                                         # 返回一个 Match 对象
<_sre.SRE_Match object at 0x10a42aac0>
>>> m.group(0)   # 可省略 0
'12'
>>> m.start(0)   # 可省略 0
3
>>> m.end(0)     # 可省略 0
5
>>> m.span(0)    # 可省略 0
(3, 5)
```

在上面，当匹配成功时返回一个 Match 对象，其中：

- group([group1, …]) 方法用于获得一个或多个分组匹配的字符串，当要获得整个匹配的子串时，可直接使用 group() 或 group(0)；
- start([group]) 方法用于获取分组匹配的子串在整个字符串中的起始位置（子串第一个字符的索引），参数默认值为 0；
- end([group]) 方法用于获取分组匹配的子串在整个字符串中的结束位置（子串最后一个字符的索引+1），参数默认值为 0；
- span([group]) 方法返回 (start(group), end(group))。

再看看一个例子：

```python
>>>import re
>>> pattern = re.compile(r'([a-z]+) ([a-z]+)', re.I)   # re.I 表示忽略大小写
>>> m = pattern.match('Hello World Wide Web')
>>> print(m)                               # 匹配成功，返回一个 Match 对象
<_sre.SRE_Match object at 0x10bea83e8>
>>> m.group(0)                            # 返回匹配成功的整个子串
'Hello World'
>>> m.span(0)                             # 返回匹配成功的整个子串的索引
(0, 11)
>>> m.group(1)                            # 返回第一个分组匹配成功的子串
'Hello'
>>> m.span(1)                             # 返回第一个分组匹配成功的子串的索引
(0, 5)
>>> m.group(2)                            # 返回第二个分组匹配成功的子串
'World'
>>> m.span(2)                             # 返回第二个分组匹配成功的子串
(6, 11)
>>> m.groups()                            # 等价于 (m.group(1), m.group(2), ...)
('Hello', 'World')
>>> m.group(3)                            # 不存在第三个分组
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
IndexError: no such group
```
7. findall

在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，如果没有找到匹配的，则返回空列表。

注意： match 和 search 是匹配一次 findall 匹配所有。

语法格式为：
```python
findall(string[, pos[, endpos]])
```
参数：

- string 待匹配的字符串。
- pos 可选参数，指定字符串的起始位置，默认为 0。
- endpos 可选参数，指定字符串的结束位置，默认为字符串的长度。

查找字符串中的所有数字：

```python
import re

pattern = re.compile(r'\d+')   # 查找数字
result1 = pattern.findall('runoob 123 google 456')
result2 = pattern.findall('run88oob123google456', 0, 10)

print(result1)
print(result2)
```
输出结果：
```
['123', '456']
['88', '12']
```
8. re.finditer

和 findall 类似，在字符串中找到正则表达式所匹配的所有子串，并把它们作为一个迭代器返回。
```python
re.finditer(pattern, string, flags=0)
```

参数：

| 参数	| 描述 |
| --- | --- |
| pattern	| 匹配的正则表达式|
| string	| 要匹配的字符串。|
| flags	| 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。可选标志 |

实例
```python
import re

it = re.finditer(r"\d+","12a32bc43jf3")
for match in it:
  print (match.group() )
```
输出结果：
```
12
32
43
3
```

9. re.split

split 方法按照能够匹配的子串将字符串分割后返回列表，它的使用形式如下：
```python
re.split(pattern, string[, maxsplit=0, flags=0])
```
参数：

| 参数	| 描述 |
| --- | --- |
| pattern	| 匹配的正则表达式 |
| string	| 要匹配的字符串。 |
| maxsplit	| 分隔次数，maxsplit=1 分隔一次，默认为 0，不限制次数。 |
| flags	| 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。可选标志 |

实例
```python
>>>import re
>>> re.split('\W+', 'runoob, runoob, runoob.')
['runoob', 'runoob', 'runoob', '']
>>> re.split('(\W+)', ' runoob, runoob, runoob.')
['', ' ', 'runoob', ', ', 'runoob', ', ', 'runoob', '.', '']
>>> re.split('\W+', ' runoob, runoob, runoob.', 1)
['', 'runoob, runoob, runoob.']

>>> re.split('a*', 'hello world')   # 对于一个找不到匹配的字符串而言，split 不会对其作出分割
['hello world']
```
10. 正则表达式对象
- re.RegexObject<br>
re.compile() 返回 RegexObject 对象。
- re.MatchObject<br>
group() 返回被 RE 匹配的字符串。
  - start() 返回匹配开始的位置
  - end() 返回匹配结束的位置
  - span() 返回一个元组包含匹配 (开始,结束) 的位置

11. 正则表达式修饰符 - 可选标志

正则表达式可以包含一些可选标志修饰符来控制匹配的模式。修饰符被指定为一个可选的标志。多个标志可以通过按位 OR(|) 它们来指定。如 re.I | re.M 被设置成 I 和 M 标志：

| 修饰符	| 描述 |
| --- | --- |
| re.I	| 使匹配对大小写不敏感 |
| re.L	| 做本地化识别（locale-aware）匹配 |
| re.M	| 多行匹配，影响 ^ 和 $ |
| re.S	| 使 . 匹配包括换行在内的所有字符 |
| re.U	| 根据Unicode字符集解析字符。这个标志影响 \w, \W, \b, \B.|
| re.X	| 该标志通过给予你更灵活的格式以便你将正则表达式写得更易于理解。|

12. 正则表达式模式

模式字符串使用特殊的语法来表示一个正则表达式：

字母和数字表示他们自身。一个正则表达式模式中的字母和数字匹配同样的字符串。

多数字母和数字前加一个反斜杠时会拥有不同的含义。

标点符号只有被转义时才匹配自身，否则它们表示特殊的含义。

反斜杠本身需要使用反斜杠转义。

由于正则表达式通常都包含反斜杠，所以你最好使用原始字符串来表示它们。模式元素(如 r'\t'，等价于 \\t )匹配相应的特殊字符。

下表列出了正则表达式模式语法中的特殊元素。如果你使用模式的同时提供了可选的标志参数，某些模式元素的含义会改变。

| 模式	| 描述 |
| --- | --- |
| ^ | 匹配字符串的开头 |
| $	| 匹配字符串的末尾。|
| .	| 匹配任意字符，除了换行符，当re.DOTALL标记被指定时，则可以匹配包括换行符的任意字符。|
| [...]	| 用来表示一组字符,单独列出：[amk] 匹配 'a'，'m'或'k'|
| [^...]	| 不在[]中的字符：[^abc] 匹配除了a,b,c之外的字符。|
| re*	| 匹配0个或多个的表达式。|
| re+	| 匹配1个或多个的表达式。|
| re?	| 匹配0个或1个由前面的正则表达式定义的片段，非贪婪方式 |
| re{ n}	| 匹配n个前面表达式。例如，"o{2}"不能匹配"Bob"中的"o"，但是能匹配"food"中的两个o。|
| re{ n,}	| 精确匹配n个前面表达式。例如，"o{2,}"不能匹配"Bob"中的"o"，但能匹配"foooood"中的所有o。"o{1,}"等价于"o+"。"o{0,}"则等价于"o*"。|
| re{ n, m}	| 匹配 n 到 m 次由前面的正则表达式定义的片段，贪婪方式|
| a| b	| 匹配a或b|
| (re)	| 匹配括号内的表达式，也表示一个组|
| (?imx)	| 正则表达式包含三种可选标志：i, m, 或 x 。只影响括号中的区域。|
| (?-imx)	| 正则表达式关闭 i, m, 或 x 可选标志。只影响括号中的区域。|
| (?: re)	| 类似 (...), 但是不表示一个组
| (?imx: re)	| 在括号中使用i, m, 或 x 可选标志
| (?-imx: re)	| 在括号中不使用i, m, 或 x 可选标志
| (?#...)	| 注释.| |
| (?= re)	| 前向肯定界定符。如果所含正则表达式，以 ... 表示，在当前位置成功匹配时成功，否则失败。但一旦所含表达式已经尝试，匹配引擎根本没有提高；模式的剩余部分还要尝试界定符的右边。|
| (?! re)	| 前向否定界定符。与肯定界定符相反；当所含表达式不能在字符串当前位置匹配时成功。|
| (?> re)	| 匹配的独立模式，省去回溯。|
| \w	| 匹配数字字母下划线|
| \W	| 匹配非数字字母下划线|
| \s	| 匹配任意空白字符，等价于 [\t\n\r\f]。|
| \S	| 匹配任意非空字符|
| \d	| 匹配任意数字，等价于 [0-9]。|
| \D	| 匹配任意非数字|
| \A	| 匹配字符串开始|
| \Z	| 匹配字符串结束，如果是存在换行，只匹配到换行前的结束字符串。|
| \z	| 匹配字符串结束|
| \G	| 匹配最后匹配完成的位置。|
| \b	| 匹配一个单词边界，也就是指单词和空格间的位置。例如， 'er\b' 可以匹配"never" 中的 'er'，但不能匹配 "verb" 中的 'er'。|
| \B	| 匹配非单词边界。'er\B' 能匹配 "verb" 中的 'er'，但不能匹配 "never" 中的 'er'。|
| \n, \t, | 等。	匹配一个换行符。匹配一个制表符, 等
| \1...\9	| 匹配第n个分组的内容。|
| \10	| 匹配第n个分组的内容，如果它经匹配。否则指的是八进制字符码的表达式。|

12. 正则表达式实例

字符匹配

| 实例	| 描述 |
| --- | --- |
| python	| 匹配 "python".|

字符类

| 实例	| 描述 |
| --- | --- |
[Pp]ython	| 匹配 "Python" 或 "python"|
| rub[ye]	| 匹配 "ruby" 或 "rube"|
| [aeiou]	| 匹配中括号内的任意一个字母|
| [0-9]	| 匹配任何数字。类似于 [0123456789]|
| [a-z]	| 匹配任何小写字母|
| [A-Z]	| 匹配任何大写字母|
| [a-zA-Z0-9]	| 匹配任何字母及数字|
| [^aeiou]	| 除了aeiou字母以外的所有字符|
| [^0-9]	| 匹配除了数字外的字符|

13. 特殊字符类

| 实例	| 描述 |
| --- | --- |
| .	| 匹配除 "\n" 之外的任何单个字符。要匹配包括 '\n' 在内的任何字符，请使用象 '[.\n]' 的模式。|
| \d	| 匹配一个数字字符。等价于 [0-9]。|
| \D	| 匹配一个非数字字符。等价于 [^0-9]。|
| \s	| 匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。|
| \S	| 匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。|
| \w	| 匹配包括下划线的任何单词字符。等价于'[A-Za-z0-9_]'。|
| \W	| 匹配任何非单词字符。等价于 '[^A-Za-z0-9_]'。|

## XML 解析

1. 什么是 XML？

XML 指可扩展标记语言（eXtensible Markup Language），标准通用标记语言的子集，是一种用于标记电子文件使其具有结构性的标记语言。 你可以通过本站学习 XML 教程

XML 被设计用来传输和存储数据。

XML 是一套定义语义标记的规则，这些标记将文档分成许多部件并对这些部件加以标识。

它也是元标记语言，即定义了用于定义其他与特定领域有关的、语义的、结构化的标记语言的句法语言。

2. PYTHON 对 XML 的解析

常见的 XML 编程接口有 DOM 和 SAX，这两种接口处理 XML 文件的方式不同，当然使用场合也不同。

Python 有三种方法解析 XML，SAX，DOM，以及 ElementTree:

- SAX (simple API for XML ) <br>
Python 标准库包含 SAX 解析器，SAX 用事件驱动模型，通过在解析 XML 的过程中触发一个个的事件并调用用户定义的回调函数来处理 XML 文件。
- DOM(Document Object Model) <br>
将 XML 数据在内存中解析成一个树，通过对树的操作来操作 XML。

本章节使用到的 XML 实例文件 movies.xml 内容如下：
```xml
<collection shelf="New Arrivals">
  <movie title="Enemy Behind">
    <type>War, Thriller</type>
    <format>DVD</format>
    <year>2003</year>
    <rating>PG</rating>
    <stars>10</stars>
    <description>Talk about a US-Japan war</description>
  </movie>
  <movie title="Transformers">
    <type>Anime, Science Fiction</type>
    <format>DVD</format>
    <year>1989</year>
    <rating>R</rating>
    <stars>8</stars>
    <description>A schientific fiction</description>
  </movie>
  <movie title="Trigun">
    <type>Anime, Action</type>
    <format>DVD</format>
    <episodes>4</episodes>
    <rating>PG</rating>
    <stars>10</stars>
    <description>Vash the Stampede!</description>
  </movie>
  <movie title="Ishtar">
    <type>Comedy</type>
    <format>VHS</format>
    <rating>PG</rating>
    <stars>2</stars>
    <description>Viewable boredom</description>
  </movie>
</collection>
```
2. PYTHON 使用 SAX 解析 XML

SAX 是一种基于事件驱动的API。

利用 SAX 解析 XML 文档牵涉到两个部分: 解析器和事件处理器。

解析器负责读取 XML 文档，并向事件处理器发送事件，如元素开始跟元素结束事件。

而事件处理器则负责对事件作出响应，对传递的 XML 数据进行处理。

- 1、对大型文件进行处理；
- 2、只需要文件的部分内容，或者只需从文件中得到特定信息。
- 3、想建立自己的对象模型的时候。
在 Python 中使用 sax 方式处理 xml 要先引入 xml.sax 中的 parse 函数，还有 xml.sax.handler 中的 ContentHandler。

**ContentHandler 类方法介绍**

characters(content) 方法

调用时机：

从行开始，遇到标签之前，存在字符，content 的值为这些字符串。

从一个标签，遇到下一个标签之前， 存在字符，content 的值为这些字符串。

从一个标签，遇到行结束符之前，存在字符，content 的值为这些字符串。

标签可以是开始标签，也可以是结束标签。

startDocument() 方法

文档启动的时候调用。

endDocument() 方法

解析器到达文档结尾时调用。

startElement(name, attrs) 方法

遇到XML开始标签时调用，name 是标签的名字，attrs 是标签的属性值字典。

endElement(name) 方法

遇到XML结束标签时调用。

MAKE_PARSER 方法

以下方法创建一个新的解析器对象并返回。
```
xml.sax.make_parser( [parser_list] )
```
参数说明:

parser_list - 可选参数，解析器列表

PARSER 方法

以下方法创建一个 SAX 解析器并解析xml文档：
```
xml.sax.parse( xmlfile, contenthandler[, errorhandler])
```
参数说明:

- xmlfile - xml文件名
- contenthandler - 必须是一个 ContentHandler 的对象
- errorhandler - 如果指定该参数，errorhandler 必须是一个 SAX ErrorHandler 对象

PARSESTRING 方法

parseString 方法创建一个 XML 解析器并解析 xml 字符串：
```
xml.sax.parseString(xmlstring, contenthandler[, errorhandler])
```
参数说明:

- xmlstring - xml字符串
- contenthandler - 必须是一个 ContentHandler 的对象
- errorhandler - 如果指定该参数，errorhandler 必须是一个 SAX ErrorHandler对象

PYTHON 解析XML实例
```python
#!/usr/bin/python3

import xml.sax

class MovieHandler( xml.sax.ContentHandler ):
  def __init__(self):
    self.CurrentData = ""
    self.type = ""
    self.format = ""
    self.year = ""
    self.rating = ""
    self.stars = ""
    self.description = ""

  # 元素开始调用
  def startElement(self, tag, attributes):
    self.CurrentData = tag
    if tag == "movie":
      print ("*****Movie*****")
      title = attributes["title"]
      print ("Title:", title)

  # 元素结束调用
  def endElement(self, tag):
    if self.CurrentData == "type":
      print ("Type:", self.type)
    elif self.CurrentData == "format":
      print ("Format:", self.format)
    elif self.CurrentData == "year":
      print ("Year:", self.year)
    elif self.CurrentData == "rating":
      print ("Rating:", self.rating)
    elif self.CurrentData == "stars":
      print ("Stars:", self.stars)
    elif self.CurrentData == "description":
      print ("Description:", self.description)
      self.CurrentData = ""

  # 读取字符时调用
  def characters(self, content):
    if self.CurrentData == "type":
      self.type = content
    elif self.CurrentData == "format":
      self.format = content
    elif self.CurrentData == "year":
      self.year = content
    elif self.CurrentData == "rating":
      self.rating = content
    elif self.CurrentData == "stars":
      self.stars = content
    elif self.CurrentData == "description":
      self.description = content

if ( __name__ == "__main__"):

  # 创建一个 XMLReader
  parser = xml.sax.make_parser()
  # 关闭命名空间
  parser.setFeature(xml.sax.handler.feature_namespaces, 0)

  # 重写 ContextHandler
  Handler = MovieHandler()
  parser.setContentHandler( Handler )

  parser.parse("movies.xml")
```
以上代码执行结果如下：
```
*****Movie*****
Title: Enemy Behind
Type: War, Thriller
Format: DVD
Year: 2003
Rating: PG
Stars: 10
Description: Talk about a US-Japan war
*****Movie*****
Title: Transformers
Type: Anime, Science Fiction
Format: DVD
Year: 1989
Rating: R
Stars: 8
Description: A schientific fiction
*****Movie*****
Title: Trigun
Type: Anime, Action
Format: DVD
Rating: PG
Stars: 10
Description: Vash the Stampede!
*****Movie*****
Title: Ishtar
Type: Comedy
Format: VHS
Rating: PG
Stars: 2
Description: Viewable boredom
```
完整的 SAX API 文档请查阅[Python SAX APIs](http://docs.python.org/library/xml.sax.html)

3. 使用XML.DOM解析XML

文件对象模型（Document Object Model，简称DOM），是W3C组织推荐的处理可扩展置标语言的标准编程接口。

一个 DOM 的解析器在解析一个 XML 文档时，一次性读取整个文档，把文档中所有元素保存在内存中的一个树结构里，之后你可以利用DOM 提供的不同的函数来读取或修改文档的内容和结构，也可以把修改过的内容写入xml文件。

python中用xml.dom.minidom来解析xml文件，实例如下：
```python
#!/usr/bin/python3

from xml.dom.minidom import parse
import xml.dom.minidom

# 使用minidom解析器打开 XML 文档
DOMTree = xml.dom.minidom.parse("movies.xml")
collection = DOMTree.documentElement
if collection.hasAttribute("shelf"):
print ("Root element : %s" % collection.getAttribute("shelf"))

# 在集合中获取所有电影
movies = collection.getElementsByTagName("movie")

# 打印每部电影的详细信息
for movie in movies:
print ("*****Movie*****")
if movie.hasAttribute("title"):
print ("Title: %s" % movie.getAttribute("title"))

type = movie.getElementsByTagName('type')[0]
print ("Type: %s" % type.childNodes[0].data)
format = movie.getElementsByTagName('format')[0]
print ("Format: %s" % format.childNodes[0].data)
rating = movie.getElementsByTagName('rating')[0]
print ("Rating: %s" % rating.childNodes[0].data)
description = movie.getElementsByTagName('description')[0]
print ("Description: %s" % description.childNodes[0].data)
```
以上程序执行结果如下：
```
Root element : New Arrivals
*****Movie*****
Title: Enemy Behind
Type: War, Thriller
Format: DVD
Rating: PG
Description: Talk about a US-Japan war
*****Movie*****
Title: Transformers
Type: Anime, Science Fiction
Format: DVD
Rating: R
Description: A schientific fiction
*****Movie*****
Title: Trigun
Type: Anime, Action
Format: DVD
Rating: PG
Description: Vash the Stampede!
*****Movie*****
Title: Ishtar
Type: Comedy
Format: VHS
Rating: PG
Description: Viewable boredom
```
完整的 DOM API 文档请查阅[Python DOM APIs](http://docs.python.org/library/xml.dom.html)。

## JSON 数据解析

JSON (JavaScript Object Notation) 是一种轻量级的数据交换格式。它基于ECMAScript的一个子集。

Python3 中可以使用 json 模块来对 JSON 数据进行编解码，它包含了两个函数：

- json.dumps(): 对数据进行编码。
- json.loads(): 对数据进行解码。

在json的编解码过程中，python 的原始类型与json类型会相互转换，具体的转化对照如下：

1. Python 编码为 JSON 类型转换对应表：

| Python	| JSON |
| --- | --- |
| dict	| object|
| list, tuple	| array|
| str	| string|
| int, float, int- & float-derived Enums	| number|
| True	| true|
| False	| false|
| None	| null|

2. JSON 解码为 Python 类型转换对应表：

| JSON	| Python |
| --- | ---|
| object	| dict|
| array	| list|
| string	| str|
| number (int)	| int|
| number (real)	| float|
| true	| True|
| false	| False|
| null	| None|

3. json.dumps 与 json.loads 实例
以下实例演示了 Python 数据结构转换为JSON：

实例(PYTHON 3.0+)
```python
#!/usr/bin/python3

import json

# Python 字典类型转换为 JSON 对象
data = {
'no' : 1,
'name' : 'Runoob',
'url' : 'http://www.runoob.com'
}

json_str = json.dumps(data)
print ("Python 原始数据：", repr(data))
print ("JSON 对象：", json_str)
```
执行以上代码输出结果为：
```
Python 原始数据： {'url': 'http://www.runoob.com', 'no': 1, 'name': 'Runoob'}
JSON 对象： {"url": "http://www.runoob.com", "no": 1, "name": "Runoob"}
```
通过输出的结果可以看出，简单类型通过编码后跟其原始的repr()输出结果非常相似。

接着以上实例，我们可以将一个JSON编码的字符串转换回一个Python数据结构：

```python
#!/usr/bin/python3

import json

# Python 字典类型转换为 JSON 对象
data1 = {
'no' : 1,
'name' : 'Runoob',
'url' : 'http://www.runoob.com'
}

json_str = json.dumps(data1)
print ("Python 原始数据：", repr(data1))
print ("JSON 对象：", json_str)

# 将 JSON 对象转换为 Python 字典
data2 = json.loads(json_str)
print ("data2['name']: ", data2['name'])
print ("data2['url']: ", data2['url'])
```
执行以上代码输出结果为：
```
Python 原始数据： {'name': 'Runoob', 'no': 1, 'url': 'http://www.runoob.com'}
JSON 对象： {"name": "Runoob", "no": 1, "url": "http://www.runoob.com"}
data2['name']:  Runoob
data2['url']:  http://www.runoob.com
```
如果你要处理的是文件而不是字符串，你可以使用 json.dump() 和 json.load() 来编码和解码JSON数据。例如：

```python
# 写入 JSON 数据
with open('data.json', 'w') as f:
  json.dump(data, f)

  # 读取数据
  with open('data.json', 'r') as f:
    data = json.load(f)
```
更多资料请参考：https://docs.python.org/3/library/json.html

## 日期和时间 书签

Python 程序能用很多方式处理日期和时间，转换日期格式是一个常见的功能。

Python 提供了一个 time 和 calendar 模块可以用于格式化日期和时间。

时间间隔是以秒为单位的浮点小数。

每个时间戳都以自从1970年1月1日午夜（历元）经过了多长时间来表示。

Python 的 time 模块下有很多函数可以转换常见日期格式。如函数time.time()用于获取当前时间戳, 如下实例:
```python
#!/usr/bin/python3

import time;  # 引入time模块

ticks = time.time()
print ("当前时间戳为:", ticks)
```
以上实例输出结果：
```
当前时间戳为: 1459996086.7115328
```
时间戳单位最适于做日期运算。但是1970年之前的日期就无法以此表示了。太遥远的日期也不行，UNIX和Windows只支持到2038年。


1. 什么是时间元组？

很多Python函数用一个元组装起来的9组数字处理时间:

| 序号	| 字段	| 值 |
| --- | --- | --- |
| 0	| 4位数年	| 2008 |
| 1	| 月	| 1 到 12 |
| 2	| 日	| 1到31 |
| 3	| 小时 | 0到23 |
| 4	| 分钟	| 0到59 |
| 5	| 秒	| 0到61 (60或61 是闰秒) |
| 6	| 一周的第几日	| 0到6 (0是周一) |
| 7	| 一年的第几日	| 1到366 (儒略历) |
| 8	| 夏令时	| -1, 0, 1, -1是决定是否为夏令时的旗帜|

上述也就是struct_time元组。这种结构具有如下属性：

| 序号	| 属性	| 值 |
| --- | --- | --- |
| 0	| tm_year	| 2008 |
| 1	| tm_mon	| 1 到 12|
| 2	| tm_mday	| 1 到 31|
| 3	| tm_hour	| 0 到 23|
| 4	| tm_min	| 0 到 59|
| 5	| tm_sec	| 0 到 61 (60或61 是闰秒)|
| 6	| tm_wday	| 0到6 (0是周一)|
| 7	| tm_yday	| 一年中的第几天，1 到 366|
| 8	| tm_isdst	| 是否为夏令时，值有：1(夏令时)、0(不是夏令时)、-1(未知)，默认 -1|

2. 获取当前时间

从返回浮点数的时间戳方式向时间元组转换，只要将浮点数传递给如localtime之类的函数。
```python
#!/usr/bin/python3

import time

localtime = time.localtime(time.time())
print ("本地时间为 :", localtime)
```
以上实例输出结果：
```
本地时间为 : time.struct_time(tm_year=2016, tm_mon=4, tm_mday=7, tm_hour=10, tm_min=28, tm_sec=49, tm_wday=3, tm_yday=98, tm_isdst=0)
```

3. 获取格式化的时间

你可以根据需求选取各种格式，但是最简单的获取可读的时间模式的函数是asctime():
```python
#!/usr/bin/python3

import time

localtime = time.asctime( time.localtime(time.time()) )
print ("本地时间为 :", localtime)
```
以上实例输出结果：
```
本地时间为 : Thu Apr  7 10:29:13 2016
```
4. 格式化日期

我们可以使用 time 模块的 strftime 方法来格式化日期，：
```
time.strftime(format[, t])
```
```python
#!/usr/bin/python3

import time

# 格式化成2016-03-20 11:45:39形式
print (time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()))

# 格式化成Sat Mar 28 22:24:24 2016形式
print (time.strftime("%a %b %d %H:%M:%S %Y", time.localtime()))

# 将格式字符串转换为时间戳
a = "Sat Mar 28 22:24:24 2016"
print (time.mktime(time.strptime(a,"%a %b %d %H:%M:%S %Y")))
```
以上实例输出结果：
```
2016-04-07 10:29:46
Thu Apr 07 10:29:46 2016
1459175064.0
```
python中时间日期格式化符号：

- %y 两位数的年份表示（00-99）
- %Y 四位数的年份表示（000-9999）
- %m 月份（01-12）
- %d 月内中的一天（0-31）
- %H 24小时制小时数（0-23）
- %I 12小时制小时数（01-12）
- %M 分钟数（00=59）
- %S 秒（00-59）
- %a 本地简化星期名称
- %A 本地完整星期名称
- %b 本地简化的月份名称
- %B 本地完整的月份名称
- %c 本地相应的日期表示和时间表示
- %j 年内的一天（001-366）
- %p 本地A.M.或P.M.的等价符
- %U 一年中的星期数（00-53）星期天为星期的开始
- %w 星期（0-6），星期天为星期的开始
- %W 一年中的星期数（00-53）星期一为星期的开始
- %x 本地相应的日期表示
- %X 本地相应的时间表示
- %Z 当前时区的名称
- %% %号本身

5. 获取某月日历

Calendar模块有很广泛的方法用来处理年历和月历，例如打印某月的月历：
```python
#!/usr/bin/python3

import calendar

cal = calendar.month(2016, 1)
print ("以下输出2016年1月份的日历:")
print (cal)
```
以上实例输出结果：
```
以下输出2016年1月份的日历:
January 2016
Mo Tu We Th Fr Sa Su
            1  2  3
4  5  6  7  8  9 10
11 12 13 14 15 16 17
18 19 20 21 22 23 24
25 26 27 28 29 30 31
```

6. TIME 模块

Time 模块包含了以下内置函数，既有时间处理的，也有转换时间格式的：

| 序号	| 函数及描述	| 实例 |
| --- | --- | --- |
| 1	| time.altzone <br>返回格林威治西部的夏令时地区的偏移秒数。如果该地区在格林威治东部会返回负值（如西欧，包括英国）。对夏令时启用地区才能使用。| 以下实例展示了 altzone()函数的使用方法：<br>>>> import time<br>>>> print ("time.altzone %d " % time.altzone)<br>time.altzone -28800 |
| 2	| time.asctime([tupletime]) <br>接受时间元组并返回一个可读的形式为"Tue Dec 11 18:07:14 2008"（2008年12月11日 周二18时07分14秒）的24个字符的字符串。| 以下实例展示了 asctime()函数的使用方法：<br><br>>>> import time<br>>>> t = time.localtime()<br>>>> print ("time.asctime(t): %s " % time.asctime(t))<br>time.asctime(t): Thu Apr  7 10:36:20 2016 |
| 3	| time.clock()<br>用以浮点数计算的秒数返回当前的CPU时间。用来衡量不同程序的耗时，比time.time()更有用。	| 由于该方法依赖操作系统，在 Python 3.3 以后不被推荐，而在 3.8 版本中被移除，需使用下列两个函数替代。<br>time.perf_counter()  # 返回系统运行时间<br>time.process_time()  # 返回进程运行时间|
| 4	| time.ctime([secs])<br><br>作用相当于asctime(localtime(secs))，未给参数相当于asctime()	| <br>以下实例展示了 ctime()函数的使用方法：<br><br>>>> import time<br>>>> print ("time.ctime() : %s" % time.ctime())<br>time.ctime() : Thu Apr  7 10:51:58 2016|
| 5	| time.gmtime([secs])<br>接收时间戳（1970纪元后经过的浮点秒数）并返回格林威治天文时间下的时间元组t。注：t.tm_isdst始终为0	| 以下实例展示了 gmtime()函数的使用方法：<br><br>>>> import time<br>>>> print ("gmtime :", time.gmtime(1455508609.34375))<br>gmtime : time.struct_time(tm_year=2016, tm_mon=2, tm_mday=15, tm_hour=3, tm_min=56, tm_sec=49, tm_wday=0, tm_yday=46, tm_isdst=0)|
| 6	| time.localtime([secs] <br>接收时间戳（1970纪元后经过的浮点秒数）并返回当地时间下的时间元组t（t.tm_isdst可取0或1，取决于当地当时是不是夏令时）。	| 以下实例展示了 localtime()函数的使用方法：<br><br>>>> import time<br>>>> print ("localtime(): ", time.localtime(1455508609.34375))<br>localtime():  time.struct_time(tm_year=2016, tm_mon=2, tm_mday=15, tm_hour=11, tm_min=56, tm_sec=49, tm_wday=0, tm_yday=46, tm_isdst=0)|
| 7	| time.mktime(tupletime) <br>接受时间元组并返回时间戳（1970纪元后经过的浮点秒数）。	| 实例 |
| 8	| time.sleep(secs) <br>推迟调用线程的运行，secs指秒数。	|以下实例展示了 sleep()函数的使用方法：<br><br>#!/usr/bin/python3<br>import time<br><br>print ("Start : %s" % time.ctime())<br>time.sleep( 5 )<br>print ("End : %s" % time.ctime()) |
| 9	| time.strftime(fmt[,tupletime])<br>接收以时间元组，并返回以可读字符串表示的当地时间，格式由fmt决定。	| 以下实例展示了 strftime()函数的使用方法：<br><br>>>> import time<br>>>> print (time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()))<br>2016-04-07 11:18:05 |
| 10	| time.strptime(str,fmt='%a %b %d %H:%M:%S %Y')<br>根据fmt的格式把一个时间字符串解析为时间元组。	| 以下实例展示了 strptime()函数的使用方法：<br><br>>>> import time<br>>>> struct_time = time.strptime("30 Nov 00", "%d %b %y")<br>>>> print ("返回元组: ", struct_time)<br>返回元组:  time.struct_time(tm_year=2000, tm_mon=11, tm_mday=30, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=3, tm_yday=335, tm_isdst=-1) |
| 11	| time.time( )<br>返回当前时间的时间戳（1970纪元后经过的浮点秒数）。	 | 以下实例展示了 time()函数的使用方法：<br><br>>>> import time<br>>>> print(time.time())<br>1459999336.1963577|
| 12	| time.tzset()<br>根据环境变量TZ重新初始化时间相关设置。 | 实例 |
| 13	| time.perf_counter()<br>返回计时器的精准时间（系统的运行时间），包含整个系统的睡眠时间。由于返回值的基准点是未定义的，所以，只有连续调用的结果之间的差才是有效的。	| 实例 |
| 14	| time.process_time()<br>返回当前进程执行 CPU 的时间总和，不包含睡眠时间。由于返回值的基准点是未定义的，所以，只有连续调用的结果之间的差才是有效的。| |

Time模块包含了以下2个非常重要的属性：

| 序号	| 属性及描述 |
| --- | --- |
| 1	| time.timezone <br>属性time.timezone是当地时区（未启动夏令时）距离格林威治的偏移秒数（>0，美洲;<=0大部分欧洲，亚洲，非洲）。|
| 2	| time.tzname <br> 属性time.tzname包含一对根据情况的不同而不同的字符串，分别是带夏令时的本地时区名称，和不带的。|

7. 日历（CALENDAR）模块

此模块的函数都是日历相关的，例如打印某月的字符月历。

星期一是默认的每周第一天，星期天是默认的最后一天。更改设置需调用calendar.setfirstweekday()函数。模块包含了以下内置函数：

| 序号	| 函数及描述|
| --- | ---|
| 1	| calendar.calendar(year,w=2,l=1,c=6)<br>返回一个多行字符串格式的year年年历，3个月一行，间隔距离为c。 每日宽度间隔为w字符。每行长度为21* W+18+2* C。l是每星期行数。|
| 2	| calendar.firstweekday( )<br>返回当前每周起始日期的设置。默认情况下，首次载入caendar模块时返回0，即星期一。|
| 3	| calendar.isleap(year)<br>是闰年返回 True，否则为 false。<br>>>> import calendar<br>>>> print(calendar.isleap(2000))<br>True<br>>>> print(calendar.isleap(1900))<br>False | 
| 4	| calendar.leapdays(y1,y2)<br>返回在Y1，Y2两年之间的闰年总数。| 
| 5	| calendar.month(year,month,w=2,l=1)<br>返回一个多行字符串格式的year年month月日历，两行标题，一周一行。每日宽度间隔为w字符。每行的长度为7* w+6。l是每星期的行数。| 
| 6	| calendar.monthcalendar(year,month)<br>返回一个整数的单层嵌套列表。每个子列表装载代表一个星期的整数。Year年month月外的日期都设为0;范围内的日子都由该月第几日表示，从1开始。| 
| 7	| calendar.monthrange(year,month)<br>返回两个整数。第一个是该月的星期几，第二个是该月有几天。星期几是从0（星期一）到 6（星期日）。| <br><br>>>> import calendar<br>>>> calendar.monthrange(2014, 11)<br>(5, 30)<br><br>(5, 30)解释：5 表示 2014 年 11 月份的第一天是周六，30 表示 2014 年 11 月份总共有 30 天。| 
| 8	| calendar.prcal(year,w=2,l=1,c=6) <br>相当于 print calendar.calendar(year,w,l,c).| 
| 9	| calendar.prmonth(year,month,w=2,l=1)<br>相当于 print calendar.calendar（year，w，l，c）。| 
| 10 | 	calendar.setfirstweekday(weekday)<br>设置每周的起始日期码。0（星期一）到6（星期日）。| 
| 11	| calendar.timegm(tupletime) <br>和time.gmtime相反：接受一个时间元组形式，返回该时刻的时间戳（1970纪元后经过的浮点秒数）。| 
| 12	| calendar.weekday(year,month,day)<br> 返回给定日期的日期码。0（星期一）到6（星期日）。月份为 1（一月） 到 12（12月）。| 

8. 其他相关模块和函数
在Python中，其他处理日期和时间的模块还有：

- [time 模块](https://docs.python.org/3/library/time.html)
- [datetime模块](https://docs.python.org/3/library/datetime.html)

## 标准库概览

1. 操作系统接口

os模块提供了不少与操作系统相关联的函数。
```python
>>> import os
>>> os.getcwd()      # 返回当前的工作目录
'C:\\Python34'
>>> os.chdir('/server/accesslogs')   # 修改当前的工作目录
>>> os.system('mkdir today')   # 执行系统命令 mkdir 
0
```
建议使用 "import os" 风格而非 "from os import \*"。这样可以保证随操作系统不同而有所变化的 os.open() 不会覆盖内置函数 open()。

在使用 os 这样的大型模块时内置的 dir() 和 help() 函数非常有用:
```python
>>> import os
>>> dir(os)
<returns a list of all module functions>
>>> help(os)
<returns an extensive manual page created from the module's docstrings>
```
针对日常的文件和目录管理任务，:mod:shutil 模块提供了一个易于使用的高级接口:
```python
>>> import shutil
>>> shutil.copyfile('data.db', 'archive.db')
>>> shutil.move('/build/executables', 'installdir')
```
2. 文件通配符

glob模块提供了一个函数用于从目录通配符搜索中生成文件列表:
```python
>>> import glob
>>> glob.glob('*.py')
['primes.py', 'random.py', 'quote.py']
```
2. 命令行参数

通用工具脚本经常调用命令行参数。这些命令行参数以链表形式存储于 sys 模块的 argv 变量。例如在命令行中执行 "python demo.py one two three" 后可以得到以下输出结果:
```Python
>>> import sys
>>> print(sys.argv)
['demo.py', 'one', 'two', 'three']
```
错误输出重定向和程序终止

sys 还有 stdin，stdout 和 stderr 属性，即使在 stdout 被重定向时，后者也可以用于显示警告和错误信息。
```python
>>> sys.stderr.write('Warning, log file not found starting a new one\n')
Warning, log file not found starting a new one
```
大多脚本的定向终止都使用 "sys.exit()"。

3. 字符串正则匹配

re模块为高级字符串处理提供了正则表达式工具。对于复杂的匹配和处理，正则表达式提供了简洁、优化的解决方案:
```Python
>>> import re
>>> re.findall(r'\bf[a-z]*', 'which foot or hand fell fastest')
['foot', 'fell', 'fastest']
>>> re.sub(r'(\b[a-z]+) \1', r'\1', 'cat in the the hat')
'cat in the hat'
```
如果只需要简单的功能，应该首先考虑字符串方法，因为它们非常简单，易于阅读和调试:
```Python
>>> 'tea for too'.replace('too', 'two')
'tea for two'
```
4. 数字

math模块为浮点运算提供了对底层C函数库的访问:
```Python
>>> import math
>>> math.cos(math.pi / 4)
0.70710678118654757
>>> math.log(1024, 2)
10.0
```
random提供了生成随机数的工具。
```Python
>>> import random
>>> random.choice(['apple', 'pear', 'banana'])
'apple'
>>> random.sample(range(100), 10)   # sampling without replacement
[30, 83, 16, 4, 8, 81, 41, 50, 18, 33]
>>> random.random()    # random float
0.17970987693706186
>>> random.randrange(6)    # random integer chosen from range(6)
4
```
5. 访问互联网

有几个模块用于访问互联网以及处理网络通信协议。其中最简单的两个是用于处理从 urls 接收的数据的 urllib.request 以及用于发送电子邮件的 smtplib:
```Python
>>> from urllib.request import urlopen
>>> for line in urlopen('http://tycho.usno.navy.mil/cgi-bin/timer.pl'):
...     line = line.decode('utf-8')  # Decoding the binary data to text.
...     if 'EST' in line or 'EDT' in line:  # look for Eastern Time
...         print(line)

<BR>Nov. 25, 09:43:32 PM EST

>>> import smtplib
>>> server = smtplib.SMTP('localhost')
>>> server.sendmail('soothsayer@example.org', 'jcaesar@example.org',
... """To: jcaesar@example.org
... From: soothsayer@example.org
...
... Beware the Ides of March.
... """)
>>> server.quit()
```
注意第二个例子需要本地有一个在运行的邮件服务器。

6. 日期与时间

datetime模块为日期和时间处理同时提供了简单和复杂的方法。

支持日期和时间算法的同时，实现的重点放在更有效的处理和格式化输出。

该模块还支持时区处理:
```Python
>>> # dates are easily constructed and formatted
>>> from datetime import date
>>> now = date.today()
>>> now
datetime.date(2003, 12, 2)
>>> now.strftime("%m-%d-%y. %d %b %Y is a %A on the %d day of %B.")
'12-02-03. 02 Dec 2003 is a Tuesday on the 02 day of December.'

>>> # dates support calendar arithmetic
>>> birthday = date(1964, 7, 31)
>>> age = now - birthday
>>> age.days
14368
```
7. 数据压缩

以下模块直接支持通用的数据打包和压缩格式：zlib，gzip，bz2，zipfile，以及 tarfile。
```Python
>>> import zlib
>>> s = b'witch which has which witches wrist watch'
>>> len(s)
41
>>> t = zlib.compress(s)
>>> len(t)
37
>>> zlib.decompress(t)
b'witch which has which witches wrist watch'
>>> zlib.crc32(s)
226805979
```
8. 性能度量

有些用户对了解解决同一问题的不同方法之间的性能差异很感兴趣。Python 提供了一个度量工具，为这些问题提供了直接答案。

例如，使用元组封装和拆封来交换元素看起来要比使用传统的方法要诱人的多,timeit 证明了现代的方法更快一些。
```Python
>>> from timeit import Timer
>>> Timer('t=a; a=b; b=t', 'a=1; b=2').timeit()
0.57535828626024577
>>> Timer('a,b = b,a', 'a=1; b=2').timeit()
0.54962537085770791
```
相对于 timeit 的细粒度，:mod:profile 和 pstats 模块提供了针对更大代码块的时间度量工具。

9. 测试模块

开发高质量软件的方法之一是为每一个函数开发测试代码，并且在开发过程中经常进行测试

doctest模块提供了一个工具，扫描模块并根据程序中内嵌的文档字符串执行测试。

测试构造如同简单的将它的输出结果剪切并粘贴到文档字符串中。

通过用户提供的例子，它强化了文档，允许 doctest 模块确认代码的结果是否与文档一致:
```Python
def average(values):
"""Computes the arithmetic mean of a list of numbers.

>>> print(average([20, 30, 70]))
40.0
"""
return sum(values) / len(values)

import doctest
doctest.testmod()   # 自动验证嵌入测试
```
unittest模块不像 doctest模块那么容易使用，不过它可以在一个独立的文件里提供一个更全面的测试集:
```Python
import unittest

class TestStatisticalFunctions(unittest.TestCase):
  
  def test_average(self):
    self.assertEqual(average([20, 30, 70]), 40.0)
    self.assertEqual(round(average([1, 5, 7]), 1), 4.3)
    self.assertRaises(ZeroDivisionError, average, [])
    self.assertRaises(TypeError, average, 20, 30, 70)
    
    unittest.main() # Calling from the command line invokes all tests
```

