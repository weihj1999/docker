# 1. 负载均衡概览
来源： https://www.jianshu.com/p/612a55652ae8

Keepalived LVS+DR合设配置方法以及存在的问题

# 2. 运维相关

## 2.1 OS监控

### 2.1.1 指标

| 指标名称	| 指标说明	| 原理说明	| 采集审计 |
| --- | --- | --- | --- |
| os_cpu_usage	| 所有CPU平均使用率 		|  |
|os_cpu_max_core_st_usage | 单核CPU最大steal占比	| 在虚拟环境中运行其他操作系统花费的时间。这里如果统计所有CPU核的，可能会导致平均被拉低，这样单核进程无法被识别到。因此只关注所有核中，最大的steal比率。|  | 	
|os_cpu_max_core_ usage	| 单核CPU最大使用率	| | | 	
|os_cpu_max_core_irq	| 单核CPU最大的最大硬中断占比	| 频繁的中断会严重影响性能。比较常见的是使用iptables（连接数）或者tc流控的时候，在流量大的时候，会导致频繁的中断	| 在检测进程的cpu和内存的时候，会将top信息中进程非cpu为0和内存为0的部分做审计记录 |
| os_cpu_max_core_soft	| 单核CPU最大的软中断占比		| 同上 | 同上 |
| os_filesystem_readonly	| 文件系统只读指标	| mount | grep -E "^/dev.\*\(ro"<br>grep匹配不为空，则说明存在文件系统只读的情况	| 将识别出来的内容审计保存，支撑审计分析<br>[root@dcs2-bms-cn-north-4-az2-03 sysadmin]# mount | grep -E "^/.*\(ro"<br>/dev/sdb1 on /var/log/dcs type ext4<br>(ro,relatime,data=ordered)<br>/dev/sdb2 on /opt/dcs/data type ext4 (ro,relatime,data=ordered) |
|os_disk_inode_usage	| 所有磁盘分区最大的inode使用率 | 		|  使用率超过80%的磁盘，记录审计日志 |
|os_disk_usage	| 所有磁盘分区最大的磁盘使用率 | 		| 使用率超过80%的磁盘，记录审计日志 |
| os_disk_await_max	| IO设备操作最大时延	| 需要具备过滤部分磁盘的监控，因为大部分业务对磁盘敏感性业务，通常是单独挂载磁盘，或者单独挂载SSD超高性能的磁盘。<br> 通过/tmp/os_disk_io_ignore_devices保存需要过滤的磁盘设备	| 如果await超过10ms，记录审计日志。审计日志支撑磁盘影响多久的审计。 |
| os_disk_write _max	| 磁盘设备写速率（KB/s）		| 同上 | 同上 |
| os_disk_read_max	| 磁盘设备读速率（KB/s）	| 同上 | 同上 | 	
| os_file_handles			| | | |
|os_loadavg		| | | | 	
|os_memory_usage		| | | | 	
|os_memory_available	| OS可用内存	| 不同规格使用率不一样，风险也不太一样。但是如果OS内存可用小于2G之类的，风险比较高。特别是针对逻辑多租，如果可用内存小于100G，也有风险	| 不涉及审计 |
| os_network_error_packets		| | 从interface监控错误的包	| 如果error的包不为零，或者drop包不为零，需要把ifconfig原始输出归档审计 |
| os_network_droped_packets	| | 	从interface监控丢弃的包	| 同上 |
| os_network_interface_abnormal			| | | |
| os_network_ping_abnormal | 	数据面不涉及	| | |
|os_network_tcp_fast_retran	| 系统快重传量	| | |  	
|os_sshd_abnormal	| 数据面不涉及		| | |
|os_network_tcp_timeout_retran	| 系统超时tcp重传量	| | | 	
|os_crond_abnormal	| crond服务是否异常	| service crond status<br>输出结果包含：active (running)	| 守护进程未执行下，保存service crond status结果 |
|os_ntpd_abnormal | | | | 			
|os_syslogd_abnormal	| rsyslogd服务是否异常	| service rsyslog status<br>输出结果包含：active (running)	| 守护进程未执行下，保存service rsyslog status结果 |
| os_hotpatch_abnormal		| | | | 	
|os_msg_has_kernel_error	| 内核是否存在错误日志	| | | 	
|os_has_rebooted			| | | |
|os_ntp_sync_abnormal	| | | | 		
|os_uptime	| 系统启动时间（秒）	| 记录系统启动的时间，如果判断重启，则告警规则可以设置为uptime < 5分钟之类的	| |
|os_msg_memory_abnormal	| 系统是否存在内存错误日志	| /var/log/messages日志存在”bad pmd日志“，可能导致系统内存异常，存在数据丢失风险,需要关闭大内存透明页| | 	
|os_conntrack_usage	| 连接跟踪表使用率	| | | 	
|os_cron_has_failed	| 系统cron任务调度失败	| 需要和监控同事了解如何获取调度失败情况	| |
|os_network_tcp_overflowed	| 系统tcp连接全队列溢出	| 如果是半连接队列满了，最直接的现象就是syn包丢弃。	| |
|os_directory_file_changed			| | | |
|os_network_syn_drop	| 系统syn包丢弃量	| | | 	
| os_has_uninterruptible _procs		| | | | 	
|os_account_expire	| 系统账号过期时间	| 包含账号有效期、密码有效期等有效时间	| |
|os_swap_usage	| Swap使用率		| | |
|os_swap_used | 	Swap使用量	| 大部分情况下，swap一旦使用都可能会影响性能，需要关注系统是否存在swap的使用 <br> 如果没有挂载swap，则设置值为-1，因为数据面老实例和新实例可能存在有的有挂载，有的没挂载，监控上需要能区分。| | 	
|os_network_max_rxkB	| 网络最大接受速率		| | |
|os_network_max_rxpck	| 网络最大接受PPS		| | |
|os_network_max_txpck	| 网络最大传输PPS		| | |
|os_network_max_txkB	| 网络最大传输速率		| | |
|os_ntp_offset	| 时钟偏差	| 通过ntpdate -q ntp_server_ip查询时钟偏差，如果服务器不可用，则偏差设置为999	| |
|os_message_warn	| messages中警告日志量	| | 同上| 	
|os_message_error	| messages中错误日志量		| | 通过日志监控，无需审计。|
|os_message_log_time	| 监控日志采集的message日志时长	如果日志打印太快，则会严重影响性能。当前message日志监控主要是取最后1000行日志作为日志打印，如果这1000行日志覆盖的时间小于10分钟，则说明日志打印太快，需要分析为何日志打印这么快；<br> 如果是0，则说明日志没有打印，这种情况也需要关注下是否打印存在问题	| 同上 |
|os_ping_gateway	| 系统ping网关时延	| 如果ping不通，则时延为所设置的超时时间，如果可以ping通，关注最大时延	| |
|os_ping_external	| 客户网络ping时延	| 根据当前ESTABLISHEDde TCP连接，过滤非资源租户侧子网的地址（也就是租户侧子网地址，或者从租户侧过来的VPN地址），获取ping最大的时延，且超时时间设置为5s，如果ping不通，则丢弃（因为租户侧很可能安全组没放通ping）| 	Ping时延超过10ms，记录审计日志|
|os_ping_internal	| 资源租户网络ping时延	| 根据当前ESTABLISHED的TCP连接，过滤资源租户侧子网的地址，尝试ping，获取ping最大的时延，且超时时间设置为5s，ping不通则置超时时间为5秒	| Ping时延超过10ms，记录审计日志 |
|os_tcp_RecvQ_blocked	| TCP接受队列出现阻塞的客户端数量	| ss命令中，RecvQ出现持续大于1000000的个数，这个时候，说明服务端处理不及时，服务端性能跟不上。	| 出现阻塞客户端后，将原始内容保存，用于分析审计的客户端 |
|os_tcp_SendQ_blocked	| TCP发送队列出现阻塞的客户端数量	| ss命令中，SendQ出现持续大于1000000的个数，这个时候说明客户端处理不及时，客户端性能跟不上	| 同上 |
|os_process_number	| 系统进程的个数		| | |
|os_swap_pswpin	| Swap置入（Byte）	| Swap使用，通常不一定表示影响性能，只有swap置入置出才真正会影响性能，该指标主要支撑问题定位	| |
|os_swap_pswpout	| Swap置出（Byte）| 同上 | | 		
|os_eth1_route	| 资源租户方案下eth1路由是否正常	| 需要注意的是，到eth0的路由不需要探测。因为数据面监控，定位是发现潜在隐患，真正实例是否异常，一定需要通过靠谱的手段检测。当前DCS、DMS等服务是1分钟管理面会探测一次，如果eth0路由异常，探测必然会失败	|  |

