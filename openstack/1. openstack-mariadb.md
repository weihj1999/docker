# 1. MariaDB的高可用主备数据库容器实践

设计的想法是：
1. 追求多容器实例，主备部署
2. 次之支持数据库定期备份，恢复
3. 为了部署的简单，不考虑使用swarm或者k8s进行容器的调度和编排。
4. 测试调通后，交给自研调度部署软件处理；


网上有一个实践：

https://severalnines.com/database-blog/running-mariadb-galera-cluster-without-orchestration-tools-db-container-management

这个也不错，支持读写分离，但是涉及其他管理器，生产可以考虑；
https://github.com/erasys/mariadb-ha-test
