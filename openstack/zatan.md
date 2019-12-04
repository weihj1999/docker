# 6. 杂谈

## 6.1 存储DWPD

摘要：SSD发展的历史，从EMLC到CLMC、到TLC、未来到QLC，DWPD值越来越低，但是存储系统的可靠性和使用年限却越来越好。为什么？ DWPD的英文名称是Diskful Writes Per Day（每日满容量写入次数），SSD介质的可写次数已经不是存储可靠性的关键，可靠性的关键是软件对SSD物理空间的写入均衡的算法。一个均衡的写入算法，可以保障SSD容量平均磨损。假设10块600GB SSD，每日平均一次均衡写入，数据量是6TB，那么5年维保期间总写入数据量是6*365*

SSD发展的历史，从EMLC到CLMC、到TLC、未来到QLC，DWPD值越来越低，但是存储系统的可靠性和使用年限却越来越好。为什么？

DWPD的英文名称是Diskful Writes Per Day（每日满容量写入次数），SSD介质的可写次数已经不是存储可靠性的关键，可靠性的关键是软件对SSD物理空间的写入均衡的算法。一个均衡的写入算法，可以保障SSD容量平均磨损。假设10块600GB SSD，每日平均一次均衡写入，数据量是6TB，那么5年维保期间总写入数据量是6*365*5 = 10.95PB。企业极少具备10PB数据量，而只购买一个6TB容量的存储设备。业界有大量的研究论文已经论证，DWPD的实用价值远不如存储阵列算法带来的稳定性可信，其中比较著名的论文是2015年google在自己的数据中心做的关于SSD阵列磨损度统计的报告。
华为主流发货的SSD的DWPD为1， 业界主流友商的SSD DWPD一般为1和3，少量友商具备DWPD=10的SSD。高DWPD的SSD主要的应用场景是单盘频繁读写的存储或者计算设备，总体上，领先的存储企业已经规模应用TLC，且很快会走向规模应用QLC。

**一些优化磨损的算法创新是有效提供延长SSD盘数据的手段。**

解释 DWPD，TBW，颗粒原始PE cycle的关系：

估计业务需要的每天写入数据量：Drive Writes Per Day (DWPD).  Terabytes Written (TBW)

同时下面文章解释 DWPD ，TBW 实际上是一回事， DWPD说的一个盘每天写入的数据量/SSD盘容量（例如400GB 容量的SSD盘， 0.3 DWPD的寿命规格，表示 5年维保生命期内，每天可以写入 0.3X400=120GB数据）。 TBW = 每天写的数据量 X 5 X 365（转换为TB 单位）.

很多人认为颗粒原始的 PE cycle 次数就是，写入的SSD盘SAS/SATA接口上写入的数据次数。 这个理解有些误解。 这中间有转换参数（与业务模型相关），不能直接等同。   因为计算相对依赖业务IO特点，所以DWPD规格通常是以最严酷IO模型进行宣称的。（4K 全随机写的IO模型）。 如果IO 模型不是这么残酷，则支持5年的DWPD实际值更大，也就是每天可以写入更多数据，仍然满足寿命要求。

所以估计阵列中每个SSD 每天写入的数据量，是评估应用对SSD寿命需求的方法。

同时，根据客户应用的分析，配套SSD满足95%以上应用，仅写入量特别大的不同应用需要考虑分析该值。

## 6.2 Openstack容器化

Redhat自己搞了容器化的方案，但是已经放弃贡献社区了：
1. 社区推动力不足，代码在社区得不到价值提升；
2. 该领域过于专业，每个厂商基于自己的研究搞。


### 6.2.1 MariaDB的高可用主备数据库容器实践

设计的想法是：
1. 追求多容器实例，主备部署
2. 次之支持数据库定期备份，恢复
3. 为了部署的简单，不考虑使用swarm或者k8s进行容器的调度和编排。
4. 测试调通后，交给自研调度部署软件处理；


网上有一个实践：

https://severalnines.com/database-blog/running-mariadb-galera-cluster-without-orchestration-tools-db-container-management

这个也不错，支持读写分离，但是涉及其他管理器，生产可以考虑；
https://github.com/erasys/mariadb-ha-test

### 6.2.2 RabbitMQ的容器化实践

社区的包安装方法：
https://docs.openstack.org/mitaka/install-guide-ubuntu/environment-messaging.html
三大步骤：
1. 安装包
``` 
    # apt-get install rabbitmq-server
``` 
2. 创建openstack用户
``` 
    # rabbitmqctl add_user openstack RABBIT_PASS
    Creating user "openstack" ...
    ...done.
``` 
3. 给用户授权

```    
# rabbitmqctl set_permissions openstack ".*" ".*" ".*"
Setting permissions for user "openstack" in vhost "/" ...
...done.
```