### 2.1.2	告警规则
| 告警名称	| 告警规则	| 触发次数	| 客户等级	| 告警等级	| 告警影响	| 备注 |
| --- | --- | --- | --- | --- | --- | --- |
| os_cpu_usage_over_80	| os_cpu_usage>80	| 2	| V4/V5	| 紧急	| | 	保障性告警|
| os_cpu_st_usage_is_high	| 系统CPU资源steal使用率超过30%同时CPU使用率超过90%，请检查宿主机CPU过高，存在资源争抢| 	10| 	V4/V5	| 紧急	| 宿主机进程争抢资源	| 运维告警|

## 2.2	进程监控

进程指标，传入进程名字，以及进程pid，如下指标通用性实现

| 指标名称	| 指标说明	| 原理说明	| 采集审计|
| --- | --- | --- | --- |
| xxx_process_status	| 进程运行状态		| | |
| xxx_process_fd_num	| 进程fd使用量		| | |
| xxx_process_fd_usage	| 进程fd使用率		| | |
| xxx_process_max_cpu	| 进程最大cpu使用率	| | | 	
| xxx_process_max_mem	| 进程最大内存使用率		| | |
| xxx_process_avg_cpu	| 进程平均cpu使用率		| | |
| xxx_process_avg_mem	| 进程平均内存使用率		| | |
| xxx_process_uptime	| 进程启动时间	| 通过命令ps -p 86941 -o etimes获取进程号启动的时间（秒）	| |

