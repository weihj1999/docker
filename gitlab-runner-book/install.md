# 安装Gitlab Runner
Gitlab Runner可以在GNU/Linux, macOS, FreeBSD以及Windows操作系统上，支持使用docker安装，也支持binary手工安装，或者使用rpm/deb包安装（通常需要用到Gitlab提供的源）

## 在容器中运行Gitlab Runner
## 1. 镜像
Gitlab Runner容器镜像（基于Ubuntu或者Alpine linxu构建）被设计为可以运行标准gitlab-runner命令的包装器，就像在主机上运行gitlab-runner一样。

gitlab-runner的运行

    gitlab-runner [Runner command and options...]

在容器里面封装了gitlab-runner之后，运行形式：

    docker run [chosen docker options...] gitlab/gitlab-runner [Runner command and options...]
看起来的输出如下：

    docker run --rm -t -i gitlab/gitlab-runner --help

    NAME:
    gitlab-runner - a GitLab Runner

    USAGE:
    gitlab-runner [global options] command [command options] [arguments...]

    VERSION:
    10.7.0 (7c273476)

    (...)

简而言之，gitlab-runner命令被docker run [docker options] gitlab/gitlab-runner 替换了，其他部分保持不变，不同的事实gitlab-runner的命令在容器内执行，而不是主机上。

通常为了保持体验一致性，主机可以设置别名alias.

## 2. 容器镜像安装

**Step 1.** 安装dokcer

    curl -sSL https://get.docker.com/ | sh
**Step 2.** 挂载一个配置卷

    docker run -d --name gitlab-runner restart always \
      -v /srv/gitlab-runner/config:/etc/gitlab-runner \
      -v /var/run/docker.sock:/var/run/docker.sock \
      gitlab/gitlab-runner:latest
或者使用一个配置容器来挂载定制的数据卷,

    docker run -d --name gitlab-runner-config \
      -v /etc/gitlab-runner \
      busybox:latest \
      /bin/true
然后运行runner，

    docker run -d --name gitlab-runner --restart always \
      -v /var/run/docker.sock:/var/run/docker.sock \
      --volumes-from gitlab-runner-config \
      gitlab/gitlab-runner:latest
**Step 3.** 注册

下面就是注册容器的过程了，请参考下一章节，runner在注册成功之前，不会执行任何任务