我们的目标是创建RabbitMQ容器集群，涉及的问题
1. 集群的编排
2. 集群的扩展
3. 基本能力(优先完成)

编排和扩展能力暂时不用管，后续交给maruno处理。

Openstack-Ansible对RabbitMQ的研究：
https://docs.openstack.org/openstack-ansible/pike/admin/maintenance-tasks/rabbitmq-maintain.html
1. 域名要求<br>
RabbitMQ集群可在所有节点之间复制，包括提供高可用性的消息队列。 RabbitMQ节点使用域名相互寻址。 所有群集成员的主机名，以及可能使用与Rabbit相关的CLI工具的任何计算机，都必须是可解析的。

>**重要提醒**
有一个和hostanme有概念的bug，如果主机的.bashrc里面有变量，名字叫HOSTNAME的话，lxc_container模块关联的容器会自动继承这个变量，然后会把容器的hostname设置错误。
https://github.com/ansible/ansible/pull/22246

2. openstack-ansible中RabbitMQ的关键信息总结：
- 创建三个独立的rabbitmq-server，然后把第二个第三个节点加入集群
- mnesia提供了分布式数据库的能力，RabbitMQ用来保存user,exchanges,queues,和bindings等信息。
http://erlang.org/doc/apps/mnesia/Mnesia_overview.html
- 节点出问题后，可以先从集群拿掉，然后修复后重新加入集群。