## 2.3	可靠性配置
### 2.3.1	资源租户VM网卡配置
通常，ECS默认的网络配置是默认的dhcp，云服务实践过程中零星发现dhcp会受到很多因素影响，比如低概率出现IP丢失等问题，或者从dhcp获取不到IP的情况。

实践要点：
1.	推荐使用静态IP配置，避免dhcp租借导致的失败，或者定期续租可能导致的问题。
2.	如果需要使用dhcp，则需要定时脚本，不断判断vm的网卡是否异常，通过重启网络进行自恢复。
3.	资源租户镜像建议加固，镜像启动VM后，crontab定期运行脚本，判断eth0是否有IP，如果没有IP，则执行“service network restart”命令，重新获取IP。<br>
早期DCS经常出现创建实例失败，原因是VM启动后，eth0的IP没有获取到，当时出现几次，由于实例创建会回滚，当时未定位到根因。但是通过在镜像打入脚本，如果vm启动后，没有eth0的IP，则重新通过脚本重启网络。后续未出现过类似问题。


# 3	公共组件运维能力

## 3.1	Keepalived

### 3.1.1	技术原理

### 3.1.2	可靠性方案


#### 3.1.2.1	高可靠配置garp_master_refresh
配置方法：<br>

配置说明：通过配置garp_master_refresh为60，Keepalived的主节点每60秒周期发送免费ARP广播，避免网络脑裂情况，发送免费ARP不生效，造成VIP的MAC地址错误，VIP无法被访问的问题。

#### 3.1.2.2	配置VRRP广播网段

针对租户面服务使用Keepalived，需要将VRRP广播网段设置为资源租户网段，避免在租户网段广播，可能导致和租户其他业务出现冲突。比如租户使用了自己业务的VRRP，存在广播流量互相影响的问题。

