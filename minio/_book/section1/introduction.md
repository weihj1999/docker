# 第一节 快速入门
## 1.1 基本介绍
MinIO 支持对象存储的服务器产品，完全开源，遵从 **Apache License v2.0** 协议. 兼容Amazon S3。用于存储非结构化数据，比如照片，视频，日志文件，备份以及容器或者虚拟机镜像。支持的文件大小从几KB到 **5TB** 不等.

MinIO server is light enough to be bundled with the application stack, similar to NodeJS, Redis and MySQL.

MionIO在缺省模式下，为了更快体验，不计算MD5SUM，这个能力会带来一下兼容性问题，比如说s3ql等严重依赖MD5SUM的s3客户端。 这种情况下，可以在启动服务的时候，加上参数选项 **--compat**。

    minio --compat server /data

## 1.2 MinIO支持docker安装
稳定版本：

    docker pull minio/minio
    docker run -p 9000:9000 minio/minio server /data
Edge版本：

>备注： docker不显示自动生成的key，在-it交互模式下才会显示。通常情况下，不推荐使用自动生产的key。

## 1.2 防火墙端口
通常情况下，MinIO使用端口9000来监听接入的连接。通常在GUN/Linux系统下，使用 *iptables* 命令来打开该端口

    iptables -A INPUT -p tcp --dport 9000 -j ACCEPT
    service iptables restart
建议打开9000到9010端口

    iptables -A INPUT -p tcp --dport 9000:9010 -j ACCEPT
    service iptables restart

CentOS系统支持firewall-cmd

    firewall-cmd --get-active-zones
    firewall-cmd --zone=public --add-port=9000/tcp --permanent
    firewall-cmd --reload

## 1.3 MinIO支持浏览器访问
![minio浏览器](/images/minio-browser.PNG)

## 1.4 MionIO客户端
mc提供了命令行工具，支持文件系统和S3

## 1.5 MinIO的升级
MinIO支持滚动升级，比如说在一个分布式集群下升级其中一个MinIO实例，从而实现不中断升级。推荐使用 *mc admin update* 进行升级，他会自动更新集群中所有节点，并重启服务。

    mc admin update <minio alias, e.g., myminio>
- *mc admin update* 只有你当用户对可执行文件所在的父目录有写入权限的时候才你能正常工作。比如如果升级包在/usr/local/bin/minio，则用户需要对/usr/local/bin/目录有写入权限；
- 如果需要更新服务器，则推荐在所有服务器通过 *mc upatte* 升级后，更新mc，
- *mc admin update* 在docker容器环境下禁止使用，dockr容器环境下有另外一套机制来更新容器
- 如果使用Vault作为MinIO的KMS，需要确保按照在线文档完成Vault升级流程。 [官网在线手册](https://www.vaultproject.io/docs/upgrading/index.html)
- 如说使用etcd来做联邦，需要确保按照在线手册完成etcd的升级流程[官方在线升级手册](https://github.com/etcd-io/etcd/blob/master/Documentation/upgrades/upgrading-etcd.md)
