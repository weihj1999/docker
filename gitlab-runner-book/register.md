# 注册 Gitlab Runner
安装Gitlab Runner后需要进行注册，当然是注册到gitlab，否则任何任务都没有执行的地方，我们称注册好了的gitlab-runner为执行机。

## 前提条件
- gitlab服务器
- 从gitlab取得一个token，（shared or specific）

## GNU/Linux注册
1. 运行命令：
```
sudo gitlab-runner register
```
2. 输入需要注册的gitlab的URL
```
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com )
https://gitlab.com
```
3. 输入token
```
Please enter the gitlab-ci token for this runner
xxx
```
4. 输入一个简单的描述
```
Please enter the gitlab-ci description for this runner
[hostname] my-runner
```
5. 输入与该执行机关联的标签（tag）  
```
Please enter the gitlab-ci tags for this runner (comma separated):
my-tag,another-tag
```
6. 输入执行器
```
Please enter the executor: ssh, docker+machine, docker-ssh+machine, kubernetes, docker, parallels, virtualbox, docker-ssh, shell:
docker
```
7. 如果选择docker作为执行器，需要额外输入默认使用的镜像
```
Please enter the Docker image (eg. ruby:2.1):
alpine:latest
```

## 使用docker作为执行机的注册方式

上一节我们介绍了如何在容器里面运行gitab-runner，下面介绍如何在这种方式下进行注册。

需要注意：注册完成后，相应的配置信息会记录在我们指定的配置卷
**/srv/gitlab-runner/config**， 执行机会自动加载这些配置。


使用docker容器注册一个执行机（runner）
1. 运行注册命令

        docker run --rm -t -i -v /srv/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner register
2. 输入Gitlab的URL
```
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com )
https://gitlab.com
```
3. 输入token
```
Please enter the gitlab-ci token for this runner
xxx
```
4. 输入描述
```
Please enter the gitlab-ci description for this runner
[hostname] my-runner
```
5. 输入与该执行机关联的标签（tags)
```
Please enter the gitlab-ci tags for this runner (comma separated):
my-tag,another-tag
```
6. 输入执行器
```
Please enter the executor: ssh, docker+machine, docker-ssh+machine, kubernetes, docker, parallels, virtualbox, docker-ssh, shell:
docker
```
7. 使用docker执行器的情况下，输入默认的镜像
```
Please enter the Docker image (eg. ruby:2.1):
alpine:latest
```

## 使用一条命令完成注册
上面的操作手册，需要用户进行交互式输入，这里介绍通过一行命令进行注册，通常叫做non-interactive模式注册。

查看register子命令选项

    gitlab-runner register -h
使用常用的选项注册：

    sudo gitlab-runner register \
      --non-interactive \
      --url "https://gitlab.com/" \
      --registration-token "PROJECT_REGISTRATION_TOKEN" \
      --executor "docker" \
      --docker-image alpine:latest \
      --description "docker-runner" \
      --tag-list "docker,aws" \
      --run-untagged="true" \
      --locked="false" \
      --access-level="not_protected"
如果使用docker容器运行gitlab-runner，命令如下：

    docker run --rm -v /srv/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner register \
      --non-interactive \
      --executor "docker" \
      --docker-image alpine:latest \
      --url "https://gitlab.com/" \
      --registration-token "PROJECT_REGISTRATION_TOKEN" \
      --description "docker-runner" \
      --tag-list "docker,aws" \
      --run-untagged="true" \
      --locked="false" \
      --access-level="not_protected"
## runner配置模板文件[[runners]]
有一部分runner的配置信息不支持以环境变量方式调用，比如说：

- 环境变量不支持切片
- 整个Kubernetes执行器存储卷配置的设置在全局上都没有命令行选项支持。

对于任何自动化处理的环境（例如GitLab Runner官方Helm chart），这都是一个问题。 在这种情况下，唯一的解决方案是在Runner被注册后手动更新config.toml文件。 这并不是最理想的，容易出错并且不可靠。 特别是当同一Runner安装程序完成多个注册时。

使用配置模板文件可以解决此问题。使用配置模板时，需要传递一个文件路径，两种方法：
- 使用命令行选项 --template-config.
- 使用环境变量 TEMPLATE_CONFIG_FILE environment variable.

配置模板文件要求：
- 只能由一个[[runners]]
- 不能有全局选项

当使用 **--template-config** 或者 **TEMPLATE_CONFIG_FILE** 时，**[[runners]]** 的配置信息会合并到config.toml文件中：
仅对空选项进行合并，包含：
- Empty strings.
- Nulls or/non existent entries.
- Zeroes.

