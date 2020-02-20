# 浅谈Nginx负载均衡

## 一 特点

### 1.1 应用情况

Nginx做为一个强大的Web服务器软件，具有高性能、高并发性和低内存占用的特点。此外，其也能够提供强大的反向代理功能。俄罗斯大约有超过20%的虚拟主机采用Nginx作为反向代理服务器,在国内也有腾讯、新浪、网易等多家网站在使用Nginx作为反向代理服务器。据Netcraft统计，世界上最繁忙的网站中有11.48%使用Nginx作为其服务器或者代理服务器。基于反向代理的功能，Nginx作为负载均衡主要有以下几点理由：

1. 高并发连接
2. 内存消耗少
3. 配置文件非常简单
4. 成本低廉
5. 支持Rewrite重写规则
6. 内置的健康检查功能
7. 节省带宽
8. 稳定性高

### 1.2 架构

![架构](http://p3.pstatp.com/large/pgc-image/e6e439ca69c9424b8ee323db329ad9c4)

nginx在启动后，会以daemon的方式在后台运行，后台进程包含一个master进程和多个worker进程。工作进程以非特权用户运行。

master进程主要用来管理worker进程，包含：接收来自外界的信号，向各worker进程发送信号，监控worker进程的运行状态，当worker进程退出后(异常情况下)，会自动重新启动新的worker进程。

worker进程则是处理基本的网络事件。多个worker进程之间是对等的，他们同等竞争来自客户端的请求，各进程互相之间是独立的。一个请求，只可能在一个worker进程中处理，一个worker进程，不可能处理其它进程的请求。

- 开发模型：epoll和kqueue。
- 支持的事件机制：kqueue、epoll、rt signals、/dev/poll 、event ports、select以及poll。
- 支持的kqueue特性包括EV_CLEAR、EV_DISABLE、NOTE_LOWAT、EV_EOF，可用数据的数量，错误代码.
- 支持sendfile、sendfile64和sendfilev;文件AIO；DIRECTIO;支持Accept-filters和TCP_DEFER_ACCEP.

### 1.3 性能

Nginx的高并发，官方测试支持5万并发连接。实际生产环境能到2-3万并发连接数。10000个非活跃的HTTP keep-alive 连接仅占用约2.5MB内存。三万并发连接下，10个Nginx进程，消耗内存150M。
>淘宝tengine团队说测试结果是“24G内存机器上，处理并发请求可达200万”。

## 二 负载均衡

### 2.1 协议支持

Nginx工作在网络的7层，可以针对http应用本身来做分流策略。支持七层HTTP、HTTPS协议的负载均衡。对四层协议的支持需要第三方插件-yaoweibin的ngx_tcp_proxy_module实现了tcp upstream。[Link](http://yaoweibin.github.io/nginx_tcp_proxy_module/)

此外，nginx本身也逐渐在完善对其他协议的支持：

nginx 1.4.0对Websocket和SPDY都做了正式的支持。

nginx 1.6.0对SPDY 3.1的正式支持 nginx

1.10.0正式支持HTTP/2

目前，nginx最新稳定版为1.10.2，主线开发版本已经到了1.11.5。Tengine最新版本则继承到了nginx的1.6.2版本。

### 2.2 均衡策略

nginx的负载均衡策略可以划分为两大类：内置策略和扩展策略。内置策略包含加权轮询和ip hash，在默认情况下这两种策略会编译进nginx内核，只需在nginx配置中指明参数即可。扩展策略有很多，如fair、通用hash、consistent hash等，默认不编译进nginx内核。

1. 加权轮询（weighted round robin）

轮询的原理很简单，首先我们介绍一下轮询的基本流程。如下是处理一次请求的流程图：

![加权轮询](http://p1.pstatp.com/large/pgc-image/d7df20edaeda4875aba2fc8a796ecef2)

图中有两点需要注意：
- 第一，如果可以把加权轮询算法分为先深搜索和先广搜索，那么nginx采用的是先深搜索算法，即将首先将请求都分给高权重的机器，直到该机器的权值降到了比其他机器低，才开始将请求分给下一个高权重的机器；
- 第二，当所有后端机器都down掉时，nginx会立即将所有机器的标志位清成初始状态，以避免造成所有的机器都处在timeout的状态，从而导致整个前端被夯住。

2. ip hash

ip hash是nginx内置的另一个负载均衡的策略，流程和轮询很类似，只是其中的算法和具体的策略有些变化，如下图所示：

ip hash算法的核心实现如下：
```
for(i = 0;i < 3;i++){

  hash = (hash * 113 + iphp->addr[i]) % 6271;

}

p = hash % iphp->rrp.peers->number;
```

从代码中可以看出，hash值既与ip有关又与后端机器的数量有关。经过测试，上述算法可以连续产生1045个互异的value，这是该算法的硬限制。
>对此nginx使用了保护机制，当经过20次hash仍然找不到可用的机器时，算法退化成轮询。

因此，从本质上说，ip hash算法是一种变相的轮询算法，如果两个ip的初始hash值恰好相同，那么来自这两个ip的请求将永远落在同一台服务器上，这为均衡性埋下了很深的隐患。

3. fair

fair策略是扩展策略，默认不被编译进nginx内核。其原理是根据后端服务器的响应时间判断负载情况，从中选出负载最轻的机器进行分流。这种策略具有很强的自适应性，但是实际的网络环境往往不是那么简单，因此要慎用。

4. 通用hash、一致性hash

这两种也是扩展策略，在具体的实现上有些差别，通用hash比较简单，可以以nginx内置的变量为key进行hash，一致性hash采用了nginx内置的一致性hash环，可以支持memcache。

5. session_sticky

此种策略就是一次会话内的请求都会落到同一个结点上。在做分布式架构时可以使用，但是**当一个结点挂掉时，会话信息同时也会丢失,如果使用session同步方案同步session信息到所有结点的话代价又会很高，慎重使用此方案。nginx默认不支持此种策略。**

### 2.2 配置示例

1. HTTP

```
upstream upstream_test{

  server 192.168.0.1:8080;

  server 192.168.0.2:8080;

  #ip_hash;

  keepalive 30;

  ## tengine config

  #check interval=300 rise=10 fall=10 timeout=100 type=http port=80;

  #check_http_send "GET / HTTP/1.0

  ";

  #check_http_expect_alive http_2xx http_3xx;

  ## tengine config

  #session_sticky cookie=cookieTest mode=insert;

}

location / {

  proxy_pass http://upstream_test;

  proxy_set_header Host $host;

  proxy_set_header X-Real-IP $remote_addr;

  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

}
```

2. TCP - ngx_tcp_proxy_module

```
tcp {

  upstream cluster {

    # simple round-robin

    server 192.168.0.1:8080;

    server 192.168.0.2:8080;

    check interval=3000 rise=2 fall=5 timeout=1000;

    #check interval=3000 rise=2 fall=5 timeout=1000 type=ssl_hello;

    #check interval=3000 rise=2 fall=5 timeout=1000 type=http;

    #check_http_send "GET / HTTP/1.0

    ";

    #check_http_expect_alive http_2xx http_3xx;

  }

  server {

    listen 8888;

    proxy_pass cluster;
```

## 三 动态负载均衡

### 3.1 自身监控

内置了对后端服务器的**健康检查**功能。如果Nginx proxy后端的某台服务器宕机了，会把返回错误的请求重新提交到另一个节点，不会影响前端访问。它没有独立的健康检查模块，而是使用业务请求作为健康检查，这省去了独立健康检查线程，这是好处。坏处是，当业务复杂时，可能出现误判，例如后端响应超时，这可能是后端宕机，也可能是某个业务请求自身出现问题，跟后端无关。

### 3.2 可扩展性

Nginx属于典型的微内核设计，其内核非常简洁和优雅，同时具有非常高的可扩展性。如下图所示：

![Nginx的微内核设计](http://p1.pstatp.com/large/pgc-image/852910eb5b974c63b1af72f79440f130)

Nginx是纯C语言的实现，其可扩展性在于其模块化的设计。目前，Nginx已经有很多的第三方模块，大大扩展了自身的功能。nginx_lua_module可以将Lua语言嵌入到Nginx配置中，从而利用Lua极大增强了Nginx本身的编程能力，甚至可以不用配合其它脚本语言（如PHP或Python等），只靠Nginx本身就可以实现复杂业务的处理。

### 3.3 配置修改

nginx的配置架构如下图所示：

![配置架构](http://p9.pstatp.com/large/pgc-image/3942b15ef71a40d8acd16a58226fec40)

Nginx支持热部署，几乎可以做到7*24不间断运行，即使运行数个月也不需要重新启动。能够在不间断服务的情况下，对软件版本进行进行升级。Nginx的配置文件非常简单，风格跟程序一样通俗易懂，能够支持perl语法。使用nginx –s reload可以在运行时加载配置文件，便于运行时扩容/减容。重新加载配置时，master进程发送命令给当前正在运行的worker进程worker进程接到命令后会在处理完当前任务后退出。同时，master进程会启动新的worker进程来接管工作。

## 四 优势和劣势

### 4.1 优势

- 可以很好地进行http 的头处理
- 对http协议以及https的良好支持
- 有足够的第三方插件供使用
- 支持热部署，更改后端是平滑的

### 4.2 劣势

- 缺少对session的支持
- 对四层tcp的支持不够好
- post请求写文件系统，造成500 error
- 缺乏主动的后端服务器健康监测
- 默认的监控界面统计信息不全

## 五 Tengine

Tengine是淘宝基于nginx开源代码二次开发一款服务器软件，在继承了nginx的特性以外，提供了一些nginx商业版才有的功能。基本上同步于nginx的更新，目前最新的版本已经继承了nginx 1.6.2稳定版。

### 5.1 特性

tengine的特性包括但不限于：

- 更友好的运维信息显示
- 动态模块加载机制
- 自动根据CPU数目设置进程个数和绑定CPU亲缘性
- 更方便的命令行参数，如列出编译的模块列表、支持的指令等
- 更加强大的负载均衡能力，包括一致性hash模块、会话保持模块，还可以对后端的服务器进行主动健康检查，根据服务器状态自动上线下线
- 动态脚本语言Lua支持。扩展功能非常高效简单
- 输入过滤器机制支持。通过使用这种机制Web应用防火墙的编写更为方便

### 5.2 负载均衡

负载均衡方面，Tengine主要有以下几个特点，基本上弥补了nginx在负载均衡方面的欠缺：

- 支持一致性Hash模块
- 会话保持模块
- 对后端服务器的主动健康检查。
- 增加了请求体不缓存到磁盘的机制

# 高级开发必须掌握

##Nginx负载均衡参数

### 负载均衡

Nginx负载均衡是最常用的Nginx功能，Nginx负载均衡的思想在于通过特定的调度算法将流量分发到不同的应用服务器上面来解决单台机器的并发压力。

最简单的案例
```
upstream backend {
  server backend1.example.com weight=5;
  server backend2.example.com:8080;
  server unix:/tmp/backend3;
  server backup1.example.com:8080 backup;
  server backup2.example.com:8080 backup;
}
server {
  location ^~ /example/ {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header REMOTE-HOST $remote_addr;
    proxy_set_header X-Forwarded-For
    proxy_pass http://backend;
  }
}
```

1. upstream

```
Syntax:	upstream name { … }
Default:	—
Context:	http
```
定义一组可以监听不同端口服务器，也可以监听TCP和UNIX域套接字的服务器。

2. server

```
Syntax:	server address [parameters];
Default:	—
Context:	upstream
```
定义服务器的地址和其他参数。地址可以指定为域名或IP地址、可选端口或在“unix:”前缀之后指定的UNIX域套接字路径。如果未指定端口，则使用端口80。一个解析为多个IP地址的域名等同定义多个服务器。

支持参数如下:

- weight

```
Syntax:	weight=number
```
设置server的权重，默认值1

- max_conns

```
Syntax:	max_conns=number
```
限制与代理服务器同时激活连接的最大数量（1.11.5）。默认值为零，意味着没有限制。如果服务器组不驻留在共享内存中，则每个工作进程的限制都有效。

如果启用了空闲保持活动连接、多个工作者和共享内存，则到代理服务器的活动连接和空闲连接的总数可能超过max_conns值。

- max_fails

```
Syntax:	max_fails=number
```
失败重连的次数，这些尝试应在fail_timeout参数设置的持续时间内发生，以考虑服务器在fail_timeout参数设置的持续时间内不可用。默认情况下，尝试失败的次数设置为1。零值禁用重试。

- fail_timeout

```
Syntax:	fail_timeout=time
```
max_fails次失败后，暂停的时间。默认是10S。

3. backup

其它所有的非backup机器down或者忙的时候，请求backup机器。所以这台机器压力会最轻。

4. down

标记当前的server暂时不参与负载

### 负载均衡模式

1. 轮询（默认）

每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。我们上面的案例使用的就是。

2. weight（权重）

指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。

例如：

```
upstream bakend {
  server 103.1.248.230:8080 weight=10;
  server 104.25.253.107:8080 weight=10;
}
```
3. ip_hash

每个请求按访问ip的hash结果分配，这样每个同IP的访问会固定访问一个后端服务器，可以解决session的问题。

例如：

```
upstream bakend {
  ip_hash;
  server 103.1.248.230:8080;
  server 104.25.253.107:8080;
}
```

4. fair（第三方）

按后端服务器的响应时间来分配请求，响应时间短的优先分配。

```
upstream backend {
  server 103.1.248.230:8080;
  server 104.25.253.107:8080;
  fair;
}
```

5. url_hash（第三方）

按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。

例：在upstream中加入hash语句，server语句中不能写入weight等其他的参数，hash_method是使用的hash算法

```
upstream backend {
  server 103.1.248.230:8080;
  server 104.25.253.107:8080;
  hash $request_uri;
  hash_method crc32;
}
```

## 反向代理、动静分离、负载均衡

### Nginx简述

Nginx是lgor Sysoev为俄罗斯访问量第二的rambler.ru站点设计开发的。从2004年发布至今，凭借开源的力量，已经接近成熟与完善。

Nginx功能丰富，可作为HTTP Web服务器，也可作为反向代理负载均衡服务器，邮件服务器等。支持FastCGI、SSL、Virtual Host、URL Rewrite、Gzip等功能。并且支持很多第三方的模块扩展。

### Nginx 优势功能

作为 Web 服务器：相比 Apache，Nginx 使用更少的资源，支持更多的并发连接，体现更高的效率，这点使 Nginx 尤其受到虚拟主机提供商的欢迎。能够支持高达 50,000 个并发连接数的响应，感谢 Nginx 为我们选择了epoll and kqueue作为开发模型.

作为负载均衡服务器：Nginx 既可以在内部直接支持 Rails 和 PHP，也可以支持作为 HTTP代理服务器 对外进行服务。Nginx 用 C 编写, 不论是系统资源开销还是 CPU 使用效率都比 Perlbal 要好的多。

作为邮件代理服务器: Nginx 同时也是一个非常优秀的邮件代理服务器（最早开发这个产品的目的之一也是作为邮件代理服务器），Last.fm 描述了成功并且美妙的使用经验。

### Tengine

Tengine是由淘宝网发起的Web服务器项目。它在Nginx的基础上，针对大访问量网站的需求，添加了很多高级功能和特性。Tengine的性能和稳定性已经在大型的网站如淘宝网，天猫商城等得到了很好的检验。它的最终目标是打造一个高效、稳定、安全、易用的Web平台。官网。

### 反向代理

要说反向代理，我们就先要理解正向代理。

#### 正向代理

在如今的网络环境下，我们如果由于技术需要要去访问国外的某些网站，此时你会发现位于国外的某网站我们通过浏览器是没有办法访问的，此时大家可能都会翻墙进行访问，翻墙的方式主要是找到一个可以访问国外网站的代理服务器，我们将请求发送给代理服务器，代理服务器去访问国外的网站，然后将访问到的数据传递给我们！

![正向代理](http://p3.pstatp.com/large/pgc-image/40f7eab6c7734e5683002f961c7a0705)

上述这样的代理模式称为正向代理，正向代理最大的特点是**客户端非常明确要访问的服务器地址；最终服务器只清楚请求来自哪个代理服务器，而不清楚来自哪个具体的客户端；正向代理模式屏蔽或者隐藏了真实客户端信息。**


正向代理总结就一句话：代理端代理的是客户端。

#### 反向代理

反向代理（Reverse Proxy）方式是指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。

例如，当用户去某宝购买东西，我们根本不用关心某宝后台具体是怎么配置的，我只知道我访问某宝的代理服务器，代理服务器会代理所有的服务器提供数据给我们。

![反向代理](http://p1.pstatp.com/large/pgc-image/5a006de6e7eb473a8d8a175f217c6df0)

反向代理总结就一句话：代理端代理的是服务端。

### 动静分离

动静分离是让动态网站里的动态网页根据一定规则把不变的资源和经常变的资源区分开来，动静资源做好了拆分以后，我们就可以**根据静态资源的特点将其做缓存操作**，这就是网站静态化处理的核心思路。

动静分离简单的概括是：**动态资源与静态资源的分离**。

在我们的软件开发中，有些请求是后段的动态数据，有些请求是不需要经过后台处理的静态资源（如：css、html、image、js等等文件），这些不需要经过后台处理的资源称为静态资源，否则即是动态资源。

动静分离将网站静态资源（HTML，JavaScript，CSS，img等文件）与后台应用分开部署，提高用户访问静态代码的速度，降低对后台应用访问。例如我们将静态资源放到nginx中，动态资源转发到tomcat服务器中。

### 负载均衡

互联网早期，业务流量比较小并且业务逻辑比较简单，单台服务器便可以满足基本的需求；但随着互联网的发展，业务流量越来越大并且业务逻辑也越来越复杂，单台机器的性能问题以及单点问题凸显了出来，因此需要多台机器来进行性能的水平扩展以及避免单点故障。但是要如何将不同的用户的流量分发到不同的服务器上面呢？

负载均衡的思想是将客户端的流量首先发送扫负载均衡服务器，由负载均衡服务器通过一定的调度算法将流量分发到不同的应用服务器上面来解决单台机器的并发压力。

举例来说，比如某宝网站，每天同时连接到网站的访问人数已经爆表，单个服务器远远不能满足人民日益增长的购买欲望了，此时就需要越来越多的服务器来解决单台机器并发访问人数限制的问题；

![负载均衡](http://p9.pstatp.com/large/pgc-image/5878c41388fb456fb07213d7db56ab1f)

# 负载均衡策略及配置

摘要：本文介绍了Nginx的负载均衡策略，一致性hash分配原理，及常用的故障节点的摘除与恢复配置。

为了增加对负载均衡的好感，我们先了解负载均衡能实现什么。

- 将多个服务器节点绑定在一起提供统一的服务入口。
- 故障转移，在意外发生的时候，可以增加一层保险，减少损失。
- 降低上线运维复杂度，实现平滑上线。运维和开发同学都喜欢。

下面正式进入主题。

## 一、Nginx的负载均衡策略

负载均衡就是将请求“均衡”地分配到多台业务节点服务器上。这里的“均衡”是依据实际场景和业务需要而定的。

对于Nginx来说，请求到达Nginx，Nginx作为反向代理服务器，有绝对的决策权，可以按照规则将请求分配给它知道的节点中的一个，通过这种分配，使得所有节点需要处理的请求量处于相对平均的状态，从而实现负载均衡。

Nginx支持的负载均衡策略很多，比较重点的如下：

- round robin(轮询)
- random(随机)
- weight(权重)
- fair(按响应时长，三方插件)
- url_hash(url的hash值)
- ip_hash(ip的hash值)
- least_conn(最少连接数)

这么多的策略，非常不利于记忆和选择，我们不妨将这些常见的策略归类，分而化之，方便挑选。

### 第一类 最佳实现
- weight(权重)
- random(随机)

最佳实践，其实就是最常见、最普通的默认配置，当然也是在一定程度上最好用的配置。不知道用什么方式的时候，就可以选择用这一类型。

轮询不用多说。这里的随机，其实在大量请求的情况下，按照概率的理论等同于轮询的方式。

1. 轮询配置参考：

```
#默认配置就是轮询策略
upstream server_group {
  server backend1.example.com;
  server backend2.example.com;
}
```
2. 随机配置参考：

```
upstream server_group {
  random;
  server backend1.example.com;
  server backend2.example.com;
  server backend3.example.com;
  server backend4.example.com;
}
```

### 第二类 性能优先
- weight(权重)
- fair(按响应时长，三方插件)
- least_conn(最少连接数)

让业务节点中性能更强的机器得到更多请求，这也是一个比较好的分配策略。

什么是性能更好的机器？这个问题也有很多的维度去考量。

- 从经验或硬件上分为高权重、低权重的机器。
- 按照节点请求的响应时长来决定是多分配请求，还是少分配请求。
- 按照保持的连接数。一般来说保持的连接数越多说明处理的任务越多，也是最繁忙的，可以将请求分配给其他机器处理。

1. 权重的配置参考：

```
upstream server_group {
  server backend1.example.com weight=5;
  #默认为不配置权重为1
  server backend2.example.com;
}
```

响应的时长(fair)配置参考：需要在Nginx编译时加入nginx-upstream-fair模块。

```
upstream server_group{
  fair;
  server backend1.example.com;
  server backend2.example.com;
  server backend3.example.com;
  server backend4.example.com;
}
```

2. 最少连接数(least_conn)配置参考：

```
upstream server_group {
  least_conn;
  server backend1.example.com;
  server backend2.example.com;
}
```

### 第三类 保持稳定
- ip_hash
- url_hash

很多请求都是有状态的，上一次请求到哪个业务节点，这次还要请求到哪台机器。比如常见的session就是这样一种有状态的业务。

这里Nginx提供了按照客户端ip的hash来作为用户的标示分配、url的hash作为分配标示的规则。本质上还是要找到用户的请求中不变的要素，抽离出来，这样就可以进行分配了。

1. ip_hash配置参考：

```
upstream server_group {
  ip_hash;
  server backend1.example.com;
  server backend2.example.com;
}
```

2. url_hash配置参考：

```
upstream server_group{
  hash $request_uri consistent;
  server backend1.example.com;
  server backend2.example.com;
  server backend3.example.com;
  server backend4.example.com;
}
```
## 二、Nginx支持一致性哈希进行分配

Nginx支持一致性hash进行分配，也就是配置中consistent。

什么是一致性hash？为什么要引入这个机制？在生产环境下，业务节点经常会出现增加或减少的情况，就算这种增加或减少都是被动的，也可能会对hash分配产生影响。如何能够做到尽量减少影响呢？这时一致性hash被发明出来。

一致性hash解决两个问题：

- 分配特别不均匀；
- 节点变动除了对分配到这个节点上的请求有影响，还会导致其他节点上的请求重新分配。

1）如何解决分配不均的问题

将原来的每一个节点复制出N个虚拟节点，并且给这些虚拟节点都起个名字。

![分配不均](http://p3.pstatp.com/large/pgc-image/1746cbb3672e40ee923af5ca40be5fc8)

比如原来有5个节点，分配的时候经常会不均匀，现在每个节点都虚拟出N个节点，就是5*N个节点，会极大降低分配不均匀的情况。下面就要说说如何分配的问题了。

2）如何解决节点变动的问题

一致性哈希的基本思想：

- 定义一个[0,(2^32)-1]的数值空间。相当于取长度从 0到2^32-1 的线段。
- 节点映射到线段上。将每个节点，包括虚拟节点，都通过hash算法得到数值，然后映射到这个取值区间上。

如下图。

![一致性Hash](http://p1.pstatp.com/large/pgc-image/13c8472abc644014b4f19748ae8ad94d)


- 计算数据的Hash值。把请求中的关键字符串通过hash算法得到一个数值，在线段中找到一个位置，如果算出来的数值大于2^32-1则认为是0。按照这个规则，其实是把这个线段首尾相连形成一个环，所以也叫hash环。
- 数据节点在线段上找归属的节点。沿着这个线段向右找到离得最近的节点，并把该节点作为这个数据的归属节点。

![一致性hash](http://p1.pstatp.com/large/pgc-image/3a6acd7890b94ce1b64e112021ee31ff)

下面再来看节点的变化对一致性Hash的影响。

- 节点减少：<br>
比如NodeA突然故障了，原来分配到其他节点上的数据不会发生变化，只有分配到NodeA上的数据会重新找离它们最近的点，从而减少了hash重新分配的数量。这也是一致性hash最大的优势。
- 节点增加：<br>
比如现在请求量抖增，需要增加节点降低负载。当新加入节点NodeE时，NodeE及它的一群虚拟节点会根据hash值分配在hash环上。这时，大部分数据再根据一致性哈希规则找其归属的Node节点都不会发生变化，只有那些值计算出来发现离NodeE更近的数据发生了变化，但数量毕竟是有限的，减少了因为节点增加造成的影响。

## 三、故障节点摘除与恢复
先看看经典配置，再详细解释。

```
upstream server_group {
  server backend1.example.com ;
  server backend2.example.com max_fails=3 fail_timeout=30s;
  server backup1.example.com backup;
}
```

### max_fails=number

这个参数决定了多少次请求后端失败后会暂停这个业务节点，不再给它发新的请求，默认值是1。此参数需要配合fail_timeout一起用。

题外话：如何定义失败，有很多种类型，这里因为主要处理HTTP代理，所以更关注proxy_next_upstream。

proxy_next_upstream：主要定义了当服务节点出现状况时，会将请求发给其他节点，也就是定义了怎么算作业务节点失败。

### fail_timeout=time

决定了当Nginx认定这个节点不可用时，暂停多久。不配置默认就是10s。

把上面两个参数联合起来考虑就是：当Nginx发现发送到这个节点上的请求失败了3次的时候，就会把这个节点摘除，摘除时间是30s，30s后才会再次发送请求到这个节点上。

### backup

类似于switch语句中的default，当主要节点都挂了的时候，会把请求打到这个backup节点。这是最后一个救兵了。

## 四、总结

由于Nginx采用了反向代理技术，对于请求的转发有绝对的控制权，使得负载均衡变成了可能。

本文介绍了负载均衡的概念，详细分类了Nginx的负载均衡策略，并提供了简单配置参考。同时介绍了一致性hash的原理，及常用的故障节点的摘除与恢复。下一篇将会介绍Nginx功能之三——HTTP缓存，敬请期待。

# Nginx： 基于TCP/UDP端口的四层负载均衡（stream模块）配置梳理

HTTP负载均衡，也就是我们通常所有"七层负载均衡"，工作在第七层"应用层"。而TCP负载均衡，就是我们通常所说的"四层负载均衡"，工作在"网络层"和"传输层"。例如，LVS（Linux Virtual Server，Linux虚拟服务）和F5（一种硬件负载均衡设备），也是属于"四层负载均衡"

```
nginx-1.9.0 已发布，该版本增加了stream 模块用于一般的TCP 代理和负载均衡，ngx_stream_core_module 这个模块在1.90版本后将被启用。但是并不会默认安装，
需要在编译时通过指定 --with-stream 参数来激活这个模块。

1）配置Nginx编译文件参数
./configure --with-http_stub_status_module --with-stream
------------------------------------------------------------------

2）编译、安装，make && make install
------------------------------------------------------------------

3）配置nginx.conf文件

stream {
  upstream kevin {
    server 192.168.10.10:8080; #这里配置成要访问的地址
    server 192.168.10.20:8081;
    server 192.168.10.30:8081; #需要代理的端口，在这里我代理一一个kevin模块的接口8081
  }
  server {
    listen 8081; #需要监听的端口
    proxy_timeout 20s;
    proxy_pass kevin;
  }
}

创建最高级别的stream（与http同一级别），定义一个upstream组 名称为kevin，由多个服务组成达到负载均衡 定义一个服务用来监听TCP连接（如：8081端口），
并且把他们代理到一个upstream组的kevin中，配置负载均衡的方法和参数为每个server；配置些如：连接数、权重等等。

首先创建一个server组，用来作为TCP负载均衡组。定义一个upstream块在stream上下文中，在这个块里面添加由server命令定义的server，指定他的IP地址和
主机名（能够被解析成多地址的主机名)和端口号。下面的例子是建立一个被称之为kevin组，两个监听1395端口的server ，一个监听8080端口的server。

upstream kevin {
  server 192.168.10.10:8080; #这里配置成要访问的地址
  server 192.168.10.20:8081;
  server 192.168.10.30:8081; #需要代理的端口，在这里我代理一一个kevin模块的接口8081
}

需要特别注意的是：
你不能为每个server定义协议，因为这个stream命令建立TCP作为整个 server的协议了。

配置反向代理使Nginx能够把TCP请求从一个客户端转发到负载均衡组中(如：kevin组)。在每个server配置块中 通过每个虚拟server的server的配置信息和在
每个server中定义的监听端口（客户端需求的代理端口号，如我推流的的是kevin协议，则端口号为：8081）的配置信息和proxy_passs 命令把TCP通信发送到
upstream的哪个server中去。下面我们将TCP通信发送到kevin 组中去。

server {
  listen 8081; #需要监听的端口
  proxy_timeout 20s;
  proxy_pass kevin;
}

当然我们也可以采用单一的代理方式：

server {
  listen 8081; #需要监听的端口
  proxy_timeout 20s;
  proxy_pass 192.168.10.30:8081; #需要代理的端口，在这里我代理一一个kevin模块的接口8081
}
------------------------------------------------------------------

4）改变负载均衡的方法：
默认nginx是通过轮询算法来进行负载均衡的通信的。引导这个请求循环的到配置在upstream组中server端口上去。 因为他是默认的方法，这里没有轮询命令，
只是简单的创建一个upstream配置组在这儿stream山下文中，而且在其中添加server。

a）least-connected ：对于每个请求，nginx plus选择当前连接数最少的server来处理:

upstream kevin {
  least_conn;
  server 192.168.10.10:8080; #这里配置成要访问的地址
  server 192.168.10.20:8081;
  server 192.168.10.30:8081; #需要代理的端口，在这里我代理一一个kevin模块的接口8081
}

b）least time ：对于每个链接,nginx pluns 通过几点来选择server的： 最底平均延时：通过包含在least_time命令中指定的参数计算出来的：
connect：连接到一个server所花的时间
first_byte：接收到第一个字节的时间
last_byte：全部接收完了的时间 最少活跃的连接数:

upstream kevin {
  least_time first_byte;
  server 192.168.10.10:8080; #这里配置成要访问的地址
  server 192.168.10.20:8081;
  server 192.168.10.30:8081; #需要代理的端口，在这里我代理一一个kevin模块的接口8081
}

c）普通的hash算法：nginx plus选择这个server是通过user_defined 关键字，就是IP地址：$remote_addr;

upstream kevin {
  hash $remote_addr consistent;
  server 192.168.10.10:8080 weight=5; #这里配置成要访问的地址
  server 192.168.10.20:8081 max_fails=2 fail_timeout=30s;
  server 192.168.10.30:8081 max_conns=3; #需要代理的端口，在这里我代理一一个kevin模块的接口8081
}
```
![负载均衡示意图](http://p9.pstatp.com/large/pgc-image/a6abdee07f7c4d3f88fbf6731e67d21b)

## Nginx的TCP负载均衡的执行原理

当Nginx从监听端口收到一个新的客户端链接时，立刻执行路由调度算法，获得指定需要连接的服务IP，然后创建一个新的上游连接，连接到指定服务器。

![负载均衡工作流示意图](http://p3.pstatp.com/large/pgc-image/9c9f645fc8634e35bf6fdb890ad75d9f)

TCP负载均衡支持Nginx原有的调度算法，包括Round Robin（默认，轮询调度），哈希（选择一致）等。同时，调度信息数据也会和健壮性检测模块一起协作，为每个连接选择适当的目标上游服务器。如果使用Hash负载均衡的调度方法，你可以使用$remote_addr（客户端IP）来达成简单持久化会话（同一个客户端IP的连接，总是落到同一个服务server上）。

和其他upstream模块一样，TCP的stream模块也支持自定义负载均和的转发权重（配置“weight=2”），还有backup和down的参数，用于踢掉失效的上游服务器。max_conns参数可以限制一台服务器的TCP连接数量，根据服务器的容量来设置恰当的配置数值，尤其在高并发的场景下，可以达到过载保护的目的。

Nginx监控客户端连接和上游连接，一旦接收到数据，则Nginx会立刻读取并且推送到上游连接，不会做TCP连接内的数据检测。Nginx维护一份内存缓冲区，用于客户端和上游数据的写入。如果客户端或者服务端传输了量很大的数据，缓冲区会适当增加内存的大小。

![Nginx缓存](http://p1.pstatp.com/large/pgc-image/cc0a4f79d70c414aabf1345a347f75a5)

当Nginx收到任意一方的关闭连接通知，或者TCP连接被闲置超过了proxy_timeout配置的时间，连接将会被关闭。对于TCP长连接，我们更应该选择适当的proxy_timeout的时间，同时，关注监听socke的so_keepalive参数，防止过早地断开连接。

## Nginx的TCP负载均衡服务健壮性监控

TCP负载均衡模块支持内置健壮性检测，一台上游服务器如果拒绝TCP连接超过proxy_connect_timeout配置的时间，将会被认为已经失效。在这种情况下，Nginx立刻尝试连接upstream组内的另一台正常的服务器。连接失败信息将会记录到Nginx的错误日志中。

![失败处理](http://p3.pstatp.com/large/pgc-image/e3c44bf266b6406c8d9c4a3abb986954)

如果一台服务器，反复失败（超过了max_fails或者fail_timeout配置的参数），Nginx也会踢掉这台服务器。服务器被踢掉60秒后，Nginx会偶尔尝试重连它，检测它是否恢复正常。如果服务器恢复正常，Nginx将它加回到upstream组内，缓慢加大连接请求的比例。

之所"缓慢加大"，因为通常一个服务都有"热点数据"，也就是说，80%以上甚至更多的请求，实际都会被阻挡在"热点数据缓存"中，真正执行处理的请求只有很少的一部分。在机器刚刚启动的时候，"热点数据缓存"实际上还没有建立，这个时候爆发性地转发大量请求过来，很可能导致机器无法"承受"而再次挂掉。以mysql为例子，我们的mysql查询，通常95%以上都是落在了内存cache中，真正执行查询的并不多。

其实，无论是单台机器或者一个集群，在高并发请求场景下，重启或者切换，都存在这个风险，解决的途径主要是两种：

1）请求逐步增加，从少到多，逐步积累热点数据，最终达到正常服务状态。

2）提前准备好"常用"的数据，主动对服务做"预热"，预热完成之后，再开放服务器的访问。

TCP负载均衡原理上和LVS等是一致的，工作在更为底层，性能会高于原来HTTP负载均衡不少。但是，不会比LVS更为出色，LVS被置于内核模块，而Nginx工作在用户态，而且，Nginx相对比较重。另外一点，令人感到非常可惜，这个模块竟然是个付费功能。

# Nginx负载均衡配置实例详解

负载均衡是我们大流量网站要做的一个东西，下面我来给大家介绍在Nginx服务器上进行负载均衡配置方法，希望对有需要的同学有所帮助哦。

1. 测试环境

由于没有服务器，所以本次测试直接host指定域名，然后在VMware里安装了三台CentOS。

```
测试域名 ：a.com

A服务器IP ：192.168.5.149 （主）

B服务器IP ：192.168.5.27

C服务器IP ：192.168.5.126
```

2. 部署思路

A服务器做为主服务器，域名直接解析到A服务器（192.168.5.149）上，由A服务器负载均衡到B服务器（192.168.5.27）与C服务器（192.168.5.126）上。

3. 域名解析

由于不是真实环境，域名就随便使用一个a.com用作测试，所以a.com的解析只能在hosts文件设置。

打开：C:\\Windows\\System32\\drivers\\etc\\hosts
在末尾添加

```
192.168.5.149 a.com
```
保存退出，然后启动命令模式ping下看看是否已设置成功


- A 服务器nginx.conf设置

打开nginx.conf，文件位置在nginx安装目录的conf目录下。

在http段加入以下代码

```
upstream a.com {

  server 192.168.5.126:80;

  server 192.168.5.27:80;

}

server{

  listen 80;

  server_name a.com;

  location / {

    proxy_pass http://a.com;

    proxy_set_header Host $host;

    proxy_set_header X-Real-IP $remote_addr;

    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

  }

}
```

保存重启nginx

- B、C服务器nginx.conf设置

打开nginx.confi，在http段加入以下代码

```
server{

  listen 80;

  server_name a.com;

  index index.html;

  root /data0/htdocs/www;

}
```

保存重启nginx

4. 测试

当访问a.com的时候，为了区分是转向哪台服务器处理我分别在B、C服务器下写一个不同内容的index.html文件，以作区分。

打开浏览器访问a.com结果，刷新会发现所有的请求均分别被主服务器（192.168.5.149）分配到B服务器（192.168.5.27）与C服务器（192.168.5.126）上，实现了负载均衡效果。

假如其中一台服务器宕机会怎样？

当某台服务器宕机了，是否会影响访问呢？

我们先来看看实例，根据以上例子，假设C服务器192.168.5.126这台机子宕机了（由于无法模拟宕机，所以我就把C服务器关机）然后再来访问看看。

访问结果：

我们发现，虽然C服务器（192.168.5.126）宕机了，但不影响网站访问。这样，就不会担心在负载均衡模式下因为某台机子宕机而拖累整个站点了。

如果b.com也要设置负载均衡怎么办？

很简单，跟a.com设置一样。如下：

假设b.com的主服务器IP是192.168.5.149，负载均衡到192.168.5.150和192.168.5.151机器上

现将域名b.com解析到192.168.5.149IP上。

在主服务器(192.168.5.149)的nginx.conf加入以下代码：

```
upstream b.com {

  server 192.168.5.150:80;

  server 192.168.5.151:80;

}

server{

  listen 80;

  server_name b.com;

  location / {

    proxy_pass http://b.com;

    proxy_set_header Host $host;

    proxy_set_header X-Real-IP $remote_addr;

    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

  }

}
```

保存重启nginx

在192.168.5.150与192.168.5.151机器上设置nginx，打开nginx.conf在末尾添加以下代码：

```
server{

  listen 80;

  server_name b.com;

  index index.html;

  root /data0/htdocs/www;

}
```

保存重启nginx

完成以后步骤后即可实现b.com的负载均衡配置。

主服务器不能提供服务吗？

以上例子中，我们都是应用到了主服务器负载均衡到其它服务器上，那么主服务器本身能不能也加在服务器列表中，这样就不会白白浪费拿一台服务器纯当做转发功能，而是也参与到提供服务中来。

如以上案例三台服务器：

```
A服务器IP ：192.168.5.149 （主）

B服务器IP ：192.168.5.27

C服务器IP ：192.168.5.126
```

我们把域名解析到A服务器，然后由A服务器转发到B服务器与C服务器，那么A服务器只做一个转发功能，现在我们让A服务器也提供站点服务。

我们先来分析一下，如果添加主服务器到upstream中，那么可能会有以下两种情况发生：

1、主服务器转发到了其它IP上，其它IP服务器正常处理；

2、主服务器转发到了自己IP上，然后又进到主服务器分配IP那里，假如一直分配到本机，则会造成一个死循环。

怎么解决这个问题呢？因为80端口已经用来监听负载均衡的处理，那么本服务器上就不能再使用80端口来处理a.com的访问请求，得用一个新的。于是我们把主服务器的nginx.conf加入以下一段代码：

```
server{

  listen 8080;

  server_name a.com;

  index index.html;

  root /data0/htdocs/www;

}
```

重启nginx，在浏览器输入a.com:8080试试看能不能访问。结果可以正常访问

既然能正常访问，那么我们就可以把主服务器添加到upstream中，但是端口要改一下，如下代码：

```
upstream a.com {

  server 192.168.5.126:80;

  server 192.168.5.27:80;

  server 127.0.0.1:8080;

}
```

由于这里可以添加主服务器IP192.168.5.149或者127.0.0.1均可以，都表示访问自己。

重启Nginx，然后再来访问a.com看看会不会分配到主服务器上。

主服务器也能正常加入服务了。

最后

一、负载均衡不是nginx独有，著名鼎鼎的apache也有，但性能可能不如nginx。

二、多台服务器提供服务，但域名只解析到主服务器，而真正的服务器IP不会被ping下即可获得，增加一定安全性。

三、upstream里的IP不一定是内网，外网IP也可以。不过经典的案例是，局域网中某台IP暴露在外网下，域名直接解析到此IP。然后又这台主服务器转发到内网服务器IP中。

四、某台服务器宕机、不会影响网站正常运行，Nginx不会把请求转发到已宕机的IP上

# Nginx: 搭建简易负载均衡服务

1. 什么是负载均衡

当一台服务器的性能达到极限时，我们可以使用服务器集群来提高网站的整体性能。那么，在服务器集群中，需要有一台服务器充当调度者的角色，用户的所有请求都会首先由它接收，调度者再根据每台服务器的负载情况将请求分配给某一台后端服务器去处理。

那么在这个过程中，调度者如何合理分配任务，保证所有后端服务器都将性能充分发挥，从而保持服务器集群的整体性能最优，这就是负载均衡问题。

2. 基本环境

三台centos虚拟机(windows也可以)，都搭建好了nginx环境。当然也可以使用docker映射三个不同端口的nginx容器进行。centos使用docker搭建nginx环境方法：《使用docker快速搭建nginx+php环境》

三台主机分别是：

```
192.168.73.128 主

192.168.73.130

192.168.73.131
```

首先我们解析个域名codehui.net到主服务器192.168.73.128上

192.168.73.128 codehui.net
3. 步骤

- 主服务器配置

编辑nginx配置文件

```
vi /etc/nginx/nginx.conf
```
在http节点中加入下面配置，负载均衡的几种常用方式请查看文尾。

```
# 负载均衡服务器列表
upstream codehui.net {
  server 192.168.73.130:80;
  server 192.168.73.131:80;
}

# 监听80端口的访问
server{
  listen 80;
  server_name codehui.net;
  location / {
    proxy_pass http://codehui.net;
  }
}
```

- 其他两台服务器配置

我们分别在两台服务器上新建个入口文件，作为区分

```
echo "<?php echo "我是一号服务器:192.168.73.130";" > index.php

echo "<?php echo "我是二号服务器:192.168.73.131";" > index.php
```

如果要配置其他功能，可以在nginx.conf中加入如下方法

```
server{
  listen 80;
  server_name codehui.net;
  root /usr/share/nginx/www;
  location / {
    index index.php index.html;
  }
}
```
- 注意重启nginx服务

```
service nginx reload
```
4. 测试

可以看到两次请求分发到了不同服务器

![代码汇1](http://p1.pstatp.com/large/pgc-image/934f271ec8504986871528204641d514)


![代码汇2](http://p3.pstatp.com/large/pgc-image/410a5fa67cd44f3885e33f604cd10297)


## 负载均衡的几种常用方式

1、 轮询(默认)

每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。

```
upstream nginx_server {
  server server1;
  server server1;
}
```
2、weight

指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。

```
upstream nginx_server {
  server server1 weight=4;
  server server2 weight=6;
}
```
权重越高，在被访问的概率越大，如上例，分别是40%，60%。

3、 ip_hash

上述方式存在一个问题就是说，在负载均衡系统中，假如用户在某台服务器上登录了，那么该用户第二次请求的时候，因为我们是负载均衡系统，每次请求都会重新定位到服务器集群中的某一个，那么已经登录某一个服务器的用户再重新定位到另一个服务器，其登录信息将会丢失，这样显然是不妥的。

我们可以采用ip_hash指令解决这个问题，如果客户已经访问了某个服务器，当用户再次访问时，会将该请求通过哈希算法，自动定位到该服务器。

每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。

```
upstream nginx_server {
  ip_hash;
  server server1;
  server server2;
}
```

4、 fair (第三方)

```
upstream nginx_server {
  server server1;
  server server2;
  fair;
}
```

5、url_hash (第三方)

按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。

```
upstream backserver {
  server squid1:3128;
  server squid2:3128;
  hash $request_uri;
  hash_method crc32;
}
```

每个设备的状态设置为:

- down 表示单前的server暂时不参与负载
- weight 默认为1.weight越大，负载的权重就越大
- max_fails：允许请求失败的次数默认为1.当超过最大次数时，回proxy_next_upstream模块定义的错误
- fail_timeout : max_fails次失败后，暂停的时间
- backup： 其它所有的非backup机器down或者忙的时候，请求backup机器。所以这台机器压力会最轻。

配置实例：


```
#user nobody;
worker_processes 4;
events {
  # 最大并发数
  worker_connections 1024;
}
http{
  # 待选服务器列表
  upstream myproject{
    # ip_hash指令，将同一用户引入同一服务器。
    ip_hash;
    server 125.219.42.4 fail_timeout=60s;
    server 172.31.2.183;
  }
  server{
    # 监听端口
    listen 80;
    # 根目录下
    location / {
      # 选择哪个服务器列表
      proxy_pass http://myproject;
    }
  }
}
```
