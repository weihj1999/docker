# 第一节 分布式MinIO
分布式MinIO使您可以将多个驱动器（甚至在不同的计算机上）合并到一个对象存储服务器中。 由于驱动器分布在多个节点上，因此分布式MinIO可以承受多个节点故障，但仍可以确保完整的数据保护。

## 1.1 为什么选贼分布式MinIO
分布式MinIO可以帮助您通过单个对象存储部署来设置高可用性存储系统。 借助分布式MinIO，无论存储设备在网络中的位置如何，都可以最佳地使用存储设备。

- 数据保护（Data protection）<br>
Distributed MinIO provides protection against multiple node/drive failures and BitRot using erasure code. As the minimum disks required for distributed MinIO is 4 (same as minimum disks required for erasure coding), erasure code automatically kicks in as you launch distributed MinIO.
分布式MinIO使用erasure code阿里放置多节点宕机/磁盘故障，同时提供bitrot保护分布式MinIO所需的最小磁盘为4（与EC所需的最小磁盘相同），在启动分布式MinIO时，自动引入EC功能。
- 高可用(High availability)<br>
单机版MinIO server存在单点故障，相反的，如果一个N节点分布式MinIO，只要保障N/2节点在线，数据就是安全的，不过至少需要（N/2+1）仲裁节点来创建对象。

举个例子: 一个16节点的分布式MinIO，每个节点16块盘，即使8节点宕机，这个集群仍然可用，不过需要9节点在线才能写数据，即创建新对象；
- 限制（Limits）<br>
As with MinIO in stand-alone mode, distributed MinIO has a per tenant limit of minimum of 2 and maximum of 32 servers. There are no limits on number of disks across these servers. If you need a multiple tenant setup, you can easily spin up multiple MinIO instances managed by orchestration tools like Kubernetes, Docker Swarm etc.
与单机版MinIO一样，分布式MinIO的每个租户限制是： 支持最少2个，最多32个服务器，服务器上的磁盘数量没有限制，如果需要一个多租户设置，通过一些编排工具比如k8s，Docker Swarm等管理多个MinIO实例；

>请注意，使用分布式MinIO，可以轻松在限制范围内配置，管理服务器和磁盘，比如：可以有2个节点，每个节点4块盘，4个节点，每个节点4块盘，8节点，每个节点2块盘，32块盘，每个节点64块盘，一次类推；

我们也可以使用[Storage Classes](https://github.com/minio/minio/tree/master/docs/erasure/storage-class)来设置每个对对象的数据定制化和奇偶校验分布（parity distribution）
- 一致性保证(Consistency Guarantees)<br>
对于分布式和单机版的所有I / O操作，MinIO均遵循严格的写后读(read-after-write)和写后列(list-after-write)一致性模型。

## 1.2 Get Started
分布式MinIO打架流程与单机版搭建流程基本一样，MinIO服务器根于传出的参数自动切换单机模式或者分布式模式；

### 1.2.1 前提条件
参考[第一章 第二节 MinIO Docker安装](/section1/install.md)

### 1.2.2 运行分布式MinIO
启动一个分布式Minio实例，你只需要把硬盘位置做为参数传给minio server命令即可，然后，你需要在所有其它节点运行同样的命令。

>注意
- 分布式Minio里所有的节点需要有同样的access秘钥和secret秘钥，这样这些节点才能建立联接。为了实现这个，你需要在执行minio server命令之前，先将access秘钥和secret秘钥export成环境变量。
- 分布式Minio使用的磁盘里必须是干净的，里面没有数据。
- 下面示例里的IP仅供示例参考，你需要改成你真实用到的IP和文件夹路径
- 分布式Minio里的节点时间差不能超过3秒，你可以使用NTP 来保证时间一致
- 在Windows下运行分布式Minio处于实验阶段，请悠着点使用。

1. 示例1： 启动分布式Minio实例，8个节点，每节点1块盘，需要在8个节点上都运行下面的命令

        export MINIO_ACCESS_KEY=<ACCESS_KEY>
        export MINIO_SECRET_KEY=<SECRET_KEY>
        minio server http://192.168.1.11/export1 http://192.168.1.12/export2 \
                       http://192.168.1.13/export3 http://192.168.1.14/export4 \
                       http://192.168.1.15/export5 http://192.168.1.16/export6 \
                       http://192.168.1.17/export7 http://192.168.1.18/export8
![示例1](/images/distributed-minio-001.JPG)

2. 示例2：启动分布式Minio实例，4节点，每节点4块盘，需要在4个节点上都运行下面的命令

        export MINIO_ACCESS_KEY=<ACCESS_KEY>
        export MINIO_SECRET_KEY=<SECRET_KEY>
        minio server http://192.168.1.11/export1 http://192.168.1.11/export2 \
                       http://192.168.1.11/export3 http://192.168.1.11/export4 \
                       http://192.168.1.12/export1 http://192.168.1.12/export2 \
                       http://192.168.1.12/export3 http://192.168.1.12/export4 \
                       http://192.168.1.13/export1 http://192.168.1.13/export2 \
                       http://192.168.1.13/export3 http://192.168.1.13/export4 \
                       http://192.168.1.14/export1 http://192.168.1.14/export2 \
                       http://192.168.1.14/export3 http://192.168.1.14/export4

![示例2](/images/distributed-minio-002.JPG)
