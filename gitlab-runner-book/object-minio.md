# minio对象存储

## 简介
![mino log](/images/minio-logo.svg)
官方github：
https://github.com/minio/minio

MinIO is an object storage server released under **Apache License v2.0**. It is compatible with Amazon **S3** cloud storage service. It is best suited for storing unstructured data such as photos, videos, log files, backups and container / VM images. Size of an object can range from a few KBs to **a maximum of 5TB**.

MinIO server is light enough to be bundled with the application stack, similar to NodeJS, Redis and MySQL.

Minio支持docker部署
稳定版本：

    docker pull minio/minio
    docker run -p 9000:9000 minio/minio server /data
Edge版本：

    docker pull minio/minio:edge
    docker run -p 9000:9000 minio/minio:edge server /data

支持使用本地持久存储进行部署，从运行docker主机上的本地存储映射到容器存储：

    docker run -p 9000:9000 --name minio1 \
      -v /mnt/data:/data \
      minio/minio server /data
## minio使用小技巧
1. 定制AK/SK
- 支持把AK/SK作为环境变量传入
- 支持设置正则字符串作为AK/SK

    docker run -p 9000:9000 --name minio1 \
      -e "MINIO_ACCESS_KEY=AKIAIOSFODNN7EXAMPLE" \
      -e "MINIO_SECRET_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY" \
      -v /mnt/data:/data \
      minio/minio server /data
2. 支持非root用户运行minio容器

        mkdir -p ${HOME}/data
        docker run -p 9000:9000 \
          --user $(id -u):$(id -g) \
          --name minio1 \
          -e "MINIO_ACCESS_KEY=AKIAIOSFODNN7EXAMPLE" \
          -e "MINIO_SECRET_KEY=wJalrXUtnFEMIK7MDENGbPxRfiCYEXAMPLEKEY" \
          -v ${HOME}/data:/data \
          minio/minio server /data
3. 支持使用docker密钥定制AK/SK

        echo "AKIAIOSFODNN7EXAMPLE" | docker secret create access_key -
        echo "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY" | docker secret create secret_key -

然后创建一个docker service来读取docker密钥

      docker service create --name="minio-service" --secret="access_key" --secret="secret_key" minio/minio server /data

4. 获取minio容器id

        docker ps -a

同时可以通过docker的其他指令来操作minio容器

    docker start <container_id>
    docker stop <container_id>
    docker logs <container_id>
    docker stats <container_id>



## minio支持分布式部署
在主机上不多多个minio容器实例
https://docs.min.io/docs/deploy-minio-on-docker-compose

## 部署我们自己的对象存储

重点调研基于Docker Compose和Swarm的分布式MinIO部署的方法，详情参考MinioBook。
