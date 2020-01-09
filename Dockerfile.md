# 1. Dockerfile快速学习

## 1.1 基础教程

主要内容：
1. 什么是Dockerfile？
2. 如何创建Dockderfile？
3. 如何从Dockerfile构建镜像？
4. 常用命令

============
什么是Dockerfile？
- 文本文件
- 包含构建Docker镜像自动化的指令（FROM,RUN,CMD)
![架构](/chapter3/images/dockerfile-001.PNG)


创建一个文件，命名文Dockderfile

```bash
# getting base image Ubuntu
FROM ubuntu
MAINTAINER Hank Wei <weihj1999@gmail.com>

RUN apt-get update

CMD ["echo","helloworld...! from my first docker image"]
```
dockerhub 有很多baseimage可以使用。
保存后退出， 使用docker build来构建镜像。

```bash
$ docker build -t myimage1:1.0 .
```

基于该镜像运行一个容器
```bash
$ docker run myimage1:1.0
```

Dockerfile Reference

## 1.2 基础指令

1. docker build

docker build命令从*dockerfile*和上下文*context*构建一个容器镜像。*context* 指的是一系列指定路径（**PATH**或者**URL**）下的文件。**PATH** 指的是本地文件系统中的一个目录，**URL** 指的是Git repository的位置。 *context* 支持递归处理，所以**PATH**包含了文件系统目录下的子目录，**URL** 包含repository下面的子模块。

基本使用方法：<br>
```bash
$ docker build .
Sending build context to Docker daemon  6.51 MB
...
```
构建过程由docker进程完成，而不是命令行cli，所以上面的命令，首先要做的就是把整个*context*发送给docker守护进程（deamon）。

在Docker守护进程运行Dockerfile中的指令之前，它会执行初步验证（preliminary validation），如果有错误直接返回：
```bash
$ docker build -t test/myapp .
Sending build context to Docker daemon 2.048 kB
Error response from daemon: Unknown instruction: RUNCMD
```

其他用法：
1. 指定Dockerfile的位置
```bash
$ docker build -f /path/to/a/Dockerfile .
```
2. 指定一个repository和标签
```bash
$ docker build -t shykes/myapp .
```

Docker执行Dockerfile中的指令是按顺序依次执行的，必要的情况下，每个指令都会生成一个新的image，一直到最终输出一个新的image。 Docker会自动清除发送的*context*。

>注意：<br>
每个指令都是独立执行的，同时会创建新的image。所以指令**RUN cd /tmp**对context里面的下一个指令不会有任何效果。

可能的情况下，Docker会重（chong）用中间临时镜像（cache，或者intermediate镜像）来加速*docker build*过程，这个可以从output中查看Using cache明显看的出来。 使用cache和操作cache参看下节build cache。

```bash
$ docker build -t svendowideit/ambassador .
Sending build context to Docker daemon 15.36 kB
Step 1/4 : FROM alpine:3.2
 ---> 31f630c65071
Step 2/4 : MAINTAINER SvenDowideit@home.org.au
 ---> Using cache
 ---> 2a1c91448f5f
Step 3/4 : RUN apk update &&      apk add socat &&        rm -r /var/cache/
 ---> Using cache
 ---> 21ed6e7fbb73
Step 4/4 : CMD env | grep _TCP= | (sed 's/.*_PORT_\([0-9]*\)_TCP=tcp:\/\/\(.*\):\(.*\)/socat -t 100000000 TCP4-LISTEN:\1,fork,reuseaddr TCP4:\2:\3 \&/' && echo wait) | sh
 ---> Using cache
 ---> 7ea8aef582cc
Successfully built 7ea8aef582cc
```

Build Cache只会在镜像存在本地父链（parent Chain）的时候才会使用，这些镜像有之前的build创建，或者镜像可以由*docker load*加载。 如果想使用指定的镜像使用cache，可以指定参数*--cache-from*

18.09版本以后，docker支持使用新的backend来执行镜像构建，这个工具叫做Buildkit，由*moby/buildkit*项目提供，主要提供的能力：
- 检测并跳过执行未使用的构建Stage
- 并行构建独立构建Stage
- 两次构建之间仅增量传输构建上下文中的更改文件
- 在构建上下文中检测并跳过传输未使用的文件
- 使用具有许多新功能的外部Dockerfile实现
- 避免与其他API产生副作用（中间图像和容器）
- 优先考虑构建缓存以进行自动修剪

使用buildkit，在运行*docker build*之前需要指定环境变量*DOCKER_BUILDKIT=1*

