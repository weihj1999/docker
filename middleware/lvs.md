# LVS三种负载均衡方式比较

## 1、什么是LVS？

首先简单介绍一下LVS (Linux Virtual Server)到底是什么东西，其实它是一种集群(Cluster)技术，**采用IP负载均衡技术和基于内容请求分发技术**。**调度器具有很好的吞吐率**，将请求均衡地转移到不同的服务器上执行，且调度器自动屏蔽掉服务器的故障，从而将一组服务器构成一个高性能的、高可用的虚拟服务器。整个服务器集群的结构对客户是透明的，而且无需修改客户端和服务器端的程序。

为此，在设计时需要考虑系统的透明性、可伸缩性、高可用性和易管理性。一般来说，LVS集群采用三层结构，其体系结构如图所示：

![体系架构](http://p3.pstatp.com/large/pgc-image/ffb649b7f13d4210a7859f0c9b06729d)

## 2、LVS主要组成部分为：

负载调度器（load balancer/ Director），它是整个集群对外面的前端机，负责将客户的请求发送到一组服务器上执行，而客户认为服务是来自一个IP地址（我们可称之为虚拟IP地址）上的。

服务器池（server pool/ Realserver），是一组真正执行客户请求的服务器，执行的服务一般有WEB、MAIL、FTP和DNS等。

共享存储（shared storage），它为服务器池提供一个共享的存储区，这样很容易使得服务器池拥有相同的内容，提供相同的服务。

## 3、LVS负载均衡方式:

### 3.1  Virtual Server via Network Address Translation NAT（VS/NAT）

VS/NAT是一种最简单的方式，所有的RealServer只需要将自己的网关指向Director即可。客户端可以是任意操作系统，但此方式下，一个Director能够带动的RealServer比较有限。在VS/NAT的方式下，Director也可以兼为一台RealServer。VS/NAT的体系结构如图所示。

![VS/NAT架构](http://p9.pstatp.com/large/pgc-image/42ec0dfcd6ee4720bd197816d16b7def)


VS/NAT的体系结构

### 3.2 Virtual Server via IP Tunneling(VS/TUN)

IP隧道（IP tunneling）是将一个IP报文封装在另一个IP报文的技术，这可以使得目标为一个IP地址的数据报文能被封装和转发到另一个IP地址。IP隧道技术亦称为IP封装技术（IP encapsulation）。IP隧道主要用于移动主机和虚拟私有网络（Virtual Private Network），在其中隧道都是静态建立的，隧道一端有一个IP地址，另一端也有唯一的IP地址。

它的连接调度和管理与VS/NAT中的一样，只是它的报文转发方法不同。调度器根据各个服务器的负载情况，动态地选择一台服务器，将请求报文封装在另一个IP报文中，再将封装后的IP报文转发给选出的服务器；服务器收到报文后，先将报文解封获得原来目标地址为 VIP 的报文，服务器发现VIP地址被配置在本地的IP隧道设备上，所以就处理这个请求，然后根据路由表将响应报文直接返回给客户。

![VS/TUN的体系结构](http://p3.pstatp.com/large/pgc-image/eda2b7f55e0e4147a7689186e8c129a5)

VS/TUN的工作流程：

![VS/TUN的工作流程](http://p3.pstatp.com/large/pgc-image/8e88f207babe42a6ba3bfc9637afabdf)


### 3.3 Virtual Server via Direct Routing(VS/DR)

VS/DR方式是通过改写请求报文中的MAC地址部分来实现的。Director和RealServer必需在物理上有一个网卡通过不间断的局域网相连。 RealServer上绑定的VIP配置在各自Non-ARP的网络设备上(如lo或tunl),Director的VIP地址对外可见，而RealServer的VIP对外是不可见的。RealServer的地址即可以是内部地址，也可以是真实地址。

![VS/DR的体系结构](http://p1.pstatp.com/large/pgc-image/9e4dfac55aa042ebbc0f017a5ed37b30)

VS/DR的工作流程：

VS/DR的工作流程如图所示：它的连接调度和管理与VS/NAT和VS/TUN中的一样，它的报文转发方法又有不同，将报文直接路由给目标服务器。在VS/DR中，调度器根据各个服务器的负载情况，动态地选择一台服务器，不修改也不封装IP报文，而是将数据帧的MAC地址改为选出服务器的MAC地址，再将修改后的数据帧在与服务器组的局域网上发送。因为数据帧的MAC地址是选出的服务器，所以服务器肯定可以收到这个数据帧，从中可以获得该IP报文。当服务器发现报文的目标地址VIP是在本地的网络设备上，服务器处理这个报文，然后根据路由表将响应报文直接返回给客户。

![VS/DR的工作流程](http://p3.pstatp.com/large/pgc-image/8e88f207babe42a6ba3bfc9637afabdf)


## 4、三种负载均衡方式比较：

### 4.1 Virtual Server via NAT

VS/NAT 的优点是服务器可以运行任何支持TCP/IP的操作系统，它只需要一个IP地址配置在调度器上，服务器组可以用私有的IP地址。缺点是它的伸缩能力有限，当服务器结点数目升到20时，调度器本身有可能成为系统的新瓶颈，因为在VS/NAT中请求和响应报文都需要通过负载调度器。我们在Pentium166 处理器的主机上测得重写报文的平均延时为60us，性能更高的处理器上延时会短一些。假设TCP报文的平均长度为536 Bytes，则调度器的最大吞吐量为8.93 MBytes/s. 我们再假设每台服务器的吞吐量为800KBytes/s，这样一个调度器可以带动10台服务器。（注：这是很早以前测得的数据）

基于 VS/NAT的的集群系统可以适合许多服务器的性能要求。如果负载调度器成为系统新的瓶颈，可以有三种方法解决这个问题：混合方法、VS/TUN和 VS/DR。在DNS混合集群系统中，有若干个VS/NAT负调度器，每个负载调度器带自己的服务器集群，同时这些负载调度器又通过RR-DNS组成简单的域名。

但VS/TUN和VS/DR是提高系统吞吐量的更好方法。

对于那些将IP地址或者端口号在报文数据中传送的网络服务，需要编写相应的应用模块来转换报文数据中的IP地址或者端口号。这会带来实现的工作量，同时应用模块检查报文的开销会降低系统的吞吐率。

### 4.2 Virtual Server via IP Tunneling

在VS/TUN 的集群系统中，负载调度器只将请求调度到不同的后端服务器，后端服务器将应答的数据直接返回给用户。这样，负载调度器就可以处理大量的请求，它甚至可以调度百台以上的服务器（同等规模的服务器），而它不会成为系统的瓶颈。即使负载调度器只有100Mbps的全双工网卡，整个系统的最大吞吐量可超过 1Gbps。所以，VS/TUN可以极大地增加负载调度器调度的服务器数量。VS/TUN调度器可以调度上百台服务器，而它本身不会成为系统的瓶颈，可以用来构建高性能的超级服务器。VS/TUN技术对服务器有要求，即所有的服务器必须支持“IP Tunneling”或者“IP Encapsulation”协议。目前，VS/TUN的后端服务器主要运行Linux操作系统，我们没对其他操作系统进行测试。因为“IP Tunneling”正成为各个操作系统的标准协议，所以VS/TUN应该会适用运行其他操作系统的后端服务器。

### 4.3 Virtual Server via Direct Routing

跟VS/TUN方法一样，VS/DR调度器只处理客户到服务器端的连接，响应数据可以直接从独立的网络路由返回给客户。这可以极大地提高LVS集群系统的伸缩性。跟VS/TUN相比，这种方法没有IP隧道的开销，但是要求负载调度器与实际服务器都有一块网卡连在同一物理网段上，服务器网络设备（或者设备别名）不作ARP响应，或者能将报文重定向（Redirect）到本地的Socket端口上。

三种LVS负载均衡技术的优缺点归纳以下表：

![对比](http://p9.pstatp.com/large/pgc-image/8a5805c563ae4dfeb83fcdd1b8653b0e)


注：以上三种方法所能支持最大服务器数目的估计是假设调度器使用100M网卡，调度器的硬件配置与后端服务器的硬件配置相同，而且是对一般Web服务。使 用更高的硬件配置（如千兆网卡和更快的处理器）作为调度器，调度器所能调度的服务器数量会相应增加。当应用不同时，服务器的数目也会相应地改变。所以，以上数据估计主要是为三种方法的伸缩性进行量化比较。

## 5、lvs的负载调度算法 在内核中的连接调度算法上，IPVS已实现了以下八种调度算法：

### 5.1 一 轮叫调度（Round­Robin Schedul ing ）

轮叫调度（Round Robin Scheduling）算法就是以轮叫的方式依次将请求调度不同的服务器，即每次调度执行i=(i+1)mod n，并选出第i台服务器。算法的优点是其简洁性，它无需记录当前所有连接的状态，所以它是一种无状态调度。

### 5.2 二 加权轮叫调度（Weighted Round­Robin Scheduling ）

加权轮叫调度 （Weighted Round­Robin Scheduling）算法可以解决服务器间性能不一的情况，它用相应的权值表示服务器的处理性能，服务器的缺省权值为1。假设服务器A的权值为1，B的权值为2，则表示服务器B的处理性能是A的两倍。

加权轮叫调度算法是按权值的高 低和轮叫方式分配请求到各服务器。权值高的服务器先收到的连接，权值高的服 务器比权值低的服务器处理更多的连接，相同权值的服务器处理相同数目的连接数。

### 5.3 三 最小连接调度（Least­Connect ion Schedul ing ）

最小连接调度（Least­ Connect ion Scheduling）算法是把新的连接请求分配到当前连接数最小的服务器。最小连接调度是一种动态调度算法，它通过服务器当前所活跃的连接数来估计服务器的负载情况。调度器需要记录各个服务器已建立连接的数目，当一个请求被调度到某台服务器，其连接数加1；当连接中止或超时，其连接数减一。

### 5.4 四 加权最小连接调度（Weighted Least­Connectio n Scheduling）

加权最小连接调 度（Weighted Least­Connectio n Scheduling）算法是最小连接调度的超集，各个服务器用相应的权值表示其处理性能。服务器的缺省权值为1，系统管理员可以动态地设置服务器的权值。加权最小连接调度在调度新连接时尽可能使服务器的已建立连接数和其权值成比例。

### 5.5 五 基于局部性的最少链接（Locality­Based Least Connections Schedulin g ）

基于局部性的最少链接调度（Locality­Based Least Connections Scheduling，以下简称为LBLC）算法是针对请求报文的目标IP地址的负载均衡调度，目前主要用于Cache集群系统，因为在Cache集群中客户请求报文的目标IP地址是变化的。这里假设任何后端服务器都可以处理任一请求，算法的设计目标是在服务器的负载基本平衡情况下，将相同目标IP地址的请求调度到同一台服务器，来提高各台服务器的访问局部性和主存Cache命中率，从而整个集群系统的处理能力。

LBLC调度算法先根据请求的目标IP 地址 找出该目标IP地址最近使用的服务器，若该服务器是可用的且没有超载，将请求发送到该服务器；若服务器不存在，或者该服务器超载且有服务器处于其一半的工 作负载，则用“最少链接”的原则选出一个可用的服务器，将请求发送到该服务器。

### 5.6 六 带复制的基于局部性最少链接（Locality­Based Least Connectio ns with Replication Scheduling）

带复制的基于局部性最少链接调度（Locality­Based Least Connectio ns with Replication Scheduling，以下简称为LBLCR）算法也是针对目标IP地址的负载均衡，目前主要用于Cache集群系统。

它与LBLC算法的不同之处是它要 维护从一个目标IP地址到一组服务器的映射，而LBLC算法维护从一个目标IP地址到一台服务器的映射。对于一个“热门”站点的服务请求，一台Cache 服务器可能会忙不过来处理这些请求。这时，LBLC调度算法会从所有的Cache服务器中按“最小连接”原则选出一台Cache服务器，映射该“热门”站点到这台Cache服务器，很快这台Cache服务器也会超载，就会重复上述过程选出新的Cache服务器。

这样，可能会导致该“热门”站点的映像会出现 在所有的Cache服务器上，降低了Cache服务器的使用效率。LBLCR调度算法将“门站”点映射到一组Cache服务器（服务器集合），当该“热门”站点的请求负载增加时，会增加集合里的Cache服务器，来处理不断增长的负载；当该“热门”站点的请求负载降低时，会减少集合里的Cache服务器 数目。这样，该热门站点的映像不可能出现在所有的Cache服务器上，从而提供Cache集群系统的使用效率。

LBLCR算法先根据请求的目标IP 地址找出该目标IP地址对应的服务器组；按“最小连接”原则从该服务器组中选出一台服务器，若服务器没有超载，将请求发送到该服务器；若服务器超载；则按“最小连接”原则从整个集群中选出一台服务器，将该服务器加入到服务器组中，将请求发送到该服务器。同时，当该服务器组有一段时间没有被修改，将最忙的服 务器从服务器组中删除，以降低复制的程度。

### 5.7 七 目标地址散列调度（Destinat ion Hashing Scheduling ）

目标地址散列调度 （Destinat ion Hashing Scheduling）算法也是针对目标IP地址的负载均衡，但它是一种静态映射算法，通过一个散列（Hash）函数将一个目标IP地址映射到一台服务器。目标地址散列调度算法先根据请求的目标IP地址，作为散列键（Hash Key）从静态分配的散列表找出对应的服务器，若该服务器是可用的且未超载，将请求发送到该服务器，否则返回空。

### 5.8 八 源地址散列调度（Source Hashing Scheduling）

源地址散列调度（Source Hashing Scheduling）算法正好与目标地址散列调度算法相反，它根据请求的源IP地址，作为散列键（Hash Key）从静态分配的散列表找出对应的服务器，若该服务器是可用的且未超载，将请求发送到该服务器，否则返回空。它采用的散列函数与目标地址散列调度算法 的相同。它的算法流程与目标地址散列调度算法的基本相似，除了将请求的目标IP地址换成请求的源IP 地址，所以这里不一一叙述。在实际应用中，源地址散列 调度和目标地址散列调度可以结合使用在防火墙集群中，它们可以保证整个系统的唯一出入口。

# 一个讲座

https://www.ixigua.com/i6795316767129338382/

https://www.ixigua.com/i6795316809756049933/

https://www.ixigua.com/i6795316778449764875/

https://www.ixigua.com/i6795316772703568395/


# 配置实战

https://www.toutiao.com/a6576405597216834056/

后续收藏，LVS里面的重点概念解释的比较清楚。

# 主从热备+负载均衡（LVS + keepalived）图解

## 1、环境准备

本机 + virtualBox + 4台centOs虚拟机，如下图

![虚拟机准备](http://p9.pstatp.com/large/pgc-image/285c1645992c41d0a7413534d1e4f858)

virtualBox安装以及CentOS安装这里就不再演示，大家自行搭建；本机在本次试验中扮演的角色就是客户端，起到一个发送请求的作用，两台CentOS做负载均衡服务器（一台为主机，一台为备机），另外两台作为真实的Web服务器（安装有tomcat）。

本次实验基于DR负载均衡模式（直接路由），设置一个VIP（Virtual IP）为192.168.1.200，用户只需要访问这个IP地址即可获得网页服务。其中，负载均衡主机为192.168.1.114（master），备机为 192.168.1.112（brucelee）。Web服务器A为192.168.1.111（youzhibing），Web服务器B为192.168.1.115（youzhibing03），四台机器命名除了master外都不太规范，但不影响实验。四台CentOS的防火墙都需要关闭。利用Xshell链接CentOS，如下图

![虚拟机细节](http://p3.pstatp.com/large/pgc-image/1873414ac2b447ee9b2d32b874a7156c)

## 2、配置两台web服务器

和本地部署web项目一样，将myWeb部署到tomcat中，开启tomcat，宿主机访问(virtualBox安装linux，并搭建tomcat请点这)，如下图

![webserver1](http://p3.pstatp.com/large/pgc-image/aed3e6f5695d4a238a77692703682b99)
![webserver1](http://p1.pstatp.com/large/pgc-image/c7994ab51151459e9d081ee2f2aaecb3)
![RealServer配置](http://p9.pstatp.com/large/pgc-image/863c60b972874bf7be439e7d839c19fe)

/etc/init.d/realserver 内容如下

```
#vi /usr/local/sbin/realserver.sh
#!/bin/bash
# description: Config realserver lo and apply noarp
#Written by :NetSeek http://www.linuxtone.org
SNS_VIP=192.168.1.200
. /etc/rc.d/init.d/functions
case "$1" in
start)
ifconfig lo:0 $SNS_VIP netmask 255.255.255.255 broadcast $SNS_VIP
/sbin/route add -host $SNS_VIP dev lo:0
echo "1" >/proc/sys/net/ipv4/conf/lo/arp_ignore
echo "2" >/proc/sys/net/ipv4/conf/lo/arp_announce
echo "1" >/proc/sys/net/ipv4/conf/all/arp_ignore
echo "2" >/proc/sys/net/ipv4/conf/all/arp_announce
sysctl -p >/dev/null 2>&1
echo "RealServer Start OK"
;;
stop)
ifconfig lo:0 down
route del $SNS_VIP >/dev/null 2>&1
echo "0" >/proc/sys/net/ipv4/conf/lo/arp_ignore
echo "0" >/proc/sys/net/ipv4/conf/lo/arp_announce
echo "0" >/proc/sys/net/ipv4/conf/all/arp_ignore
echo "0" >/proc/sys/net/ipv4/conf/all/arp_announce
echo "RealServer Stoped"
;;
*)
echo "Usage: $0 {start|stop}"
exit 1
esac
exit 0
```
保存脚本文件后更改该文件权限：chmod 755 realserver，开启realserver服务：

```
service realserver start；
```

注意：配置real server是web服务器都需要配置的，有多少台就配置多少台！

## 3、配置负载服务器(master)

### 3.1、安装Keepalived

```
yum install -y keepalived
```

在CentOS下，通过yum install命令可以很方便地安装软件包，但是前提是你的虚拟机要联网，若没有联网则先从有网的地方下载压缩包，然后拷贝或者上传到linux系统，再进行安装；

### 3.2、配置keepalived

①进入keepalived.conf所在目录：
```
cd /etc/keepalived
```
②首先清除掉keepalived原有配置：
```
true > keepalived.conf
```
③重新编辑keepalived配置文件：
```
vi keepalived.conf
```

内容如下：

```
global_defs {
  notification_email {
    997914490@qq.com
  }
  notification_email_from sns-lvs@gmail.com
  smtp_server 192.168.1.114
  smtp_connection_timeout 30
  router_id LVS_MASTER # 设置lvs的id，在一个网络应该是唯一的
}
vrrp_instance VI_1 {
  state MASTER # 指定keepalived的角色，MASTER为主，BACKUP为备
  interface eth0 # 当前进行vrrp通讯的网络接口卡(当前centos的网卡)
  virtual_router_id 66 # 虚拟路由编号，主从要一直
  priority 100 # 优先级，数值越大，获取处理请求的优先级越高
  advert_int 1 # 检查间隔，默认为1s(vrrp组播周期秒数)
  authentication {
    auth_type PASS
    auth_pass 1111
  }
  virtual_ipaddress {
    192.168.1.200 # 定义虚拟ip(VIP)，可多设，每行一个
  }
}
# 定义对外提供的LVS的VIP以及port
virtual_server 192.168.1.200 8080 {
  delay_loop 6 # 设置健康检查时间，单位为秒
  lb_algo wrr # 设置负载调度的算法为wrr
  lb_kind DR # 设置lvs实现负载的机制，有NAT、TUN、DR三个模式
  nat_mask 255.255.255.0
  persistence_timeout 0 # 同一IP 0秒内的请求都发到同个real server
  protocol TCP
  real_server 192.168.1.111 8080 { # 指定real server1的ip地址
    weight 3 # 配置节点权值，数值越大权重越高
    TCP_CHECK {
      connect_timeout 10
      nb_get_retry 3
      delay_before_retry 3
    }
  }
  real_server 192.168.1.115 8080 {
    weight 3
    TCP_CHECK {
      connect_timeout 10
      nb_get_retry 3
      delay_before_retry 3
    }
  }
}
```

### 3.3、配置负载服务器(slave)

和主负载服务器一样，先安装keepalived，然后编辑keepalived.conf，内容如下，与主负载服务器有些许差别

```
global_defs {

  notification_email {

    997914490@qq.com

  }

  notification_email_from sns-lvs@gmail.com

  smtp_server 192.168.1.112

  smtp_connection_timeout 30

  router_id LVS_BACKUP # 设置lvs的id，在一个网络应该是唯一的

}

vrrp_instance VI_1 {

  state BACKUP # 指定keepalived的角色，MASTER为主，BACKUP为备

  interface eth0 # 当前进行vrrp通讯的网络接口卡(当前进行vrrp通讯的网络接口卡)

  virtual_router_id 66 # 虚拟路由编号，主从要一致

  priority 99 # 优先级，数值越大，获取处理请求的优先级越高

  advert_int 1 # 检查间隔，默认为1s(vrrp组播周期秒数)

  authentication {

    auth_type PASS

    auth_pass 1111

  }

  virtual_ipaddress {

    192.168.1.200 # 定义虚拟ip(VIP)，可多设，每行一个

  }

}

# 定义对外提供的LVS的VIP以及port

virtual_server 192.168.1.200 8080 {

  delay_loop 6 # 设置健康检查时间，单位为秒

  lb_algo wrr # 设置负载调度的算法为wrr

  lb_kind DR # 设置lvs实现负载的机制，有NAT、TUN、DR三个模式

  nat_mask 255.255.255.0

  persistence_timeout 0 # 同一IP 0秒内的请求都发到同个real server

  protocol TCP

  real_server 192.168.1.111 8080 { # 指定real server1的ip地址

    weight 3 # 配置节点权值，数值越大权重越高

    TCP_CHECK {

      connect_timeout 10

      nb_get_retry 3

      delay_before_retry 3

    }

  }

  real_server 192.168.1.115 8080 {

    weight 3

    TCP_CHECK {

      connect_timeout 10

      nb_get_retry 3

      delay_before_retry 3

    }

  }

}
```


启动keepalive服务，主从都要启动！

```
service keepalived start
```

如果所有的"代码主题"都不符合你的要求，你可以参考"一键排版"下的"代码块样式"自定义

## 4、效果展示

### 4.1、负载均衡演示

![演示1](http://p3.pstatp.com/large/pgc-image/125fb22904c74782860be364aaafa1e1)

![演示2](http://p3.pstatp.com/large/pgc-image/01024d8e19e14351843ee6223247ead9)

这里出现了问题，发现同一个浏览器一段时间内刷新，得到的结果是同一个，也就是说同一个ip某一段时间内的请求交给了同一台real server处理，用centos上的浏览器测试也是一段时间内的请求得到的是同一个结果，但是与宿主机却有所不同，比如宿主机得到的是相同的192.168.1.115，而centos上得到的却是192.168.1.111，，可明明我配置了persistence_timeout 0，找了不少资料，还是没能解决！

### 4.2、故障

①、web服务器出现故障

192.168.1.115发生故障，关闭115也就是youzhibing03的tomcat服务

![web1故障](http://p1.pstatp.com/large/pgc-image/2b911a178cac4595a6189abd8bf63da5)

那么宿主机与虚拟机上的结果都只有一个，都得到如下结果

![故障演示1](http://p1.pstatp.com/large/pgc-image/1b80f34bffef49da9bcb190d0bf9006c)

②、web服务器修复，重新启动115的tomcat服务

![web1故障修复](http://p1.pstatp.com/large/pgc-image/c727b3b223114b088736701691e7d831)
![故障恢复展示](http://p3.pstatp.com/large/pgc-image/125fb22904c74782860be364aaafa1e1)

可虚拟机上显示的却还是192.168.111的结果

### 4.3、主从热备演示

①、关闭主负载服务器的keepalived服务

![关闭keepalived](http://p1.pstatp.com/large/pgc-image/ba36179697344846a7218a312d03b79d)

刷新页面，依然能得到如下结果

![刷新结果](http://p1.pstatp.com/large/pgc-image/4d41ccb7ae484dc4abc36dc2d11406a2)

那么也就说明从负载服务器(brucelee)接管了，我们来看下日志

![日志检查](http://p3.pstatp.com/large/pgc-image/a193a3878e11400cbfabc4b413408aa4)

![日志检查](http://p3.pstatp.com/large/pgc-image/4ec9819387c84b0eae849c2d3a3c168e)

发现从负载服务器确实接管了主负载服务器的任务

②、当之前的主负载服务器(master)修复后，日志文件如下

![master日志](http://p1.pstatp.com/large/pgc-image/64b3e42f25b64d0ba985b313fa1cd877)

![master日志](http://p1.pstatp.com/large/pgc-image/dfa9467e3f7e4f4b87939e65f430dde2)

主负载服务器恢复后，从负载服务器让出位置，回到最初的主从状态了！

## 5、总结

总的来说，最终的效果还是符合标题的，虽然在负载均衡那一块有些许疑问，好像没有达到负载均衡的目的，但是也确实只是好像，因为总体而言还是有负载均衡效果的。
