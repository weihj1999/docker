# mq消息中间件

1. 什么是中间件?

非底层操作系统软件，非业务应用软件，不是直接给最终用户使用的，不能直接给用户带来价值的软件系统称为中间件。

2. 什么是消息中间件?

关注于数据的发送与接收，利用高效可靠的异步消息传递机制集成分布式系统。图示如下:

![中间件接口](http://p1.pstatp.com/large/pgc-image/d67b55cbaa864a7b91e663bb8820c53d)

3. 什么是JMS？

Java消息服务(message service)即是JMS，是一个Java平台关于面向消息中间件的API，用于在两个程序间或分布式应用系统之间发送消息，进行异步通信。

4. 什么是AMQP？

AMQP(Advanced message queuing protocol)是一个提供统一消息服务的应用层标准协议，基于此协议的客户端与消息中间件可传递消息，并不受客户端、中间件等不同产品，不同开发语言等条件的限制。

5.JMS与AMQP对比

![JMS与AMQP对比](http://p1.pstatp.com/large/pgc-image/57228498104d458b8c58b49beadbf68d)

6. 常见消息中间件对比

![常见消息中间件对比](http://p9.pstatp.com/large/pgc-image/82b0804c0e9140c08787b8b15a68c789)

7. JMS规范

JMS相关概念：

提供者:实现JMS规范的消息中间件服务器

客户端:发送或者接受消息的应用程序

生产者/发布者:创建并发送消息的客户端

消息者/订阅者:接受并处理消息的客户端。

消息:应用程序之间传递的数据内容

消息模式:在客户端之间消息传递的方式，JMS定义了主题和队列两种模式

7.1 消息模式-队列模型

客户端包括生产者和消费者

队列中的消息只能被一个消费者消费

消费者可以随时消费队列中的消息(可以不用预先预定此消息)

![消费模式](http://p1.pstatp.com/large/pgc-image/3b3277dbf34044db8a3d8520c8b1fd9a)

7.2 消息模式-主题模式

客户端包括发布者和订阅者

主题中的消息被所有订阅者消费

消费者不能消费订阅之前就发送到主题中的消息·

![消息模式](http://p9.pstatp.com/large/pgc-image/0e5700f85d8c4725801666a40df59cb5)

（图中需要订阅者先订阅主题，然后发布者发布消息，因为鼎业这不能消费订阅之前就发送的消息）

7.3 JMS编码接口

ConnectonFactory:用于创建连接到消息中间件的连接工厂

Connection:代表了应用程序和消息服务器之间的通信链路

Destination:指消息发布和接收的地点，包括队列和主题

Session:表示一个单线程的上下文，用于发送和接收消息

MessageConsumer:由Session创建，用于接收到发送到目标的消息

MessageProducer:由Session创建，用于发送消息到目标

Message:是在生产者和消费者之间传送的对象，由消息头(必须存在)、一组消息属性和一个消息体组成。

7.4 JMS编码接口之间的关系

![JMS模式](http://p3.pstatp.com/large/pgc-image/f444fe77a40244979a9a27f70daa55d0)

Connection是可以被多个线程共享的，一个Connection创建多个Session，每个Session属于一个线程上下文。也就是Connection可被其他线程共享，而Session是单线程的，只在当前上下文有效，可以进行一些事务操作。Session也可以创建消息。

# 消息中间件中的概念




![Kafaka Log](http://p3.pstatp.com/large/pgc-image/fed2f3983f7948d79fceb19a6f33db00)

接上一篇的《什么是分布式消息中间件？》，这一篇来介绍一下!

Topic

主题，从逻辑上讲一个Topic就是一个Queue，即一个队列；从存储上讲，一个Topic存储了一类相同的消息，是一类消息的集合。比如一个名称为trade.order.queue的Topic里面存的都是订单相关的消息。

Partition

分区。分区是存在于服务端，内部保持顺序、且顺序不可变更的一个队列，用于存储消息。分区可能不应该出现在消息领域内，在使用消息中间件发送和消费时，实际上用户是感受不到分区这个概念的。下面这幅图便于大家去理解分区：

![分区](http://p3.pstatp.com/large/pgc-image/9fa972b143074ad295b90b21bf7c6b24)

一个Topic存储消息时会分为多个Partition，每个Partition内消息是有顺序的。至于为什么需要将Topic划分成按照Partition存储，在以后设计和实现部分会解释。

Producer

生产者，消息的生产方，一般由业务系统负责产生消息。

Producer负责决定将消息发送到哪个Topic的那个Partition。

Consumer

消费者，消息的消费方，一般是后台系统负责异步消费消息。

Consumer订阅Topic，消费Topic内部的消息。

Broker

消息的存储者，一般也称为Server，在JMS中叫Provider，在RocketMQ（阿里开源的消息中间件）中叫Broker。

NameServer

NameServer其实不是消息中间件的概念了，一般在分布式系统中都会有一个角色作为NameServer用于服务发现，在Kafka中使用ZooKeeper来实现，在RocketMQ中单独写了NameServer服务。

Group

Group用于标志Consumer的身份，拥有相同Group名称的Consumer一般消费一类消息，且消费逻辑是相同的。

RocketMQ中Producer也需要Group标志身份，但是实际上Producer是不需要的。因为Producer之间是不相关的，Consumer之间是需要协同工作的。

这里多解释一下为什么Consumer之间是需要协同工作的。

比如启动了两个Consumer来消费订单消息，然后调用物流系统进行发货。那么在产生一条订单消息后，只能让两个Consumer中的一个来处理消息（否者就发货两次了）所以需要一个标识将这两个Consumer标记为行为一致的。另一个场景是如果一个Consumer实例宕机了，这个时候需要有行为相同的Consumer去接管它的消费任务，那么就需要一个标识来标识行为相同的Consumer。

那是不是只要行为相同的Consumer只存在一个就好了呢？是的，如果只有行为一致的Consumer，那么就不存在协同工作，也可以不需要Group，每个Consumer拥有独立的ID即可。但是实际系统是不可能的，从系统可用性和性能上都不可能（单个Consumer就有单点问题，也有性能问题，毕竟我们谈的是分布式系统）。

集群消费

集群消费的含义是说一类Consumer（即Group相同的Consumer的集合）共同完成对一个Topic的消费。其实上面说明Consumer需要协同工作时举例中就默认是集群消费了，这也是现实业务中95%以上需求的消费方式。

具体来看集群消费模式如下：

![集群消费模式架构](http://p3.pstatp.com/large/pgc-image/e2e6e01877924c2698ee0b3ed847dd72)
Consumer0和Consumer1属于同一个Group，假设Topic中有0~5共6条消息，Consumer0消费到0~2，Consumer1消费到3~5，它们共同完成了Topic中消息的消费。

这存在于大量的无状态的后台系统中，就如上面说的消费订单消息进行发货的例子。

广播消费

广播消费的含义是Topic中的每一条消息都会被一类Consumer（属于同一个Group的多个Consumer）中的每个Consumer实例消费。

如下图，Topic总的0~5共6条消费，Consumer0会消费到0~5完整的6条消息，Consumer1也会消费到0~5的6条消息。

![广播消费](http://p9.pstatp.com/large/pgc-image/5442ca2b7d9c4f20b8f38fcd5f5cd320)

这种消费往往应用在有状态的服务，比如缓存服务器去消费消息更新自己的缓存数据，那么每一台缓存服务器都需要拿到消息。

结语

了解什么是分布式消息中间件和消息中间件的一些概念之后，下一篇计划谈一谈分布式消息中间件的需求，毕竟要有的放矢，明确需求才能知道要做什么，怎么做才合适。

PS：为什么本文开头会用一张Kafka的Logo呢？因为Kafka真的是一个非常优秀的软件，文中一些概念也来源于Kafka（如果对消息中间件有兴趣，强烈建议去看看Kafka的文档和实现）。

# 中间件开发杂谈

什么是中间件开发？

随着国内软件行业的发展，国内互联网公司规模越来越大，业务越来越复杂，随之使用大量的中间件来提高后台服务性能。由此产生了中间件开发和维护人员。

诚然，在小公司，中间件，例如缓存，MQ，RPC 等服务，极大可能是由业务开发人员自己维护，或者委托第三方云平台运维（支付一些费用）。但，如果后台开发超过 200 人，基本就会组建自己的中间件或者基础架构团队，用于维护后台服务器基础架构和中间件。

更大规模的公司，则由于各种各样的原因（性能，KPI），会自己开发中间件，简称自研。这要求中间件团队需要更多的人员。

中间件开发人员需要哪些素质？

既然需要中间件开发人员，那么中间件开发人员一般从哪里招聘呢？招聘的要求是什么？

通常，一个公司在刚开始组建中间件团队的时候，都会从公司内部挑选精英人才，或者挑选对中间件感兴趣的人才。这时候，可能你没有相关经验，但你仍然有机会参与到中间件开发中。反之，如果你没有中间件开发经验，想通过招聘的方式进入中间件行业，那么相对而言，会有些曲折。

在这个分工明确的时代，即使是中间件，也有很多种类，我这里稍微分一下，可能不是很准确。

- 服务治理中间件，例如 RPC 相关中间件，限流熔断，链路追踪，分布式配置中心等等。你可以从 SpringCloud 里找到相关的产品。当然国内也有很多优秀的产品。
- 存储中间件，例如缓存，MQ等等，如果存储涉及到分布式（通常都会涉及），那么要求相对较高。
- 各种 Proxy，不论是数据库，还是 Cache，还是各种存储，通常单机无法承载海量数据，比较简单的办法就是使用 Proxy 进行代理，让应用透明的使用集群。出于性能考虑，这里通常会使用性能较高的产品，例如 goLang，C++ 等。Java 的长处——开发效率，在这个地方权重不大。
- 各种分布式中间件，例如 ZK 这种，这个我个人认为难度是较大的。分布式向来是软件开发中比较困难的一个点。特别是涉及到存储和一致性。
- 容器相关，k8s，docker等，容器化已经是大势所趋，其实我也不是很懂（听大佬们说的）。

回到之前的话题: 一个中间件开发者需要哪些素质？

- 语言基础。从 Java 程序员的角度，基础通常就是：集合，并发，JVM，Netty，IO、NIO（mmap，sendfile）
- 计算机基础，由于中间件开发人员经常和 OS 打交道，所以计算机基础也必不可少，例如文件系统（IO/磁盘），进程线程，内存管理。
- 网络基础，搞后台的人员，肯定要对网络熟悉了，熟悉在 Linux 下排查网络问题，熟悉 Epoll 原理等。
- 分布式相关知识，互联网海量数据背景下，分布式知识必不可少，CAP， Paxos，Raft，zab，2pc，3pc，base等等。最好能根据这些理论写出实现代码。
- 熟悉开源实现，即使你是业务开发人员，你也 100% 会接触开源项目，例如 Spring，那么，通常你需要对这种常用的开源代码有深刻的理解，不仅知晓其原理，也领会其设计。从大的角度看，你得看清整个框架的背景，设计和取舍，从小的角度看，你得看清框架的内部实现细节，有哪些有趣的地方（通常这种框架都会进行性能优化）。
- 了解行业风向标，中间件行业和业务开发稍有不同，每个中间件的版本升级都会让该领域的开发者们侧目（类似 iPhone 发布会），了解其特性，进而了解行业趋势，最后成为行业引领。

如何成为中间件开发人员？

好，说完了中间件开发人员需要哪些素质，自然，如何成为中间件开发人员，就不言自明了。

说白了，以上 6 个点，都是硬骨头。

- 对于已经开始工作的人来说，需要平时深刻的积累，说的难听一点，如果你的业务开发任务很重，你很难搞定上门的这些内容。
- 对于还在上学的同学来说，很爽，你可以用学校（不仅仅指大学，据我所知的大神，通常是初中/小学就开始编程，但这不是必须的）里大把的时间来学习，一个个的搞定这些知识点，和社招不同，如果你的知识达到上面的水平，那么 SP offer 应该是随便拿了 ：）

你可能想跳槽。

那么你大概需要做以下准备：

- 巩固 Java 基础，集合源码，并发源码，JVM 原理，Netty 原理源码，IO 相关（涉及到零拷贝文件存储），这些都是 Java 基础，通常是必须的。
- 分布式原理，最起码知晓理论知识，最好能写一个，哪怕参照开源的也行。
- 源码，Spring Mybatis Tomcat 等等，这些代码通常是你最先接触的，不妨从这里开始。RPC 中间件相关的，Dubbo，Motan，SOFA，挑一个吧，推荐 SOFA。
- 再熟悉熟悉（熟悉指源码和设计）分布式的相关产品，假设你是 Java 开发，推荐 RocketMQ，Apollo 配置中心等等中间件，其实都可以，MQ 相对复杂。
- 操作系统，通常，你在研究上面的内容时，会遇到操作系统的疑问，遇到不要绕过，尽量弄明白。
- 自己的产品，有就最好了，例如公众号，博客，教学视频，GitHub 项目等等，总之，是拿得出手的东西。
- 加大牛好友，了解行业风向标。也许你是一个矜持的人，但从事了这个行业，你有必要和行业里优秀的人学习(看看朋友圈就好)。

# 中间件Kafka入门

什么是消息中间件？

非底层操作系统软件，非业务应用软件，不是直接给最终用户使用的，不能直接给客户带来价值的软件统称为中间件
关注于数据的发送和接收，利用高效可靠的异步消息传递机制集成分布式系统。

什么是Kafka？

Kafka是一种高吞吐量的分布式发布订阅消息系统，是一个分布式的、分区的、可靠的分布式日志存储服务。它通过一种独一无二的设计提供了一个消息系统的功能。

kafka官方：http://kafka.apache.org/

Kafka作为一个分布式的流平台，这到底意味着什么？

我们认为，一个流处理平台具有三个关键能力：

- 发布和订阅消息（流），在这方面，它类似于一个消息队列或企业消息系统。
- 以容错的方式存储消息（流）。
- 在消息流发生时处理它们。


什么是kakfa的优势?

它应用于2大类应用：

- 构建实时的流数据管道，可靠地获取系统和应用程序之间的数据。
- 构建实时流的应用程序，对数据流进行转换或反应。

kafka有四个核心API

- 应用程序使用 Producer API 发布消息到1个或多个topic（主题）。
- 应用程序使用 Consumer API 来订阅一个或多个topic，并处理产生的消息。
- 应用程序使用 Streams API 充当一个流处理器，从1个或多个topic消费输入流，并生产一个输出流到1个或多个输出topic，有效地将输入流转换到输出流。
- Connector API允许构建或运行可重复使用的生产者或消费者，将topic连接到现有的应用程序或数据系统。例如，一个关系数据库的连接器可捕获每一个变化。


![消息通信](http://p1.pstatp.com/large/pgc-image/c93ac48518fd4d2e9344b94ce441ffdd)


Client和Server之间的通讯，是通过一条简单、高性能并且和开发语言无关的TCP协议。并且该协议保持与老版本的兼容。Kafka提供了Java Client（客户端）。除了Java Client外，还有非常多的其它编程语言的Client。

Kafka好处

- 可靠性 - Kafka是分布式，分区，复制和容错的。
- 可扩展性 - Kafka消息传递系统轻松缩放，无需停机。
- 耐用性 - Kafka使用分布式提交日志，这意味着消息会尽可能快地保留在磁盘上，因此它是持久的。
- 性能 - Kafka对于发布和订阅消息都具有高吞吐量。 即使存储了许多TB的消息，它也保持稳定的性能。

Kafka非常快，并保证零停机和零数据丢失。

应用场景


- 指标 - Kafka通常用于操作监控数据。 这涉及聚合来自分布式应用程序的统计信息，以产生操作数据的集中馈送。
- 日志聚合解决方案 - Kafka可用于跨组织从多个服务收集日志，并使它们以标准格式提供给多个服务器。
- 流处理 - 流行的框架(如Storm和Spark Streaming)从主题中读取数据，对其进行处理，并将处理后的数据写入新主题，供用户和应用程序使用。 Kafka的强耐久性在流处理的上下文中也非常有用。

Kafka相关术语

序号组件和说明

1. Topics（主题）: 属于特定类别的消息流称为主题。 数据存储在主题中。主题被拆分成分区。 对于每个主题，Kafka保存一个分区的数据。 每个这样的分区包含不可变有序序列的消息。 分区被实现为具有相等大小的一组分段文件。
2. Partition（分区）:主题可能有许多分区，因此它可以处理任意数量的数据。
3. Partition offset（分区偏移）每个分区消息具有称为 offset 的唯一序列标识。
4. Replicas of partition（分区备份）副本只是一个分区的备份。 副本从不读取或写入数据。 它们用于防止数据丢失。
5. Brokers（经纪人）代理是负责维护发布数据的简单系统。 每个代理中的每个主题可以具有零个或多个分区。 假设，如果在一个主题和N个代理中有N个分区，每个代理将有一个分区。假设在一个主题中有N个分区并且多于N个代理(n + m)，则第一个N代理将具有一个分区，并且下一个M代理将不具有用于该特定主题的任何分区。假设在一个主题中有N个分区并且小于N个代理(n-m)，每个代理将在它们之间具有一个或多个分区共享。 由于代理之间的负载分布不相等，不推荐使用此方案。
6. Kafka Cluster（Kafka集群）Kafka有多个代理被称为Kafka集群。 可以扩展Kafka集群，无需停机。 这些集群用于管理消息数据的持久性和复制。
7. Producers（生产者）生产者是发送给一个或多个Kafka主题的消息的发布者。 生产者向Kafka经纪人发送数据。 每当生产者将消息发布给代理时，代理只需将消息附加到最后一个段文件。 实际上，该消息将被附加到分区。 生产者还可以向他们选择的分区发送消息。
8. Consumers（消费者）Consumers从经纪人处读取数据。 消费者订阅一个或多个主题，并通过从代理中提取数据来使用已发布的消息。
9. Leader（领导者） Leader 是负责给定分区的所有读取和写入的节点。每个分区都有一个服务器充当Leader 。
10. Follower（追随者）跟随领导者指令的节点被称为Follower。 如果领导失败，一个追随者将自动成为新的领导者。 跟随者作为正常消费者，拉取消息并更新其自己的数据存储。

使用Kafka

1. 安装jdk
2. 安装zookepper 官方：http://zookeeper.apache.org/
3. 安装kafka

我这里jdk是已经安装好的。

安装zookepper：
```
tar -zxvf zookeeper-3.4.13.tar.gz #解压
cd zookeeper-3.4.13/config #进入配置目录
#zookeeper运行需要config里有config文件。但是解压后默认只有zoo_sample.cfg，我们将名字修改下即可
mv zoo_sample.cfg zoo.cfg #修改配置文件名字
```
启动zookeper，来到bin目录：
```
./zkServer.sh start #启动zookepper
```
停止zookeper，来到bin目录：
```
./zkServer.sh start #停止zookepper
```
kafka下载：https://www.apache.org/dyn/closer.cgi?path=/kafka/0.9.0.0/kafka_2.11-0.9.0.0.tgz

使用Kafka，
```
tar -zxvf kafka_2.11-2.1.0.tgz #解压kafka
```
启动zookpper服务,来到kafka的bin目录：
```
./zookeeper-server-start.sh config/zookeeper.properties #启动服务
```
启动kafka服务：
```
./kafka-server-start.sh config/server.properties #启动kafka服务
```
创建一个主题:
```
kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test #topic_name
topic_name：主题的名字'test'。
```

创建好后查看主题:
```
kafka-topics.sh --list --zookeeper localhost:2181
```
Kafka提供了一个命令行的工具，可以从输入文件或者命令行中读取消息并发送给Kafka集群。每一行是一条消息。

运行producer（生产者）,然后在控制台输入几条消息到服务器。

发送消息：
```
./kafka-console-producer.sh --broker-list localhost:9092 --topic test #主题为test
```
进入之后就可发送消息！！！

Kafka也提供了一个消费消息的命令行工具，将存储的信息输出出来。

消费消息：
```
#topic主题需要与被消费的主题对应上
./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
```
Kafka常用命令
```
#查看所有主题列表
kafka-topics.sh --zookeeper localhost:2181 --list
#查看指定topic信息
kafka-topics.sh --zookeeper localhost:2181 --describe --topic topic_name
#控制台向topic生产数据
kafka-console-producer.sh --broker-list localhost:9092 --topic topic_name
#控制台消费topic的数据
kafka-console-consumer.sh --zookeeper localhost:2181 --topic topic_name --from-beginning
#查看topic某分区偏移量最大（小）值
kafka-run-class.sh kafka.tools.GetOffsetShell --topic hive-mdatabase-hostsltable --time -1 --broker-list localhost:9092 --partitions 0
#增加topic分区数
kafka-topics.sh --zookeeper localhost:2181 --alter --topic topic_name --partitions 10
#删除topic，慎用，只会删除zookeeper中的元数据，消息文件须手动删除
kafka-run-class.sh kafka.admin.DeleteTopicCommand --zookeeper localhost:2181 --topic topic_name
```