详细信息可以参考：[官方文档](https://github.com/moby/buildkit/blob/master/frontend/dockerfile/docs/experimental.md)

2. Dockerfile格式
Dockerfile基本格式如下：
```bash
# Comment
INSTRUCTION arguments
```
指定参数对大小写不敏感，但是通常为了区别，指令使用大写，参数使用小写。

docker按顺序运行dockfile中的指令，Dockerfile必须以一个FROM指令开始，在它之前可以放解析器指令（parser directive），注释以及全变参数（ARGS）等。

FROM指令制定了需要使用的父镜像，FROM可以先于一个或者多个ARG指令（用户声明Dockerfile中FROM指令需要使用到的参数）。

\#开头的行Docker会按照注释处理，除非那一行作为一个有效的解释器指令。下面显示的是注释：
```
# Comment
RUN echo 'we are running some # of cool things'
```
注释中不支持换行符(Line continuation characters)。

3. 解释器指令（Parser directive）
- 解析器指令是可选的，并且会影响Dockerfile中后续行的处理方式。
- 解析器指令不会在构建中添加执行层（Layer），也不会显示为构建步骤。
- 解释器指令通过特定的格式进行书写 **\# directive=value**。
- 单个指令只能使用一次。

处理完注释，空行或生成器指令后，Docker不再寻找解析器指令。 而是将格式化为解析器指令的任何内容都视为注释，并且不会尝试验证它是否可能是解析器指令。 因此，所有解析器指令必须位于Dockerfile的最顶部。

解析器指令不区分大小写。 但是，约定是小写的。 约定还应在任何解析器指令之后包含一个空白行。 解析器伪指令不支持行Line continuation characters（比如常见的_, \\之类的行继续符）。

基于以上规则，下面都是无效的示例：
```
# direc \
tive=value
```
出现两次解析器指令也是无效的。
```
# directive=value1
# directive=value2

FROM ImageName
```
按照注释处理，如果出现在构建指令之后
```
FROM ImageName
# directive=value
```
在注释后面也按照注释处理，即使写的是解析器指令
```
# About my dockerfile
# directive=value
FROM ImageName
```
不能解析的解释器指令也会按照注释处理，后续的即使是可以有效解析的解释器指令也会按照注释处理
```
# unknowndirective=value
# knowndirective=value
```
解析器指令中允许非换行空格。 因此，以下各行都被相同地对待：
```
#directive=value
# directive =value
#	directive= value
# directive = value
#	  dIrEcTiVe=value
```
下面两个指令可以支持：
- syntax
格式：
```
# syntax=[remote image reference]
```
比如：
```
# syntax=docker/dockerfile
# syntax=docker/dockerfile:1.0
# syntax=docker.io/docker/dockerfile:1
# syntax=docker/dockerfile:1.0.0-experimental
# syntax=example.com/user/repo:tag@sha256:abcdef...
```
这个功能，只有使用Buildkit的时候才支持。syntax指令定义了Dockerfile的位置。官方的镜像也可以在构建的Dockerfile中使用，有两个发布的官方的镜像发布途径：*stable and experimental*

Stable的样例：
- docker/dockerfile:1.0.0 - only allow immutable version 1.0.0
- docker/dockerfile:1.0 - allow versions 1.0.*
- docker/dockerfile:1 - allow versions 1..
- docker/dockerfile:latest - latest release on stable channel
experimental的样例：
- docker/dockerfile:1.0.1-experimental - only allow immutable version 1.0.1-experimental
- docker/dockerfile:1.0-experimental - latest experimental releases after 1.0
- docker/dockerfile:experimental - latest release on experimental channel

- escape
格式
```
# escape=\ (backslash)
```
或者
```
# escape=` (backtick)
```
模式的逃逸符是"\\".
举例：
```
FROM microsoft/nanoserver
COPY testfile.txt c:\\
RUN dir c:\
```
执行结果：
```
PS C:\John> docker build -t cmd .
Sending build context to Docker daemon 3.072 kB
Step 1/2 : FROM microsoft/nanoserver
 ---> 22738ff49c6d
Step 2/2 : COPY testfile.txt c:\RUN dir c:
GetFileAttributesEx c:RUN: The system cannot find the file specified.
PS C:\John>
```
如果设置**\`** 为逃逸符，可以修复上面的问题
例子：

```
# escape=`

FROM microsoft/nanoserver
COPY testfile.txt c:\
RUN dir c:\
```
输出结果如下：
```
PS C:\John> docker build -t succeeds --no-cache=true .
Sending build context to Docker daemon 3.072 kB
Step 1/3 : FROM microsoft/nanoserver
 ---> 22738ff49c6d
Step 2/3 : COPY testfile.txt c:\
 ---> 96655de338de
Removing intermediate container 4db9acbb1682
Step 3/3 : RUN dir c:\
 ---> Running in a2c157f842f5
 Volume in drive C has no label.
 Volume Serial Number is 7E6D-E0F7
```
4. 环境变量替换
环境变量通过ENV语句声明，在Dockerfile中被指令解析的变量。环境变量在Dockerfile中用$variable_name或${variable_name}表示。

${variable_name}语法还支持一些标准的bash修饰符，如下所示：
- ${variable:-word} 表示如果设置了变量，那么结果将是该值。 如果未设置变量，则将是word。
- ${variable:+word} 表示如果设置了变量，则word将为结果，否则结果为空字符串。

上面例子中的word恶意是任何字符串，包括其他的环境变量。

变量前面也可以使用逃逸符，比如：
```
FROM busybox
ENV foo /bar
WORKDIR ${foo}   # WORKDIR /bar
ADD . $foo       # ADD . /bar
COPY \$foo /quux # COPY $foo /quux
```
Dockerfile中支持环境变量的指令列表：
- ADD
- COPY
- ENV
- EXPOSE
- FROM
- LABEL
- STOPSIGNAL
- USER
- VOLUME
- WORKDIR
环境变量替换：
```
ENV abc=hello
ENV abc=bye def=$abc
ENV ghi=$abc
```
上面的例子def的值应该是hello，而不是bye； ghi的值是bye，因为它把abc赋值为bye的同一个指令；

5. .dockerignore文件

在Docker命令行将上下文发送到Docker守护程序之前，它将在上下文的根目录中查找名为.dockerignore的文件。 如果此文件存在，则CLI会修改上下文以排除与其中的模式匹配的文件和目录。 这有助于避免将不必要的大文件或敏感文件和目录发送到守护程序，并避免使用ADD或COPY将它们添加到映像中。

命令行将.dockerignore文件解释为以换行符分隔的模式列表，类似于Unix shell的文件组。 为了匹配，上下文的根被认为是工作目录和根目录。 例如，/foo/bar和foo/bar都在PATH的foo子目录中或位于URL的git存储库的根目录中排除名为bar的文件或目录。 两者都不排除其他任何东西。

如果.dockerignore文件中的行以第1列中的＃开头，则该行将被视为注释，并且在命令行解释之前将被忽略。
```
# comment
*/temp*
*/*/temp*
temp?
```
其他详细使用可以参考社区手册，进行高级开发；

6. 指令详解
-  FROM指令

FROM指令支持使用变量，在FROM之前声明的任何ARG指令定义的变量，比如：
```
ARG  CODE_VERSION=latest
FROM base:${CODE_VERSION}
CMD  /code/run-app

FROM extras:${CODE_VERSION}
CMD  /code/run-extras
```
在FROM之前声明的ARG，实际上不会进入构建阶段，所以不能被任何FROM之后的指令使用。

下面的例子说明了使用FROM之前的ARG的默认值，已经在构建阶段使用ARG指令定义的空值。

```
ARG VERSION=latest
FROM busybox:$VERSION
ARG VERSION
RUN echo $VERSION > image_version
```

- RUN指令

RUN指令支持两种形式：<br>
  - RUN \<command> (shell格式, 命令在shell中支持，默认的shell是/bin/sh -c (Linux) 或者 cmd /S /C (Windows)
  - RUN ["executable", "param1", "param2"] (exec格式)

如果使用非默认的shell，可以使用exec格式，传入相应的shell
**RUN ["/bin/bash", "-c", "echo hello"]**

shell格式下，支持使用反斜杠(\\)把一个RUN指令分写成两行
```
RUN /bin/bash -c 'source $HOME/.bashrc; \
echo $HOME'
```
等同于同一行
```
RUN /bin/bash -c 'source $HOME/.bashrc; echo $HOME'
```

exec格式实际上是通过JSON字符串解析的，所以必须使用双引号，而不是单引号。

- CMD指令

CMD指令的三种格式：
- CMD ["executable","param1","param2"] (exec格式, 最为推荐的格式)
- CMD ["param1","param2"] (默认的参数是ENTRYPOINT)
- CMD command param1 param2 (shell格式)

一个Dockerfile中只能有一个CMD指令：

**CMD的主要用途是为执行中的容器提供默认值**。 这些默认值可以包含可执行文件，也可以省略可执行文件，在这种情况下，您还必须指定ENTRYPOINT指令。同样是按照JSON格式进行解析的。

当以shell或exec格式使用时，CMD指令设置运行映像时要执行的命令。

shell格式的时候，CMD后面的命令将在/bin/sh -c下执行：

```
FROM ubuntu
CMD echo "This is a test." | wc -
```
如果希望不适用shell执行命令，请参考下面的例子：
```
FROM ubuntu
CMD ["/usr/bin/wc","--help"]
```
如果您希望容器每次都运行相同的可执行文件，则应考虑将ENTRYPOINT与CMD结合使用。

如果用户为docker run指定了参数，则它们将覆盖CMD中指定的默认值。

- LABEL指令

LABEL指令是key-value对，给镜像添加metadata。
```
LABEL <key>=<value> <key>=<value> <key>=<value> ...
```

举例：
```
LABEL "com.example.vendor"="ACME Incorporated"
LABEL com.example.label-with-value="foo"
LABEL version="1.0"
LABEL description="This text illustrates \
that label-values can span multiple lines."
```
一个镜像可以有多个label，可以在同一行中指定多个label。
```
LABEL multi.label1="value1" multi.label2="value2" other="value3"

LABEL multi.label1="value1" \
      multi.label2="value2" \
      other="value3"
```
label支持在基础镜像（父镜像）中进行继承，可以通过docker inspect来查看一个镜像的标签：
```
"Labels": {
    "com.example.vendor": "ACME Incorporated"
    "com.example.label-with-value": "foo",
    "version": "1.0",
    "description": "This text illustrates that label-values can span multiple lines.",
    "multi.label1": "value1",
    "multi.label2": "value2",
    "other": "value3"
},
```

- MAINTAINER指令

通常注明作者，但是没有label灵活，所有基本上就不适用这个指令，使用label。
```
LABEL maintainer="SvenDowideit@home.org.au
```

- EXPOSE指令

设定对外开放的端口：
```
EXPOSE <port> [<port>/<protocol>...]
```
默认情况下开放的端口是TCP协议，也可以指定为UDP:
```
EXPOSE 80/udp
```
同时开放TCP和UDP端口，写两行：
```
EXPOSE 80/tcp
EXPOSE 80/udp
```

- ENV指令

支持两种格式：
```
ENV <key> <value>
ENV <key>=<value> ...
```
ENV指令将环境变量<key>设置为值<value>。 此值将在构建阶段中所有后续指令的环境中使用，并且在许多情况下也可以内联替换(Replaced inline)。

ENV指令有两种形式。 第一种形式，ENV \<key> \<value>，将单个变量设置为一个值。 第一个空格之后的整个字符串将被视为\<value>, 包括空格字符。 该值将为其他环境变量解释，因此如果不对引号字符进行转义，则将其删除。

第二种格式ENV \<key> = \<value> ...，允许一次设置多个变量。 请注意，第二种形式在语法中使用等号（=），而第一种形式则不使用等号（=）。 与命令行解析一样，引号和反斜杠可用于在值中包含空格。

比如：
```
ENV myName="John Doe" myDog=Rex\ The\ Dog \
    myCat=fluffy
```
以及：
```
ENV myName John Doe
ENV myDog Rex The Dog
ENV myCat fluffy
```
上面的效果是一样的。

从映像运行容器时，使用ENV设置的环境变量将保留。 可以使用docker inspect查看值，并使用docker run --env <key> = <value>更改它们。

>注意：<br>
环境持久性可能导致意外的副作用。 例如，将ENV DEBIAN_FRONTEND设置为非交互式可能会使基于Debian的映像上的apt-get用户感到困惑。 要为单个命令设置值，请使用RUN \<key> = \<value> \<command>。

- ADD指令

ADD指令支持两种格式：
  - ADD [--chown=\<user>:\<group>] \<src>... \<dest>
  - ADD [--chown=\<user>:\<group>] ["\<src>",... "\<dest>"] (包含空格的路径需要此格式)

>注意：<br>
--chown功能仅在用于构建Linux容器的Dockerfiles，而在Windows容器上不起作用。 由于用户和组所有权概念不会在Linux和Windows之间转换，因此使用/etc/passwd和/etc/group将用户名和组名转换为ID限制了该功能仅适用于基于Linux OS的容器。

ADD指令从<src>复制新文件，目录或远程文件URL，并将它们添加到映像的文件系统中的路径<dest>。

可以指定多个<src>资源，但是如果它们是文件或目录，则将其路径解释为相对于构建上下文源的路径。

每个<src>都可以包含通配符，并且匹配将使用Go的filepath.Match规则进行。 例如：

```
ADD hom* /mydir/        # adds all files starting with "hom"
ADD hom?.txt /mydir/    # ? is replaced with any single character, e.g., "home.txt"
```
\<dest>是绝对路径，或相对于WORKDIR的路径，源将在目标容器内复制到该路径。
```
ADD test relativeDir/          # adds "test" to `WORKDIR`/relativeDir/
ADD test /absoluteDir/         # adds "test" to /absoluteDir/
```
添加包含特殊字符（例如[和]）的文件或目录时，您需要按照Golang规则转义那些路径，以防止将它们视为匹配模式。 例如，要添加名为arr [0] .txt的文件，请使用以下命令：
```
ADD arr[[]0].txt /mydir/    # copy a file named "arr[0].txt" to /mydir/
```
通常所有新文件和目录均使用UID和GID为0创建，除非使用--chown标志指定给定的用户名，组名或UID / GID组合以请求对所添加内容的特定所有权。--chown标志的格式允许使用用户名和组名字符串或直接整数UID和GID的任意组合。 提供不带组名的用户名或不带GID的UID将使用与GID相同的数字UID。 如果提供了用户名或组名，则将使用容器的根文件系统/etc/passwd和/etc/group文件分别执行从名称到整数UID或GID的转换。 以下示例显示了--chown标志的有效定义：
```
ADD --chown=55:mygroup files* /somedir/
ADD --chown=bin files* /somedir/
ADD --chown=1 files* /somedir/
ADD --chown=10:11 files* /somedir/
```
如果容器根文件系统不包含/etc/passwd或/etc/group文件，并且在--chown标志中使用了用户名或组名，则生成将在ADD操作中失败。 使用数字ID不需要查找，并且不依赖于容器根文件系统内容。

如果<src>是远程文件URL，则目标将具有600的权限。如果要检索的远程文件具有HTTP Last-Modified头，则该头的时间戳将用于在目标上设置mtime 文件。 但是，就像在ADD中处理的任何其他文件一样，在确定文件是否已更改以及是否应更新缓存的确定中，将不包括mtime。
>注意：<br>
如果通过通过STDIN传递Dockerfile进行构建（docker build-<somefile），则没有构建上下文，因此Dockerfile只能包含基于URL的ADD指令。 还可以通过STDIN传递压缩的存档：（docker build - <archive.tar.gz），位于存档根目录的Dockerfile和其余的存档将用作构建的上下文。

>注意：<br>
如果您的URL文件受身份验证保护，则您将需要使用RUN wget，RUN curl或从容器中使用其他工具，因为ADD指令不支持身份验证。

>注意：<br>
如果<src>的内容已更改，则第一个遇到的ADD指令将使Dockerfile中所有后续指令的缓存无效。 这包括使RUN指令的缓存无效。 有关更多信息，请参阅[《 Dockerfile最佳实践》指南](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#/build-cache)

具体ADD指令使用需要遵循一些基本的目录操作守则，参考官方手册。

- COPY指令

两种格式：
- COPY [--chown=\<user>:\<group>] \<src>... \<dest>
- COPY [--chown=\<user>:\<group>] ["\<src>",... "\<dest>"] (包含空格的路径需要此格式)

具体手册参考官方文档。

- ENTRYPOINT指令

两种格式：
  - ENTRYPOINT ["executable", "param1", "param2"] (exec格式, 优先)
  - ENTRYPOINT command param1 param2 (shell格式)

ENTRYPOINT允许您配置将作为可执行文件运行的容器。

比如启动nginx容器，监听在80端口：
```bash
docker run -i -t --rm -p 80:80 nginx
```
docker run <image>的命令行参数将在exec格式ENTRYPOINT的所有元素之后附加，并将覆盖使用CMD指定的所有元素。 这允许将参数传递给容器entry point，即docker run <image> -d将-d参数传递给entry point。 也可以使用docker run --entrypoint 标志覆盖ENTRYPOINT指令。

Shell格式可防止使用任何CMD或run命令行参数，但具有以下缺点：ENTRYPOINT将作为/bin/sh -c的子命令启动，该子命令不传递singal。 这意味着可执行文件将不是容器的PID 1，并且不会接收Unix singal，因此您的可执行文件将不会从docker stop <container>接收到SIGTERM。

只有Dockerfile中的最后一条ENTRYPOINT指令才会生效。

**Exec格式的ENTRYPOINT样例**

```bash
FROM ubuntu
ENTRYPOINT ["top", "-b"]
CMD ["-c"]
```
当运行容器的时候，top将会是唯一的进程
```bash
$ docker run -it --rm --name test  top -H
top - 08:25:00 up  7:27,  0 users,  load average: 0.00, 0.01, 0.05
Threads:   1 total,   1 running,   0 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.1 us,  0.1 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem:   2056668 total,  1616832 used,   439836 free,    99352 buffers
KiB Swap:  1441840 total,        0 used,  1441840 free.  1324440 cached Mem

  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
    1 root      20   0   19744   2336   2080 R  0.0  0.1   0:00.04 top
```
或者用docker exec查看进程
```bash
$ docker exec -it test ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  2.6  0.1  19752  2352 ?        Ss+  08:24   0:00 top -b -H
root         7  0.0  0.1  15572  2164 ?        R+   08:25   0:00 ps aux
```
下面的Docerfile例子使用ENTRYPOINT来，作为foreground运行Apache(比如说PID 1)
```bash
FROM debian:stable
RUN apt-get update && apt-get install -y --force-yes apache2
EXPOSE 80 443
VOLUME ["/var/www", "/var/log/apache2", "/etc/apache2"]
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```
如果需要写一个启动脚本来做为单一的可执行程序，通过使用exec和gosu命令, 可以确保可执行程序能够接受UNIX signal，：

```bash
#!/usr/bin/env bash
set -e

if [ "$1" = 'postgres' ]; then
    chown -R postgres "$PGDATA"

    if [ -z "$(ls -A "$PGDATA")" ]; then
        gosu postgres initdb
    fi

    exec gosu postgres "$@"
fi

exec "$@"
```
最后，如果您需要在关闭时进行一些额外的清理（或与其他容器通信）工作，或协调多个可执行文件，则需要确保ENTRYPOINT脚本接收Unix信号，然后将其传递，然后 还有更多工作：
```bash
#!/bin/sh
# Note: I've written this using sh so it works in the busybox container too

# USE the trap if you need to also do manual cleanup after the service is stopped,
#     or need to start multiple services in the one container
trap "echo TRAPed signal" HUP INT QUIT TERM

# start service in background here
/usr/sbin/apachectl start

echo "[hit enter key to exit] or run 'docker stop <container>'"
read

# stop service and clean up here
echo "stopping apache"
/usr/sbin/apachectl stop

echo "exited $0"
```
使用这个镜像运行docker run -it --rm -p 80:80 --name test apache，可以使用docker exec或者docker top检查容器的进程，然后请求搅拌来停掉Apache：
```bash
$ docker exec -it test ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.1  0.0   4448   692 ?        Ss+  00:42   0:00 /bin/sh /run.sh 123 cmd cmd2
root        19  0.0  0.2  71304  4440 ?        Ss   00:42   0:00 /usr/sbin/apache2 -k start
www-data    20  0.2  0.2 360468  6004 ?        Sl   00:42   0:00 /usr/sbin/apache2 -k start
www-data    21  0.2  0.2 360468  6000 ?        Sl   00:42   0:00 /usr/sbin/apache2 -k start
root        81  0.0  0.1  15572  2140 ?        R+   00:44   0:00 ps aux
$ docker top test
PID                 USER                COMMAND
10035               root                {run.sh} /bin/sh /run.sh 123 cmd cmd2
10054               root                /usr/sbin/apache2 -k start
10055               33                  /usr/sbin/apache2 -k start
10056               33                  /usr/sbin/apache2 -k start
$ /usr/bin/time docker stop test
test
real	0m 0.27s
user	0m 0.03s
sys	0m 0.03s
```
>注意：<br>
可以使用--entrypoint覆盖ENTRYPOINT设置，但这只能将二进制文件设置为exec（将不使用sh -c）。

>注意：<br>
exec格式被解析为JSON数组，这意味着您必须在单词周围使用双引号（“）,而非单引号（'）。

>注意：<br>
与shell格式不同，exec格式不会调用命令shell。 这意味着正常的shell处理不会发生。 例如，ENTRYPOINT [“ echo”，“ $HOME”]不会在$HOME上进行变量替换。 如果要进行shell处理，则可以使用shell形式或直接执行shell，例如：ENTRYPOINT [“sh”，“-c”，“echo $HOME”]。 当使用exec格式并直接执行shell时（例如在shell格式中），是由shell进行环境变量扩展，而不是docker。

## 1.3 最佳实践一

从上面的教程可以了解到，docker会从Dockerfile中读取指令自动构建镜像，Dockerfile是一个包含了所有命令，要求顺序执行，用来构建想要的镜像。 Dockerfile同时遵循特定的格式，支持特定的指令。

Docker镜像有一系列的只读层组成，每个只读层代表了一个Dockerfile指令。每个层堆叠起来，每个层都是上一层变化的增量，来看下这个Dockerfile：

```bash
FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.py
```
每个指令创建一层
- FROM指令从ubuntu:18.04镜像创建一层
- COPY指令从客户端当前目录添加文件
- RUN指令使用make命令构建应用
- CMD指定需要在容器内运行的命令

运行镜像并生成容器时，可以在基础层之上添加一个新的可写层（“容器层”）。 对运行中的容器所做的所有更改（例如写入新文件，修改现有文件和删除文件）都将写入此可写容器层（thin容器）。

### 1.3.1 通用准则和建议
#### 1.3.1.1 创建临时（ephemeral）容器
Dockerfile顶一个镜像会创建临时容器，“临时”意味着这个容器可以随时停止，销毁（Destroy），重建（rebuild），替换，采用的是最小的配置。

这里推荐参考[Twelve-factor App Methodology](https://12factor.net/processes)来了解如何创建无状态容器应用.

#### 1.3.1.2 理解构建上下文（Context）

当执行一个docker build命令时，当前的工作目录被称为构建上下文（context），默认情况下，Dockerfile就在这个目录，但是用户可以通过-f参数来指定不同位置，不管Dockerfile在什么地方，当前目录本身和子目录下面的文件都会被发送给Docker程序作为上下文。

**构建上下文的样例**

创建一个目录作为构建的上下文，使用cd命令进入该目录，下一个hello到文本文件,再创建一个Dockerfile中，在里面运行cat命令，基于该上下文构建镜像：
```bash
mkdir myproject && cd myproject
echo "hello" > hello
echo -e "FROM busybox\nCOPY /hello /\nRUN cat /hello" > Dockerfile
docker build -t helloapp:v1 .
```

把Dockerfile和hello复制到另外一个独立的目录，构建另外一个版本的镜像（这么做的原因是避免使用上一次构建的缓存）。使用-f指定这个Dockerfile和构建上下文的目录：
```bash
mkdir -p dockerfiles context
mv Dockerfile dockerfiles && mv hello context
docker build --no-cache -t helloapp:v2 -f dockerfiles/Dockerfile context
```
#### 1.3.1.3 使用stdin生成Dockerfile
Docker可以通过使用本地或者远程构建上下文，通过stdin来导入Dockerfile； 通过stdin插入Dockerfile可以在不将Dockerfile写入磁盘的情况下执行一次构建，或者在生成Dockerfile后期不做长期保存的情况下使用。

样例：
```bash
echo -e 'FROM busybox\nRUN echo "hello world"' | docker build -
```

```bash
docker build -<<EOF
FROM busybox
RUN echo "hello world"
EOF
```

这种方法在不需要上传构建上下文的情况下很有用。使用此语法可使用来自stdin的Dockerfile构建镜像，而无需发送其他文件作为构建上下文。 连字符（-）占据PATH的位置，并指示Docker从stdin而不是目录中读取构建上下文（仅包含Dockerfile）：
```bash
docker build [OPTIONS] -
```
以下示例使用通过stdin传递的Dockerfile构建镜像。没有文件作为构建上下文发送到守护程序
```bash
docker build -t myimage:latest -<<EOF
FROM busybox
RUN echo "hello world"
EOF
```

在您的Dockerfile不需要将文件复制到镜像中的情况下，省略构建上下文会很有用，并且由于没有文件发送到守护程序，因此可以提高构建速度。

注意：如果使用此语法，尝试构建使用COPY或ADD的Dockerfile将会失败。以下示例说明了这一点：

```bash
# create a directory to work in
mkdir example
cd example

# create an example file
touch somefile.txt

docker build -t myimage:latest -<<EOF
FROM busybox
COPY somefile.txt .
RUN cat /somefile.txt
EOF

# observe that the build fails
...
Step 2/3 : COPY somefile.txt .
COPY failed: stat /var/lib/docker/tmp/docker-builder249218248/somefile.txt: no such file or directory
```

使用此语法可使用本地文件系统上的文件，但使用来自stdin的Dockerfile构建镜像。 该语法使用-f（或--file）选项指定要使用的Dockerfile，并使用连字符（-）作为文件名来指示Docker从stdin中读取Dockerfile。

```bash
docker build [OPTIONS] -f- PATH
```
下面的示例使用当前目录（.）作为构建上下文，并使用通过stdin传递的Dockerfile构建镜像:

```bash
# create a directory to work in
mkdir example
cd example

# create an example file
touch somefile.txt

# build an image using the current directory as context, and a Dockerfile passed through stdin
docker build -t myimage:latest -f- . <<EOF
FROM busybox
COPY somefile.txt .
RUN cat /somefile.txt
EOF
```
使用来自stdin的Dockerfile使用来自远程git存储库的文件构建镜像。该语法使用-f（或--file）选项指定要使用的Dockerfile，并使用连字符（-）作为文件名来指示Docker从stdin中读取Dockerfile

```bash
docker build [OPTIONS] -f- PATH
```

如果您要从不包含Dockerfile的存储库中构建镜像，或者想要使用自定义Dockerfile进行构建而不维护自己的存储库，则此语法很有用。 下面的示例使用stdin中的Dockerfile构建镜像，并从GitHub上的“ hello-world” Git存储库添加hello.c文件。

```bash
docker build -t myimage:latest -f- https://github.com/docker-library/hello-world.git <<EOF
FROM busybox
COPY hello.c .
EOF
```

当使用远程Git存储库作为构建上下文构建镜像时，Docker在本地计算机上执行存储库的git克隆，并将这些文件作为构建上下文发送到守护程序。 此功能要求将git安装在运行docker build命令的主机上

当使用远程Git存储库作为构建上下文构建镜像时，Docker git clone在本地计算机上执行一个存储库(Repository)，并将这些文件作为构建上下文发送到守护程序。需要git在运行docker build命令的主机上安装此功能

#### 1.3.1.4 使用多阶段构建
使用多阶段构建可以大幅度降低最终镜像的大小；因为镜像在构建的最终阶段才会生成，用户也可以考虑最小化镜像的层数，通过使用cache build。

比如说，如果构建几个多层的镜像，可以按照经常变化的频度来设计能够重用的cache层，比如：
- 安装构建应用的工具
- 安装更新依赖的包
- 生成应用

下面的例子是一个go的应用Dockerfile样例：
```bash
FROM golang:1.11-alpine AS build

# Install tools required for project
# Run `docker build --no-cache .` to update dependencies
RUN apk add --no-cache git
RUN go get github.com/golang/dep/cmd/dep

# List project dependencies with Gopkg.toml and Gopkg.lock
# These layers are only re-built when Gopkg files are updated
COPY Gopkg.lock Gopkg.toml /go/src/project/
WORKDIR /go/src/project/
# Install library dependencies
RUN dep ensure -vendor-only

# Copy the entire project and build it
# This layer is rebuilt when a file changes in the project directory
COPY . /go/src/project/
RUN go build -o /bin/project

# This results in a single layer image
FROM scratch
COPY --from=build /bin/project /bin/project
ENTRYPOINT ["/bin/project"]
CMD ["--help"]
```
#### 1.3.1.5 不要安装不必要的包

为了降低复杂性，依赖性，文件大小和构建时间，请避免仅由于它们“很容易”而安装多余或不必要的软件包。例如，您不需要在数据库镜像中包括文本编辑器。

1. 应用解耦
每个容器只解决一个问题，把应用解耦到多个容器中，会是应用的水平扩展很容易。比如说一个Web应用堆栈可能包含三个独立的容器，每一个都是用不同的镜像，分别管理web应用，数据库和in-memory缓存。

把每个容器限制为一个进程，是一个很好的经验法则（rule of thumb），但也不是一成不变的。比如说，不仅可以使用init进程来生成容器，而且有些程序也可以自行生成其他进程。比如说Celery可以生成多个工作进程，而Apache可以为每个请求创建一个进程。

这个通常要根据各自的经验，使容器尽可能的保持清洁和模块化，如果容器相互依赖，则可以使用docker容器网络来确保这些容器可以通信。

2. 减少层数

在较旧的Docker版本中，务必最小化镜像中的层数以确保其性能。添加了以下功能来减少此限制：
- 只有指令RUN，COPY，ADD会创建镜像层。 其他说明创建临时的中间镜像，并且不会增加构建的大小。
- 尽可能使用多阶段构建，并且仅将所需的工件复制到最终镜像中。 这使您可以在中间构建阶段中包含工具和调试信息，而无需增加最终镜像的大小。

3. 多行参数排序

尽可能通过字母数字排序多行参数来简化以后的更改。 这有助于避免软件包重复，并使列表更易于更新。 这也使PRs易于阅读和查看。 在反斜杠（\）之前添加空格也有帮助。

这里有个构建包依赖的镜像样例：

```bash
RUN apt-get update && apt-get install -y \
  bzr \
  cvs \
  git \
  mercurial \
  subversion
```
4. 借用build Cache

构建镜像时，Docker会逐步执行Dockerfile中的指令，并按照指定的顺序执行每个指令。 在检查每条指令时，Docker在其缓存中查找可以重用的现有镜像，而不是创建新的（重复的）镜像

如果不想使用cahce，可以在执行docker build的时候加上选项：--no-cache=true；是，如果您确实需要让Docker使用其缓存，那么了解何时可以找到匹配的镜像，这一点很重要。 Docker遵循的基本规则概述如下:
- 从已在缓存中的父镜像开始，将下一条指令与从该基本镜像派生的所有子镜像进行比较，以查看是否其中一个是使用完全相同的指令构建的。如果不是，则高速缓存无效。
- 在大多数情况下，仅将Dockerfile中的指令与子镜像之一进行比较就足够了。但是，某些说明需要更多的检查和解释。
- 对于ADD和COPY指令，将检查图像中文件的内容，并为每个文件计算一个校验和。在这些校验和中不考虑文件的最后修改时间和最后访问时间。在高速缓存查找期间，将校验和与现有镜像中的校验和进行比较。如果文件中的任何内容（例如内容和元数据）发生了更改，则缓存将无效。
- 除了ADD和COPY命令外，缓存检查不会查看容器中的文件来确定缓存是否匹配。例如，在处理RUN apt-get -y update命令时，不会检查容器中更新的文件以确定是否存在缓存命中。在这种情况下，仅使用命令字符串本身来查找匹配项。

缓存无效后，所有后续Dockerfile命令都会生成新镜像，并且不使用缓存

### 1.3.2 Dockerfile指令

1. FROM

只要有可能，尽可能使用当前的官方镜像作为基础镜像，推荐使用Alpine镜像，因为这个镜像有严格控制的，并且尺寸很小（当前小于5MB），同时仍然是完整的Linux发行版。

2. LABEL

可以给镜像添加标签，以帮助按项目组织镜像，记录许可信息，帮助自动化或其他原因。 对于每个标签，添加一行以LABEL开头并带有一个或多个键值对的行。 以下示例显示了不同的可接受格式。 内嵌包含解释性注释。
```bash
# Set one or more individual labels
LABEL com.example.version="0.0.1-beta"
LABEL vendor1="ACME Incorporated"
LABEL vendor2=ZENITH\ Incorporated
LABEL com.example.release-date="2015-02-12"
LABEL com.example.version.is-production=""
```

带空格的字符串必须用引号引起来，否则必须转义。引号字符（“）也必须转义才能正确使用。

镜像可以有多个标签。在Docker 1.10之前，建议将所有标签合并为一个LABEL指令，以防止创建额外的层。不再需要此操作，但仍支持组合标签。

```bash
# Set multiple labels on one line
LABEL com.example.version="0.0.1-beta" com.example.release-date="2015-02-12"
```
也可以写成下面的形式：
```bash
# Set multiple labels at once, using line-continuation characters to break long lines
LABEL vendor=ACME\ Incorporated \
      com.example.is-beta= \
      com.example.is-production="" \
      com.example.version="0.0.1-beta" \
      com.example.release-date="2015-02-12"
```
3. RUN
用反斜杠分隔长的，复杂的RUN语句有助于让Dockerfile更易读，易理解，易维护。

RUN指令最常用的莫过于APT-GET，用于安装各种软件包。

避免使用RUN apt-get upgrade和dist-upgrade，因为很多父镜像中必要的安装包在一个unprivileged容器内都不能成功升级。如果父镜像里面的软件包过期了，联系维护人员；付过你了解到具体的包需要更新，比如说说foo包，使用apt-get install -y foo自动更新。

通常来说会把apt-get update和apt-get install综合在RUN中使用，比如：

```bash
RUN apt-get update && apt-get install -y \
    package-bar \
    package-baz \
    package-foo
```

单独使用atp-get upgrade可能会导致缓存问题和后续apt-get install失败，比如：

```bash
FROM ubuntu:18.04
RUN apt-get update
RUN apt-get install -y curl
```
镜像构建完毕后，所有的层都在docker缓存中，假设后续修改了apt-get install来添加额外的包：
```bash
FROM ubuntu:18.04
RUN apt-get update
RUN apt-get install -y curl nginx
```
Docker将初始指令和修改后的指令视为相同，并重复使用先前步骤中的缓存。 结果，由于构建使用了缓存版本，因此不执行apt-get update。 由于未运行apt-get更新，因此您的构建可能会获得curl和nginx软件包的过时版本。

使用RUN apt-get update && apt-get install -y可确保您的Dockerfile安装最新的软件包版本，而无需进一步的编码或手动干预。 这种技术称为“缓存清除（cache busting）”。 您还可以通过指定软件包版本来实现缓存清除。 这称为版本固定（version pinning），例如：

```bash
RUN apt-get update && apt-get install -y \
    package-bar \
    package-baz \
    package-foo=1.3.*
```
版本固定会强制构建物检索特定版本，而不管缓存中的内容是什么。此技术还可以减少由于所需程序包中的意外更改而导致的故障。

下面就是一个非常好的演示用例（推荐）：

```bash
RUN apt-get update && apt-get install -y \
    aufs-tools \
    automake \
    build-essential \
    curl \
    dpkg-sig \
    libcap-dev \
    libsqlite3-dev \
    mercurial \
    reprepro \
    ruby1.9.1 \
    ruby1.9.1-dev \
    s3cmd=1.1.* \
 && rm -rf /var/lib/apt/lists/*
```
s3cmd参数指定版本1.1.\*。 如果镜像先前使用的是旧版本，则指定新版本会导致apt-get更新的缓存崩溃，并确保安装新版本。 在每一行列出软件包也可以防止软件包重复中的错误.

另外，当通过删除/var/ lib/apt/lists清理apt缓存时，由于apt缓存未存储在镜像层中，因此会减小镜像大小。 由于RUN语句以apt-get update开始，因此软件包缓存始终在apt-get install之前刷新。

使用管道（PIPES）

某些RUN命令取决于使用管道字符（|）将一个命令的输出管道传输到另一个命令的能力，如以下示例所示：

```bash
RUN wget -O - https://some.site | wc -l > /number
```
Docker使用/bin/sh -c解释器执行这些命令，该解释器仅评估管道中最后一个操作的退出代码以确定成功。 在上面的示例中，只要wc -l命令成功，即使wget命令失败，该构建步骤也会成功并生成一个新镜像

如果您希望该命令由于管道中的任何阶段的错误而失败，请在set -o pipefail &&之前添加前缀，以确保意外的错误可以防止构建意外进行。例如

```bash
RUN set -o pipefail && wget -O - https://some.site | wc -l > /number
```
4. CMD
应使用CMD指令来**运行镜像所包含的软件**以及**所有参数**。 CMD几乎应始终以CMD [“ executable”，“ param1”，“ param2”…]的形式使用。 因此，如果镜像用于服务（例如Apache和Rails），则将运行诸如CMD [“ apache2”，“-DFOREGROUND”]之类的内容。 实际上，对于任何基于服务的镜像都建议使用这种形式的指令。

在大多数其他情况下，应为CMD提供交互式shell，例如bash，python和perl。 例如，CMD [“perl”，“-de0”]，CMD [“python”]或CMD [“php”，“-a”]。 使用此表单意味着执行docker run -it python之类的操作时，您将进入可用的shell中，随时可以使用。 除非您和您的预期用户已经非常熟悉ENTRYPOINT的工作原理，否则CMD很少以CMD [“ param”，“ param”]的形式与ENTRYPOINT结合使用。

5. EXPOSE

EXPOSE指令指示容器在其上侦听连接的端口。 因此，应为应用程序使用通用的传统端口。 例如，包含Apache Web服务器的映像将使用EXPOSE 80，而包含MongoDB的映像将使用EXPOSE 27017，依此类推。

对于外部访问，您的用户可以执行带有flag的docker run，该标志指示如何将指定端口映射到他们选择的端口。 对于容器链接，Docker为从收件人容器到源容器的路径（即，MYSQL_PORT_3306_TCP）提供了环境变量。

5. ENV

为了使新软件更易于运行，可以使用ENV为容器安装的软件更新PATH环境变量。 例如，ENV PATH /usr/local/nginx/bin：$PATH, 确保CMD [“nginx”]正常工作。

ENV指令还可用于提供特定于您要容器化的服务的必需环境变量，例如Postgres的PGDATA。

最后，ENV还可以用于设置常用的版本号，以便更容易维护版本，如以下示例所示:

```bash
ENV PG_MAJOR 9.3
ENV PG_VERSION 9.3.4
RUN curl -SL http://example.com/postgres-$PG_VERSION.tar.xz | tar -xJC /usr/src/postgress && …
ENV PATH /usr/local/postgres-$PG_MAJOR/bin:$PATH
```
类似于在程序中具有常量变量（与硬编码值相反），这种方法使您可以更改单个ENV指令，以自动神奇地修改容器中软件的版本。

每条ENV行都会创建一个新的中间层，就像RUN命令一样。 这意味着即使您在以后的层中取消设置环境变量，它也仍将保留在该层中，并且其值可以转储。 您可以通过创建如下所示的Dockerfile，然后对其进行构建来进行测试。

```bash
FROM alpine
ENV ADMIN_USER="mark"
RUN echo $ADMIN_USER > ./mark
RUN unset ADMIN_USER
```

```bash
$ docker run --rm test sh -c 'echo $ADMIN_USER'

mark
```
为避免这种情况，并确实需要取消设置环境变量，请在shell层中使用RUN命令和shell命令来设置，使用和取消设置该变量。 您可以使用;分隔命令或者&&。 如果您使用第二种方法，并且其中一个命令失败，则docker build也将失败。 这通常是个好主意。 将\\用作Linux Dockerfiles的行连续字符可提高可读性。 您也可以将所有命令放入shell脚本中，并让RUN命令仅运行shell脚本。

```bash
FROM alpine
RUN export ADMIN_USER="mark" \
    && echo $ADMIN_USER > ./mark \
    && unset ADMIN_USER
CMD sh
```

```bash
$ docker run --rm test sh -c 'echo $ADMIN_USER'
```
6. ADD or COPY

尽管ADD和COPY在功能上相似，但通常来说COPY是首选。 那是因为它比ADD更透明。 COPY仅支持将本地文件基本复制到容器中，而ADD的某些功能（如仅本地tar提取和远程URL支持）并不是立即显而易见的。 ADD的最佳用途是将本地tar文件自动提取到映像中,比如：ADD rootfs.tar.xz / 。

如果您有多个使用不同上下文的文件的Dockerfile步骤，请单独复制而不是一次全部复制。 这样可以确保仅在特别需要的文件发生更改时，才使每个步骤的构建缓存无效（强制重新运行该步骤）。

比如：
```bash
COPY requirements.txt /tmp/
RUN pip install --requirement /tmp/requirements.txt
COPY . /tmp/
```
对于RUN运行之前的cahce会比较小，如果把COPY . /tmp/放在RUN之前，则Cache会比较大。

由于镜像大小很重要，因此强烈建议不要使用ADD从远程URL获取软件包。 您应该使用curl或wget代替。 这样一来，您可以删除不再需要的文件，将它们解压缩后，也不必在镜像中添加其他图层。 例如，您应该避免做以下事情：

```bash
ADD http://example.com/big.tar.xz /usr/src/things/
RUN tar -xJf /usr/src/things/big.tar.xz -C /usr/src/things
RUN make -C /usr/src/things all
```
而应该这样做：

```bash
RUN mkdir -p /usr/src/things \
    && curl -SL http://example.com/big.tar.xz \
    | tar -xJC /usr/src/things \
    && make -C /usr/src/things all
```
对于不需要ADD的tar自动提取功能的其他项目（文件，目录），应始终使用COPY。

7. ENTRYPOINT

ENTRYPOINT的最佳用途是设置镜像的主命令，使该镜像像该命令一样运行（然后使用CMD作为默认标志）。 让我们从命令行工具s3cmd的镜像示例开始：

```bash
ENTRYPOINT ["s3cmd"]
CMD ["--help"]
```
现在可以像这样运行容器以显示命令的帮助：
```bash
$ docker run s3cmd
```
或者使用正确参数来执行命令：

```bash
$ docker run s3cmd ls s3://mybucket
```
这很有用，因为映像名称可以用作对二进制文件的引用，如上面的命令所示。

ENTRYPOINT指令也可以与辅助脚本结合使用，即启动该工具可能需要一个以上的步骤，也可以使其与上述命令类似地工作。

日说说Postgres Official Images使用下面的脚本作为它的ENTRYPOINT指令。

```bash
#!/bin/bash
set -e

if [ "$1" = 'postgres' ]; then
    chown -R postgres "$PGDATA"

    if [ -z "$(ls -A "$PGDATA")" ]; then
        gosu postgres initdb
    fi

    exec gosu postgres "$@"
fi

exec "$@"
```
将应用程序配置为PID 1

该脚本使用exec Bash命令，以便最终运行的应用程序成为容器的PID1。这使应用程序可以接收发送到容器的所有Unix信号。 

helper脚本被复制到容器，然后通过ENTRYPOINT运行
```bash
COPY ./docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["postgres"]
```
该脚本允许用户以多种方式与Postgres进行交互。 它可以简单地启动Postgres：

```bash
$ docker run postgres
```
或者可以传递参数服务器
```bash
$ docker run postgres postgres --help
```
最后，它也可以用于启动一个完全不同的工具，例如Bash
```bash
$ docker run --rm -it postgres bash
```

8. VOLUME

VOLUME指令应用于开放由docker容器创建的任何数据库存储区，配置存储或文件/文件夹。 强烈建议您将VOLUME用于镜像的任何可变和/或用户可维修的部分。

9. USER

如果服务可以在没有特权的情况下运行，请使用USER更改为非root用户。首先在Dockerfile中创建用户和组，例如使用:

```bash
RUN groupadd -r postgres && useradd --no-log-init -r -g postgres postgres
```

10. WORKDIR

为了清楚和可靠，您应该始终为WORKDIR使用绝对路径。另外，您应该使用WORKDIR而不是增加诸如**RUN cd … && do-something**之类的指令，这些指令难以阅读，排除故障和维护。

11. ONBUILD