### 3.1.3	监控
| 指标名称	| 指标说明	| 采集原理	| 误报识别	| 采集审计 | 	告警阈值 |
| --- | --- | --- | --- | --- | --- |
| Keepalived_failover	| 检测是否存在倒换	| 监控/var/log/message日志，监控过去5分钟是否存在关键字：<br>Received lower prio advert<br>Received higher prio advert<br>Entering BACKUP STATE<br>Entering MASTER STATE <br>Transition to MASTER STATE	| 实例重启的时候可能触发倒换		| | |
| Keepalived_check_failed	| Keepalived检测脚本执行是否异常	| 监控/var/log/message日志，监控过去5分钟是否存在关键字：
Now in FAULT state	| 实例重启的时候，可能触发检测失败 | | 		
| Keepalived_check_connectivity	| 主和备分别ping对方IP，检测连通性	| | | 		Ping失败时，命令响应记录到messages日志
| Keepalived_process_status	进程状态| | | | | 				

如果一个VIP被关联到其他MAC地址，主节点上是否有办法监控到？

### 3.1.4	故障场景、定界、应急恢复
| 故障场景	| 产生原因	| 故障对应的监控指标	| 故障定界	| 故障恢复 |
| --- | --- | --- | --- | --- |  
| VIP MAC地址错误	| 通常是网络脑裂时，主和备都发送免费ARP造成VIP MAC地址错误 |  | 		集成check_status.sh脚本，脚本检查/var/log/message是否有倒换相关的关键日志，如：| 触发主备倒换 |

### 3.1.5	现网案例

#### 3.1.5.1	DCS出现免费ARP不生效

【问题现象】<br>10月27日，应用访问DCS redis主备实例出现访问不通，导致客户业务中断1个小时。客户通过重启Redis实例恢复业务。

【问题根因】<br>当前问题根因未定位出来，当前定位的遗留问题：

1、	Redis主备实例在11:00:07~11:00:16出现VRRP报文没有收到，导致主备脑裂。为什么VRRP没有收到，导致网络脑裂，当前定位没有进展。

2、	主备网络脑裂恢复后，主节点重新发送的免费ARP没有生效，导致实例的VIP关联备机的MAC地址，当前从日志和构造的复现环境未能分析主节点发送免费ARP报文不生效的原因。

【问题改进分析】<br>
1、	DCS使用Keepalived软件，一旦出现主备倒换，新的主节点会发送免费ARP报文，VPC通过该报文刷新VIP关联的MAC地址，一旦报文丢失，将导致转发路由失败。当主备倒换过程，建议持续一段时间发送免费ARP，避免单个报文被丢弃或其他原因导致的广播没有发出，造成VIP关联错误的MAC地址。

2、	针对VIP绑定的MAC地址是否存在错误，需要具备监控能力，及时发现VIP。

3、	其他使用Keepalived或者使用VRRP实现高可靠的服务可能存在类似隐患，需要提升该问题场景的监控能力和提升的可靠性。

【问题排查过程】

1、DCS节点日志分析：<br>
11:00:16 备机打印切成Master的日志，该场景是备机没有收到主节点VRRP广播报文导致：<br>
```
Oct 27 03:00:19 dcs-vm-665741d7-server-1 Keepalived_vrrp[6962]: VRRP_Instance(redis) Entering MASTER STATE
```

11:00:17 主节点收到备机的VRRP广播报文，重新发送免费ARP广播
```
Oct 27 03:00:17 dcs-vm-665741d7-server-0 Keepalived_vrrp[6798]: VRRP_Instance(redis) Received lower prio advert, forcing new election
Oct 27 03:00:17 dcs-vm-665741d7-server-0 Keepalived_vrrp[6798]: VRRP_Instance(redis) Sending gratuitous ARPs on eth1 for 10.202.50.104
```

11:00:19 备机完成倒换，开始发送免费ARP，VPC收到该广播后，将VIP对应的MAC地址进行刷新，此时VIP被刷新成备机的MAC地址。
```
Oct 27 03:00:19 dcs-vm-665741d7-server-1 Keepalived_vrrp[6962]: VRRP_Instance(redis) Entering MASTER STATE
Oct 27 03:00:19 dcs-vm-665741d7-server-1 Keepalived_vrrp[6962]: VRRP_Instance(redis) setting protocol VIPs.
Oct 27 03:00:19 dcs-vm-665741d7-server-1 Keepalived_vrrp[6962]: VRRP_Instance(redis) Sending gratuitous ARPs on eth1 for 10.202.50.104
```

