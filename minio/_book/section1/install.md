# 第二节 MinIO Docker安装
## 2.1 前提条件（Prereqisites）
要求docker需要提前安装好；可参考[官方链接](https://www.docker.com/community-edition#/download)
## 2.2 使用容器运行单机版MinIO
MinIO需要使用持久存储卷（persistent volume）来存储配置和应用数据，在测试的情况下，可以使用本地目录启动MinIO，比如说 */data* 在容器启动的时候会在容器的文件系统中创建这个目录 */data* ，但是如果容器推出，所有数据就丢失了。

    docker run -p 9000:9000 minio/minio server /data
为了使用持久存储（persistent storage）创建MinIO容器，需要映射本地的持久目录（persistent directory）,使用 *~/miniodata*， 并且导出为/data目录，通过下面的命令执行

    docker run -p 9000:9000 --name minio1 \
      -v /mnt/data:/data \
      minio/minio server /data
  ## 2.2 使用容器运行分布式MinIO
  分布式MinIO可以通过[Docker Compose](https://docs.min.io/docs/deploy-minio-on-docker-compose)或者[Swarm mode](https://docs.min.io/docs/deploy-minio-on-docker-swarm)部署，二者最大的不同是，Docker Compose在一个主机上创建多个容器实例部署，而Swarm模式可以在多个主机上创建多个容器实例。


  因此可以看出来使用Docker Compose可以快速在计算机上创建分布式的MinIO，用来做开发，测试，staging环境。而通过Swarm部署可以提供强壮的，生产级别的部署。

  具体内容请参考高级部署；