### 举例
比如注册一个k8s的执行器，config.toml文件参考如下：

    $ sudo gitlab-runner register \
         --config /tmp/test-config.toml \
         --non-interactive \
         --url https://gitlab.com \
         --registration-token __REDACTED__ \
         --name test-runner \
         --tag-list kubernetes,test \
         --locked \
         --paused \
         --executor kubernetes \
         --kubernetes-host http://localhost:9876/

    Runtime platform                                    arch=amd64 os=linux pid=1684 revision=88310882 version=11.10.0~beta.1251.g88310882

    Registering runner... succeeded                     runner=__REDACTED__
    Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!

上述命令会创建下面的config.toml文件:

    concurrent = 1
    check_interval = 0

    [session_server]
      session_timeout = 1800

    [[runners]]
      name = "test-runner"
      url = "https://gitlab.com"
      token = "__REDACTED__"
      executor = "kubernetes"
      [runners.cache]
        [runners.cache.s3]
        [runners.cache.gcs]
      [runners.kubernetes]
        host = "http://localhost:9876/"
        bearer_token_overwrite_allowed = false
        image = ""
        namespace = ""
        namespace_overwrite_allowed = ""
        privileged = false
        service_account_overwrite_allowed = ""
        pod_annotations_overwrite_allowed = ""
        [runners.kubernetes.volumes]

从上面可以看出来基本的信息创建出来了
- 执行机的认证信息（URL和token）
- 执行执行器
- 默认情况下，空白的runner.kubernetes(除了指定的相关信息，比如K8S的主机信息)

通常情况下，需要设置一些其他选项以使Kubernetes执行程序可用，但是对于我们的例子而言，以上内容就足够了。

我们假设，我们需要配置一个 **emptyDir** 卷给k8s执行器，然而在注册的时候却不支持这么操作，所以我们需要手工附加一些配置信息到config.toml文件：

    [[runners.kubernetes.volumes.empty_dir]]
      name = "empty_dir"
      mount_path = "/path/to/empty_dir"
      medium = "Memory"
在Gitlab 12.2后，使用 **--template-config** 事情就变得简单多了.

    $ cat > /tmp/test-config.template.toml << EOF
    [[runners]]
      [runners.kubernetes]
        [runners.kubernetes.volumes]
          [[runners.kubernetes.volumes.empty_dir]]
            name = "empty_dir"
            mount_path = "/path/to/empty_dir"
            medium = "Memory"
    EOF
有了这个文件，我们现在就需要再次注册执行机，但是这一次，添加
**--template-config /tmp/test-config.template.toml**
选项，  除了这部分的信息之外，其他信息和上面的都是一样的。

    $ sudo gitlab-runner register \
         --config /tmp/test-config.toml \
         --template-config /tmp/test-config.template.toml \
         --non-interactive \
         --url https://gitlab.com \
         --registration-token __REDACTED__ \
         --name test-runner \
         --tag-list kubernetes,test \
         --locked \
         --paused \
         --executor kubernetes \
         --kubernetes-host http://localhost:9876/

    Runtime platform                                    arch=amd64 os=linux pid=8798 revision=88310882 version=11.10.0~beta.1251.g88310882

    Registering runner... succeeded                     runner=__REDACTED__
    Merging configuration from template file
    Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!

从下面的config.toml文件可以看出来效果，

    concurrent = 1
    check_interval = 0

    [session_server]
      session_timeout = 1800

    [[runners]]
      name = "test-runner"
      url = "https://gitlab.com"
      token = "__REDACTED__"
      executor = "kubernetes"
      [runners.cache]
        [runners.cache.s3]
        [runners.cache.gcs]
      [runners.kubernetes]
        host = "http://localhost:9876/"
        bearer_token_overwrite_allowed = false
        image = ""
        namespace = ""
        namespace_overwrite_allowed = ""
        privileged = false
        service_account_overwrite_allowed = ""
        pod_annotations_overwrite_allowed = ""
        [runners.kubernetes.volumes]

          [[runners.kubernetes.volumes.empty_dir]]
            name = "empty_dir"
            mount_path = "/path/to/empty_dir"
            medium = "Memory"

大部分内容和前面的是一样的， 唯一添加的就是<br>
**[[runners.kubernetes.volumes.empty_dir]]**

需要重点强调以下，gitlab-runner注册时指定的参数的优先级比通过模板文件传入的参数的优先级高。
