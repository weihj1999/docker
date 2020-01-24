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