11:00:19 备机完成切主操作后，立即收到主节点的VRRP报文，由于主节点的VRRP报文等级高，因此备机（此时已经完成切主）立即摘除vip，并切成备状态
```
Oct 27 03:00:19 dcs-vm-665741d7-server-1 Keepalived_vrrp[6962]: VRRP_Instance(redis) Received higher prio advert
Oct 27 03:00:19 dcs-vm-665741d7-server-1 Keepalived_vrrp[6962]: VRRP_Instance(redis) Entering BACKUP STATE
Oct 27 03:00:19 dcs-vm-665741d7-server-1 Keepalived_vrrp[6962]: VRRP_Instance(redis) removing protocol VIPs.
```

11:00:20 主节点收到备节点的VRRP广播，由于等级比较低，因此主节点重新发送了免费ARP报文，但是这个报文并没有生效，导致vip的MAC地址没有被刷新过来，导致了客户redis业务持续访问不通。
```
Oct 27 03:00:20 dcs-vm-665741d7-server-0 Keepalived_vrrp[6798]: VRRP_Instance(redis) Received lower prio advert, forcing new election
Oct 27 03:00:20 dcs-vm-665741d7-server-0 Keepalived_vrrp[6798]: VRRP_Instance(redis) Sending gratuitous ARPs on eth1 for 10.202.50.104
Oct 27 03:00:20 dcs-vm-665741d7-server-0 Keepalived_vrrp[6798]: VRRP_Instance(redis) Received lower prio advert, forcing new election
Oct 27 03:00:20 dcs-vm-665741d7-server-0 Keepalived_vrrp[6798]: VRRP_Instance(redis) Sending gratuitous ARPs on eth1 for 10.202.50.104
```

2、VPC路由规则备份数据分析<br>
从VPC11:34备份的数据可以看出，当时vip关联的MAC地址是备机，由于备机没有vip，导致vip无法被访问。

3、DCS的网络监控分析<br>
从DCS的网络监控分析，主和备的负载都比较低，如下监控是间隔2秒采集的最大流量数据。

但是11：00整的时候，两个节点都出现重传包冲高的现象，说明当时Redis发送的包存在快重传或者丢包重传的情况。

4、从DCS OS后台节点系统日志，可以看到27号节点ping另一个redis节点出现比较多的10ms以上的ping包<br>

AZ之间ping的时延正常时1.3ms左右，但是看到27号超过10ms的ping包很多。不过ping超过10ms有两个是在11:00附近1~2分钟，时间没有完全匹配上。

5、实例的负载排查<br>
实例的CPU非常低，基本和空负载差不多；且从命令执行的情况看来，redis无时延高的命令，无慢日志。

6、客户重启后，新的主节点重传明显增高，抓包分析

通过抓包分析，发现重传包都是乱序后的快重传，重传比率非常低；而且对比重启前的主节点，发现主节点在运行中也有另行几个重传。排除重传问题。

7、物理网络排查分析

物理同事排查两个物理节点网络传输的各个环节，未发现有丢包问题；且各个环节流量带宽整体水平很低。

排查到交换机监控显示物理节点在11:00的时候出现流量骤降，从6Mbit/s降到4Mbit/s

经过排查，下降的水平和客户的业务流量相当，是网络异常后，流量正常水平的下降。

8、主备redis和客户端分别在AZ1、AZ2的不同POD，流量路经过CNA、L2GW、交换机等，经排查CNA、L2GW指标正常，未发现丢包；

9、查看L2GW ovs日志，未发现丢包；

10、查看CNA节点ovs日志、dump-ports计数、dump-loss计数，未发现丢包

11、问题时间点虚拟机监控压力正常，宿主机 CPU、内存压力正常，流量无突增

