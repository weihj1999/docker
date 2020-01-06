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
如果需要写一个启动脚本来做为单一的可执行程序，需要确保可执行程序能够接受UNIX signal，通过使用exec和gosu命令：

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
最后，如果您需要在关闭时进行一些额外的清理（或与其他容器通信），或协调多个可执行文件，则可能需要确保ENTRYPOINT脚本接收Unix信号，然后将其传递，然后 还有更多工作：
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