[RabbitMQ的快速理解](https://blog.51cto.com/11992874/1841182)

[Openstack-Ansible安装RabbitMQ Openstack](http://trumant.github.io/openstack-ansible-multiple-rabbitmq-clusters.html)

3. docker hub支持rabbitmq容器化部署，并且由docker社区维护

- 主机名<br>
最重要的事情： RabbitMQ是基于节点名来保存消息数据的，默认的情况下使用主机名，所以docker运行的时候要通过 *-h 或者--hostname* 来为每个容器进程明确指定主机名。
```
$ docker run -d --hostname my-rabbit --name some-rabbit rabbitmq:3
```
上面的命令，会启动一个RabbitMQ容器，监听在端口5672，随后查看docker logs， 可以大概看到输出如下：
```
=INFO REPORT==== 6-Jul-2015::20:47:02 ===
node           : rabbit@my-rabbit
home dir       : /var/lib/rabbitmq
config file(s) : /etc/rabbitmq/rabbitmq.config
cookie hash    : UoNOcDhfxW9uoZ92wh6BjA==
log            : tty
sasl log       : tty
database dir   : /var/lib/rabbitmq/mnesia/rabbit@my-rabbit
```
从这里可以看得到 *database dir* 也把hostname添加到后面作为文件存储。社区的镜像默认使用 */var/lib/rabbitmq/* 作为容器的卷

- 内存限制<br>
社区的rabbitmq容器明确需要跟踪和管理内存使用，因此需要弄明白cgroup相关的限制。<br>
社区（upstream）的配置是 *vm_memory_high_watermark* 主要参考社区要求[内存管理](vm_memory_high_watermark)即可。<br>
社区镜像可以通过参数 *RABBITMQ_VM_MEMORY_HIGH_WATERMARK* 环境变量来指定，默认值说明：
  - 0.49 is treated as 49%, just like upstream ({ vm_memory_high_watermark, 0.49 })
  - 56% is treated as 56% (0.56; { vm_memory_high_watermark, 0.56 })
  - 1073741824 is treated as an absolute number of bytes ({ vm_memory_high_watermark, { absolute, 1073741824 } })
  - 1024MiB is treated as an absolute number of bytes with a unit ({ vm_memory_high_watermark, { absolute, "1024MiB" } })
  <br>如果当前容器通过 *--memory 或者-m* 限制了内存，百分比会根据这个内存限制进行计算。举个例子：如果 *--memory 2048m*， 并且 *RABBITMQ_VM_MEMORY_HIGH_WATERMARK* 设置为40%，则rabbitmq可以有效使用的内存是819MB（2048MB的40%）
- Erlang Cookie<br>
具体详情可以参考[社区文档](https://www.rabbitmq.com/clustering.html#erlang-cookie)
<br>配置一致性缓存对于集群，以及通过rabbitmqctl进行远程和跨容器管理非常有用。使用 **RABBITMQ_ERLANG_COOKIE**：<br>
```
$ docker run -d --hostname some-rabbit --name some-rabbit --network some-network -e RABBITMQ_ERLANG_COOKIE='secret cookie here' rabbitmq:3
```
从而可以通过客户机实例，或者其他管理终端连接访问
```
$ docker run -it --rm --network some-network -e RABBITMQ_ERLANG_COOKIE='secret cookie here' rabbitmq:3 bash
root@f2a2d3d27c75:/# rabbitmqctl -n rabbit@some-rabbit list_users
Listing users ...
guest   [administrator]
```
也可以通过 **RABBITMQ_NODENAME** 来使操作更简单点
```
$ docker run -it --rm --network some-network -e RABBITMQ_ERLANG_COOKIE='secret cookie here' -e RABBITMQ_NODENAME=rabbit@some-rabbit rabbitmq:3 bash
root@f2a2d3d27c75:/# rabbitmqctl list_users
Listing users ...
guest   [administrator]
```
如果需要通过文件提供cookie，类似于docker的Secrets，需要挂在到 **/var/lib/rabbitmq/.erlang.cookie**
```
docker service create ... --secret source=my-erlang-cookie,target=/var/lib/rabbitmq/.erlang.cookie ... rabbitmq
```
>**备注**<br>
为了确保容器里的Erlang能够正确读取缓存文件，需要指定 **id=XXX,gid=XXX,mode=0600**。 可以查阅[Docker Secrets](https://docs.docker.com/engine/reference/commandline/service_create/#create-a-service-with-secrets)

非常好的一个创建rabbitmq集群的视频教学：
https://www.youtube.com/watch?v=w2kGd2VRJWE

根据这个视频的理解，补充一个遗留问题：
![容器网络草图](/docker//openstack/images/container-overlay-001.PNG)





### 6.2.3 Keystone的容器化实践
这个是2017年的实践，算是比较新的了；

https://github.com/obedmr/docker-keystone

主要说明：

1. 基于dockerfile构建镜像
``` 
    docker build -t obedmr/keystone .
``` 
2. 连接的是mariaDB
``` 
    docker run --name mariadb -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=keystone -e MYSQL_USER=keystone -e MYSQL_PASSWORD=secret -d mariadb
``` 
3. 启动容器镜像
``` 
    docker run -d -p 5000:5000 -p 35357:35357 --link mariadb:mariadb -e DATABASE_HOST=mariadb -e KEYSTONE_DB_USER=keystone -e KEYSTONE_DB_PASSWORD=secret -e KEYSTONE_DB_NAME=keystone --name keystone obedmr/keystone
``` 
通常在容器启动后，需要进行定制化部分，可以在i东容器的时候加上参数：
``` 
    -v <path>/custom-post-keystone-script.sh:/usr/bin/post-keystone.sh
``` 
4. 测试验证
``` 
    docker exec -it  keystone bash
    # Inside the container
    root@26bd2b8a8a60 /root # source openrc
    openstack user list
    +----------------------------------+-------+
    | ID                               | Name  |
    +----------------------------------+-------+
    | 24620586335a473fb56fc2be2f6bfb53 | admin |
    +----------------------------------+-------+
``` 
主要分析：

1. 环境变量说明：
- DATABASE_HOST Database (MariaDB) host
- KEYSTONE_DB_USER Database username that has access to Keystone database
- KEYSTONE_DB_PASSWORD Database password for the user that has access to Keystone database
- KEYSTONE_DB_NAME Keystone database name

2. 使用的基础竟像是ubuntu：16.04， 主要包含了：
- 系统更新
- openstack源配置
- openstackclient安装
- keystone安装
- apache2安装
- 复制配置，进行覆盖
- 运行引导脚本

3. 引导脚本，主要包含下部分
- 变量设置
- keystone.conf配置替换
- 初始化keystone数据库
- 启动apache/keystone服务（keystone-manage bootstrap）<br>
keystone引导服务参考社区文档即可：<br>
https://docs.openstack.org/keystone/pike/admin/identity-bootstrap.html
keystone部署和配置完成后，在使用之前必须进行初始化，提供初始化数据，这个过程称为bootstrapping，引导过程，主要包含了： 创建用户，project，domain，服务，endpoint等等；引导的目的是在系统里面准备好足够的信息，确保系统可以通过认证流程正常访问API：

比如：<br>
``` 
    $ keystone-manage bootstrap \
        --bootstrap-password s3cr3t \
        --bootstrap-username admin \
        --bootstrap-project-name admin \
        --bootstrap-role-name admin \
        --bootstrap-service-name keystone \
        --bootstrap-region-id RegionOne \
        --bootstrap-admin-url http://localhost:35357 \
        --bootstrap-public-url http://localhost:5000 \
        --bootstrap-internal-url http://localhost:5000
``` 
- 如果提供了post-deploy脚本，则执行；

## 语录
1. 生意经：褒贬是买主，喝彩是闲人。
2. 人生凡事不可到极点，极尽必反。古时有谚语“一半黑时还有骨，十分红处变成灰”，说的是木炭，刚点着有火苗的时候还有形状，等都烧尽了，就成灰了。