12、定位最后主节点发送的免费ARP为什么不生效，排查主备两台虚拟机对应的宿主机的OVS日志，发现主节点最后发的免费ARP在宿主机的日志上并没有打印，怀疑免费ARP报文没有发出来。


13、构造环境验证频繁发送免费ARP过程

通过创建1台虚拟机，绑定一个VIP，不断快速发送免费ARP报文：arping -U -I eth0 186.168.86.100

发现如果频繁发送免费ARP报文，后面发送的免费ARP报文并没有打印日志。

和DCS系统日志核对，发现DCS主节点发送免费ARP报文间隔3秒，通过3秒间隔测试，发现后面发送的免费ARP没有打印日志。解释了故障时间主节点发送免费ARP报文时候，在宿主机的OVS日志没有打印的情况。

14、构造两个节点，不断发送免费ARP报文的过程，发现两个节点间隔时间很短情况下，发送免费ARP是生效的。测试几次并没有发现间隔短的时间，两个节点快速发送免费ARP会出现MAC更新失败。

#### 3.1.5.2	DB出现免费ARP不生效

Keepalived问题

基于Keepalived1.2.13版本下会在一下四种情况主动发送免费ARP报文，广播通知其他节点目前浮动IP与MAC的对应关系

1. 从stop/fault/backup状态切换到master状态时，会主动发送10组arp报文

日志特征：
```
VRRP_Instance(VI_1) Transition to MASTER STATE
VRRP_Instance(VI_1) Entering MASTER STATE
VRRP_Instance(VI_1) setting Protocol VIPs.
VRRP_Instance(VI_1) Sending gratuitous ARPs on eth0 for 192.168.166.99
```

2. Master工作时，收到了低优先级的ARRP心跳报文，则主动发送arp报文

报文特征：
```
VRRP_Instance(VI_1) Received lower prio advert, forcing new election
VRRP_Instance(VI_1) Sending gratuitous ARPs on eth0 for 192.168.166.99
```

3. 其余服务发送arp请求报文，且此时没有arp冲突，发送arp相应。

报文特征：
```
Request who.has 192.168.166.99 tell 192.168.169.36, length 28
... ...
Reply 192.168.166.99 is-at fa:16:3e:62:54:5f, length  28
```

4.  增加garg_master_refresh参数，单位为秒，则Master会在间隔主动发送arp包

问题现象：<br>

多个业务节点无法连接DB服务，检查发现ping不通DB的浮动IP

组网：

1. DB采取双机部署，网卡为10.44.147.17（Slave）和10.44.147.18(Master), 浮动IP：10.44.147.26（VIP节点，业务需访问的节点IP）， 绑定在主节点的网卡上；
2. 业务节点与DB节点不在一个网段。

根因分析：

keepalive自身机制导致问题发生

优化建议：

在keepalived的配置文件里配置garp_master_refresh参数，比如设置为30， 这样主节点每30秒周期性发送arp光爆，保证在网络恢复会把正确的mac广播到交换机。

