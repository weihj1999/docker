# 1. Docker Network 101

## 1.1 Docker Networking
为了使Docker容器能够通过主机彼此通信，或者和外界进行通信，必须引入网络层支持。 Docker支持不同类型的网络，每种网络都适合特定的用例。

例如，构建在单个Docker容器上运行的应用程序，与具有集群的Web应用程序相比，将具有不同的网络设置，该集群具有数据库，应用程序和负载均衡器，这些均衡器跨越多个需要相互通信的容器。 此外，来自外部世界的客户将需要访问Web应用程序容器。

### 1.1.1 Docker默认网络(docker0)

安装Docker后，将创建一个名为*docker0*的默认桥接网络。 除非指定了自定义网络，否则每个新的Docker容器都会自动附加到该网络。

除了*docker0*之外，Docker还自动创建了另外两个网络：*host* 主机（该网络上的主机和容器之间没有隔离，到外部它们在同一网络上），以及 *none*（连接的容器在特定于容器的网络堆栈上运行）。

详情请参考[Docker默认网络](https://docs.docker.com/network/)

## 1.2 Docker网络类型
Docker附带了针对不同用例的网络驱动程序。 最常见的网络类型是：网桥，Overlay和macvlan。

### 1.2.1 网桥（Bridge）
桥接网络是最常见的网络类型。 它仅限于运行Docker引擎的单个主机中的容器。 桥接网络易于创建，管理和故障排除。

为了使桥接网络上的容器能够进行通信或从外界访问，需要配置端口映射。 例如，假设您可以有一个在端口80上运行Web服务的Docker容器。 由于此容器已连接到**专用子网中的网桥网络**，因此需要将主机系统上的端口（例如8000）映射到容器上的端口80，以使外部流量可以到达Web服务。

>不做特殊配置的桥接网络是不能被外部访问的。必须通过主机上的端口映射。

要创建名为my-bridge-net的网桥网络，请将参数bridge传递给-d（驱动程序）参数，如下所示：
```bash
$ docker network create -d bridge my-bridge-net
```

### 1.2.2 Overlay网络
Overlay网络使用软件虚拟化来创建在物理网络之上运行的其他网络抽象层。 在Docker中，Over网络驱动程序用于多主机网络通信。 该驱动程序利用虚拟可扩展局域网（VXLAN）技术，可在云，内部部署和虚拟环境之间提供可移植性。 VXLAN通过跨第3层网络边界扩展第2层子网解决了常见的可移植性限制，因此容器可以在外部IP子网上运行。

要创建名为*my-overlay-net*的覆盖网络，您还需要*--subnet*参数来指定Docker将用来为容器分配IP地址的网络块：
```bash
$docker network create -d overlay --subnet=192.168.10.0/24 my-overlay-net
```

在不使用swarm的情况下使用overlay网络：

https://medium.com/@ahakimx/overlay-network-without-swarm-mode-586318724c62

### 1.2.3 Macvlan网络
macvlan驱动通过layer2 Segment将Docker容器直接连接到主机网络接口。 无需使用端口映射或网络地址转换（NAT），可以为容器分配一个公共IP地址，该地址可从外部访问。 由于数据包直接从Docker主机网络接口控制器（NIC）路由到容器，因此macvlan网络中的延迟很短。

请注意，必须为每个主机配置macvlan，并支持物理NIC，子接口，网络bond接口，甚至team接口。 主机内核模块明确过滤了流量，以实现隔离和安全性。

要创建名为macvlan-net的macvlan网络，您需要提供--gateway参数来指定子网网关的IP地址，并提供-o参数来设置特定于驱动程序的选项。 在此示例中，父接口在主机上设置为eth0接口：

```bash
$ docker network create -d macvlan \
  --subnet=192.168.40.0/24 \
  --gateway=192.168.40.1 \
  -o parent=eth0 my-macvlan-net
```

## 1.3 容器之间是怎么通信的
不同的网络在容器之间提供不同的通信模式（例如，仅通过IP地址或通过容器名称），具体取决于网络类型以及Docker默认还是用户定义的网络。

### 1.3.1 docker0上的容器的发现（DNS解析）
除非用户指定了不同的名称/主机名，否则Docker将为在默认docker0网络上创建的每个容器分配一个名称和主机名。 然后，Docker会将每个名称/主机名与容器的IP地址进行映射。 该映射允许按名称（而不是IP地址）对每个容器执行ping操作。

此外，请考虑以下示例，该示例以自定义名称，主机名和DNS服务器启动Docker容器：
```bash
$ docker run --name test-container -it \
--hostname=test-con.example.com \
--dns=8.8.8.8 \
ubuntu /bin/bash
```

在此示例中，当在测试容器内运行的进程遇到不在/etc/hosts中的主机名时，将连接至端口53上希望使用DNS服务的8.8.8.8。

了解更详细信息，请参考[Embedded DNS server in user-defined networks](https://docs.docker.com/engine/userguide/networking/configure-dns/)

### 1.3.2 直连容器（Directly linking containers）
启动容器时，可以使用--link选项将一个容器直接链接到另一个容器。 这允许容器彼此发现并安全地将有关一个容器的信息传输到另一容器。 但是，Docker已弃用此功能，建议创建用户定义的网络。

例如，假设您有一个运行数据库服务的mydb容器。 然后，我们可以创建一个名为myweb的应用程序容器，并将其直接链接到mydb：

```bash
$ docker run --name myweb --link mydb:mydb -d -P myapp python app.py
```

详细信息可以查看[容器link](https://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/)

## 1.4 容器如何与外部网络通信
Docker容器可以通过多种方式与外界通信，如下所述。

### 1.4.1 开发端口并转发
在大多数情况下，Docker网络使用子网，而没有外界的访问。 为了允许来自Internet的请求到达容器，您需要将容器端口映射到容器主机上的端口。 例如，如果先前定义了从主机端口8000到容器端口80至的映射，则对hostname：8000的请求将转发到端口80上容器内部正在运行的任何服务。

详情参考[Exposing and publishing ports](https://docs.docker.com/engine/userguide/networking/#exposing-and-publishing-ports)

### 1.4.2 容器接入多个网络

通过将容器连接到多个网络，可以实现用于连接和隔离的细粒度网络策略。 默认情况下，每个容器都将连接到单个网络。 通过首先使用*docker create*（而不是*docker run*）创建容器，然后运行命令*docker network connect*，可以将更多网络连接到容器。 例如：
```bash
$ docker network create net1 # creates bridge network name net1
$ docker network create net2 # creates bridge network name net2
$ docker create -it --net net1 --name cont1 busybox sh # creates container named cont1 attached to network net1
$ docker network connect net2 cont1 # further attaches container cont1 to network net2
```
容器cont1现在就同时接入到两个不同的网络。

详情请参考[Docker自定义网络](https://docs.docker.com/engine/userguide/networking/#user-defined-networks)

### 1.4.3 docker的IPv6网络
默认情况下，Docker仅将容器网络配置为IPv4。 要启用IPv4/IPv6双协议栈，在启动Docker守护程序时需要应用--ipv6标志。 然后docker0网桥将获得一个IPv6链接本地地址fe80 :: 1。 要将全局可路由的IPv6地址分配给您的容器，请使用标志--fixed-cidr-v6，后跟ipv6地址。

详情可参考[Docker的IPv6网络](https://docs.docker.com/engine/userguide/networking/default_network/ipv6/)

## 1.5 常见操作
Docker网络常见操作有：
- Inspect a network：要查看特定网络的配置详细信息，例如子网信息，网络名称，IPAM驱动程序，网络ID，网络驱动程序或连接的容器，请使用*docker network inspect*命令。
- List network： 运行*docker network ls*以显示当前主机上存在的所有网络（及其类型和作用域）。
- Create network： 要创建新网络，请使用*docker network create*命令并指定其类型为网桥（默认），Overlay还是macvlan。
- 创建容器或者连接容器到特定的网络： 首先请注意，网络必须已经存在于主机上。 使用--net选项在容器创建/启动时（docker创建或docker运行）指定网络； 或使用*docker network connect*命令附加现有容器。 例如：
```bash
docker network connect my-network my-container
```
- 断开容器与网络的连接：容器必须正在运行才能使用*docker network disconnect*命令将其与网络断开连接。
- 删除现有网络：如果没有附加容器，则只能使用命令*docker network rm*删除网络。 删除网络后，关联的网桥也将被删除。

## 1.6 总结

Docker提供了成熟的网络模型。 共有三种常见的Docker网络类型-在单个主机中使用的网桥网络，用于多主机通信的覆盖网络以及用于将Docker容器直接连接到主机网络接口的macvlan网络。

我们说明了Docker容器如何发现彼此并进行通信以及它们如何与外界通信。 我们展示了如何执行常见的操作，例如检查网络，创建新网络以及将容器与网络断开连接。 最后，我们简要回顾了Docker网络在常见编排平台（Docker Swarm，Kubernetes指南和Apache Mesos）的上下文中如何工作。