和用/usr/sbin/arping -l eth0(这里写具体网卡名） -s 10.44.147.26 -b 10.44.147.1 -c 1恢复的原理一样。

具体分析：

主节点keepalived没有日志，备节点的keepalived日志显示如下：

keepalived的原理是通过VRRP协议保持主备之间的心跳，通信检查。

当主备之前网络短暂的outstage发生，Slave节点将切换为master节点，并将浮动IP和Slave节点的mac光爆出去，交换机会更新ARP表项。

keepalived此时处于脑裂状态，2s后Slave节点收到higher prio请求，状态恢复为Slave节点。主节点因为一直持有浮动IP，所以没必要再次向交换机发ARP广播，最后交换机记录错误的ARP表项导致业务访问不通， 可以通过arpping发送正确的mac地址进行恢复

```
Sep 21:10:57:17 DLF-DB01 Keepalived_vrrp[1811]:VRRP_Instance(VI_1)Transition to MASTER STATE
Sep 21:10:57:17 DLF-DB01 Keepalived_vrrp[1811]:VRRP_Instance(VI_1)Entering MASTER STATE
Sep 21:10:57:17 DLF-DB01 Keepalived_vrrp[1811]:VRRP_Instance(VI_1)setting protocol VIPs.
Sep 21:10:57:17 DLF-DB01 Keepalived_vrrp[1811]:VRRP_Instance(VI_1)Sending gratuitous ARPs on eth0 for 10.44.147.26
Sep 21:10:57:17 DLF-DB01 Keepalived_healthcheckers[1810]: netlink reflector report 10.44.147.26 added
Sep 21:10:57:17 DLF-DB01 Keepalived_vrrp[1811]:VRRP_Instance(VI_1)Received higher prio advert
Sep 21:10:57:17 DLF-DB01 Keepalived_vrrp[1811]:VRRP_Instance(VI_1)Entering BACKIP STATE
Sep 21:10:57:17 DLF-DB01 Keepalived_vrrp[1811]:VRRP_Instance(VI_1)removing protocol VIPs
Sep 21:10:57:17 DLF-DB01 Keepalived_healthcheckers[1810]: netlink reflector report 10.44.147.26 removed
```

## 3.2	LVS

### 3.2.1	监控指标
| 指标名称	| 指标说明	| 原理说明	| 采集审计 |
| --- | --- | --- | --- |
| lvs_status	| 检查下ipvsadm –ln是否结果正常	|  | |
| lvs_is_master	| 是否是master节点		| | |
| lvs_dial_business_port	| 拨测业务端口是否正常	| 业务端口如果可以响应，说明业务层次端口是可以拨测通的。比如客户redis proxy集群，lvs充当负载均衡，如果通过lvs的vip和端口可以拨测业务，则说明lvs转发功能ok（即使密码不对，拨测的时候，能得到密码错误的提示，也说明lvs已经端到端转发ok）| | 	
| lvs_backend_status	| Lvs后端节点状态是否正常		| |

## 3.3	ETCD

#### 3.3.1.1	高可靠配置
ETCD使用Raft协议来维护集群内各个节点状态的一致性。简单说，ETCD集群是一个分布式系统，由多个节点相互通信构成整体对外服务，每个节点都存储了完整的数据，并且通过Raft协议保证每个节点维护的数据是一致的。每个ETCD节点都维护了一个状态机，并且，任意时刻至多存在一个有效的主节点。主节点处理所有来自客户端写操作，通过Raft协议保证写操作对状态机的改动会可靠的同步到其他节点。

### 3.3.2	监控

ETCD top监控指标及获取方式：

| 指标名称	| 指标说明	| 原理说明	| 采集审计 |
| --- | --- | --- | --- |
| etcd_health	| 集群监控状态	| HEALTH_CMD = 'curl http://{etcd监听ip}:%s/health -s'%PORT，通过调用ectd  health接口的形式进行获取 |  |
| etcd_process_status	| 进程状态	| HEALTH_CMD = 'curl http://{etcd监听ip}:%s/health -s'%PORT，通过调用ectd接口的形式进行获取| |
| etcd_server_has_leader	| Etcd集群是否有主节点。| 	HEALTH_CMD = 'curl http://{etcd监听ip}:%s/health -s'%PORT，通过调用ectd接口的形式进行获取| |
| Etcd_server_heartbeat_send_failures_total	| Etcd心跳失败数	| HEALTH_CMD = 'curl http://{etcd监听ip}:%s/health -s'%PORT，通过调用ectd接口的形式进行获取| | 	
| process_resident_memory_mb	| Etcd占用内存量	| HEALTH_CMD = 'curl http://{etcd监听ip}:%s/health -s'%PORT，通过调用ectd接口的形式进行获取| | 	
| disk_wal_fsync_duration_seconds	| Etcd同步时间均值 | 	HEALTH_CMD = 'curl http://{etcd监听ip}:%s/health -s'%PORT，通过调用ectd接口的形式进行获取| |
| http_request_duration_microseconds_count | 	| 	HEALTH_CMD = 'curl http://{etcd监听ip}:%s/health -s'%PORT，通过调用ectd接口的形式进行获取| |
| debugging_mvcc_keys_total	| Etcd key数量	| HEALTH_CMD = 'curl http://{etcd监听ip}:%s/health -s'%PORT，通过调用ectd接口的形式进行获取| | 	
| server_slow_apply_total	| Etcd 慢请求数量	| HEALTH_CMD = 'curl http://{etcd监听ip}:%s/health -s'%PORT，通过调用ectd接口的形式进行获取| | 	
| disk_backend_commit_duration_seconds	| Etcd　做数据存储的时间。	| HEALTH_CMD = 'curl http://{etcd监听ip}:%s/health -s'%PORT，通过调用ectd接口的形式进行获取| |

### 3.3.3	故障场景、定界、应急恢复

| 故障场景	| 产生原因	| 故障对应的监控指标	| 故障定界	| 故障恢复 |
| --- | --- | --- | --- | --- |
| Etcd集群member异常	| Etcd member运行时突然异常 | 	 Etcdctl  status 命令进行监控 集群的状态	| | 	摘除异常节点<br>etcdctl member remove $ID |
| 数据丢失	| 集群异常或者被程序删掉	| 对Etcd 数据量进行监控| | 		etcdctl snapshot restore snapshot.db|

## 3.4	JRE

### 3.4.1	监控指标
| 指标名称	| 指标说明	| 原理说明	| 采集审计 |
| --- | --- | --- | --- |
| jvm_heap_use_rate	| 堆内存使用率		| | |
| jvm_last_major_gc_time	| 上次full gc的时间	| | | 	
| jvm_major_gc_count	| 周期采集时间内，full gc执行次数		| | |
| jvm_last_minor_gc_time	| 上次ygc的时间	| | | 	
| jvm_thread_ount	| 当前线程数		| | |


### 3.4.2	故障场景、定界、应急恢复

## 3.5	Tomcat

### 3.5.1	指标

| 指标名称	| 指标说明	| 原理说明	| 采集审计 |
| --- | --- | --- | --- |
| tomcat_listening_port	| 端口是否启动	| 如果配置问题，或者秘钥相关的错误，可能导致进程起来后，端口监听器没有启动。	| |
| tomcat _request_num	| 请求的总数		| | |
| tomcat_request_5xx_num	| | | | 		
| tomcat_request_4xx_num	| | | | 		
| tomcat_request_2xx_num	| | | | 		
| tomcat_request_success_rate	| 请求的成功率	| | | 	
| tomcat_request_max_delay	| 请求的最大时延		| | |

### 3.5.2	告警

#### 3.5.2.1	高可靠配置

### 3.5.3	故障场景、定界、应急恢复

| 故障场景	| 产生原因	| 故障对应的监控指标	| 故障定界	| 故障恢复 |
| --- | --- | --- | --- | --- |
| 启动tomcat时，报"Property File Not Found"	| 找不到secretAppkey.properties文件		| | 查看/opt/cloud/${Component_A}/conf/keystore/application.properties文件的secas_keystore_appkey_file属性是否设置为secretAppkey.properties的绝对路径	| 查看/opt/cloud/${Component_A}/conf/keystore/application.properties文件的secas_keystore_appkey_file属性是否设置为secretAppkey.properties的绝对路径，不设置默认为/opt/cloud/3rdComponent/tomcat/conf/keystore路径。|
| tomcat启动慢	| 由于机器熵源不够，熵值低导致获取安全随机数阻塞	| 机器熵值		| | |

# 4	服务使用运维能力
数据面经常出现服务间相互依赖。如果一个服务使用另一个服务，按照PaaS服务的设计理念，使用者只关注使用即可，不需要关注底层的运维细节。
该章节需要把云服务使用需要关注的点，以及配置什么监控总结清楚，促进云服务/客户更好使用基础云服务。

## 4.1	Mysql（关注下数据面使用RDS需要在CMT配置什么指标）

## 4.2	Redis（关注下数据面使用DCS需要在CMT配置什么指标）

## 4.3	Kakfka（关注下数据面使用DMS需要在CMT配置什么指标）
