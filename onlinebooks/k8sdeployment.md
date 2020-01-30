# Kubernetes Deployment 终极指南

原文： [Kubernetes Deployments: The Ultimate Guide](https://semaphoreci.com/blog/kubernetes-deployment)

要把容器化的应用部署起来？在 Kubernetes 中部署容器化应用，总要涉及到 Deployment，这里有这个对象的所有内容。

我们最早学会的 Kubernetes 命令之一就是 *kubectl run*。具备 Docker 经验的用户，不免会用 *docker run* 命令和这个命令进行对比，结论可能是：运行容器就是这么简单。

我们来看看，在运行一个基本的 kubectl run 命令的时候，都发生了些什么：
```
$ kubectl run web --image=nginx
deployment.apps/web created
```
集群中创建了什么？
```
$ kubectl get all
NAME                       READY     STATUS    RESTARTS   AGE
pod/web-65899c769f-dhtdx   1/1       Running   0          11s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   46s

NAME                  DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/web   1         1         1            1           11s

NAME                             DESIRED   CURRENT   READY     AGE
replicaset.apps/web-65899c769f   1         1         1         11s
```
我们并没有看到容器，而是一组未知对象：

- Deployment：web
- ReplicaSet：web-65899c769f
- Pod：web-65899c769f-dhtdx

此处的 kubernetes 服务可以忽略，它在我们运行命令之前就已经存在了。

我只想要个容器！为什么看到了三个不同的对象？

简单说来，这些 Kubernetes 对象能在不停服务的情况下，为应用提供渐进式部署、回滚以及伸缩的支持。

初次见面难免会好奇：究竟是怎么回事？在了解这些问题之后，就会理解每个对象的角色和存在价值了。

持续集成提升了对代码的信心。要把这种信心扩展到发布流程之中，部署操作就需要更多保障。

## 容器和 Pod
在 Kubernetes 中，一个 Deployment 的最小单元不是容器，而是 Pod。Pod 是一组容器（当然这一组也可以只有一个），它们运行在同一台服务器中，并共享一些资源。

例如 Pod 中的容器能够通过 localhost 互相通信。在网络视角中，这些容器中的所有进程都是本地的。

但是我们永远无法创建独立的容器：最相近的操作也只能是创建一个仅包含单一容器的一个 Pod。

我们想让 Kubernetes 创建 NGINX，完整的台词是：“我要一个 Pod，其中只包含一个容器，这个容器运行的是 nginx 镜像”。
```YAML
# pod-nginx.yml
# Create it with:
#    kubectl apply -f pod-nginx.yml
apiVersion: v1
kind: Pod
metadata:
  name: web
  spec:
    containers:
      - image: nginx
      name: nginx
      ports:
        - containerPort: 80
        name: http
```
这就只有一个 Pod，那 ReplicaSet 和 Deployment 是怎么回事？

## 指令和声明
Kubernetes 是一个声明式系统（和指令式系统相对），这就意味着我们无法给它发出命令。我们不能说：“运行这个容器”。我们能做的只能是——描述我们需要的东西，然后等 Kubernetes 根据现有内容，同步为预期内容。

打个比方，我们可以说：“我要一个 40 英尺高的有黄色门的蓝色容器”，Kubernetes 会为我们查找这种容器，如果找不到，就会创建一个；如果已经有了，但它是绿色红门的，Kubernetes 就会帮我们上色；如果已经有了完全符合要求的容器，因为现有内容和预期内容一致，所以 Kubernetes 什么都不会做。

回到软件容器的话题，我们可以说：“我想要一个名字叫 web 的 Pod，其中应该有单独的容器，运行的是 nginx 镜像”。

如果这个 Pod 不存在，Kubernetes 会创建出来。如果符合我们要求的 Pod 已经存在，Kubernetes 无需进行任何动作。

基于这种思路，怎样对 web 应用进行伸缩，来满足多容器或 Pod 的运行需要呢？

### ReplicaSet 简化了 Pod 的伸缩过程

如果我们只有一个 Pod，我们想要更多的同样的 Pod，我们可能会给 Kubernetes 提出这样的要求：“我们需要一个叫做 web2 的 Pod，具体要求是：…”，然后重复之前的 Pod 规范。想要多少 Pod，就重复执行多少次。

这明显很不方便，我们要自己跟踪所有的 Pod，确保它们都同步了正确的状态，并符合特定的规范。

Kubernetes 提供了高级一些的抽象来简化这个过程：ReplicaSet。ReplicaSet 的对象结构和 Pod 很相似，只不过它还有个副本数量的字段，用于描述我们所需要的符合规范的 Pod 数量。

有了 ReplicaSet，我们就可以告诉 Kubernetes：“我需要一个叫做 web 的 ReplicaSet，其中包含 3 个 Pod，这些 Pod 符合如下规范：……”，Kubernetes 会根据这个指令来确认，是不是刚好有三个符合规范的 Pod。如果我们从头开始，就会创建这 3 个 Pod。如果已经有了 3 个 Pod，什么事都不会发生——我们的要求和现状一致。
```YAML
# pod-replicas.yml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: web-replicas
  labels:
    app: web
    tier: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        app: web
        tier: frontend
    spec:
      containers:
        - name: nginx
          image: nginx
          ports:
        - containerPort: 80
```

### ReplicaSet 的伸缩和高可用
我们可以修改现存 ReplicaSet 的副本数量，以此来完成伸缩。Kubernetes 会根据伸缩指令来创建或删除 Pod，让 Pod 数量符合要求。

高可用方面，因为 Kubernetes 会持续的对集群进行监控，确保无论什么情况下都保有指定数量的运行实例。

如果节点当机，恰好其中有一个 web 所属的 Pod，Kubernetes 会另外创建一个 Pod 来替换它。如果节点没有当机，不过是有一段时间无法联系或者没有响应，那么它再次恢复可用之后，就会多出一个 Pod，Kubernetes 会中止一个 Pod 来保证数量符合要求。

### 修改 Pod 定义会发生什么
修改 Pod 定义并不罕见。比如我们经常会希望把容器镜像替换为新版本。

记住：ReplicaSet 的使命是，“确保有 N 个符合规范的 Pod。”如果我们修改了定义，会发生什么呢——突然就没有符合新规范的 Pod 了。

写到这里，我们已经知道了声明式系统的工作方式：Kubernetes 会立刻创建 N 个符合新规范的 Pod。旧的 Pod 会一致存在，直到我们手工清理。

如果能用 CI/CD 对这些过期 Pod 做一个自动清理可能不错；如果新 Pod 的创建能用更优雅的方式也会更好。

### Deployment 驱动的 ReplicaSet
前面说的需要就是 Deployment 的职责。粗看上去，Deployment 的规范和 ReplicaSet 很像：其中包含了 Pod 规范，以及副本数量。（还有一些后面会讨论的参数）
```YAML
# deployment-nginx.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 3
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```
Deployment 并不会直接负责 Pod 的创建和删除。它会把这些工作委托给一个或多个 ReplicaSet。

在我们创建 Deployment 的时候，它会用自己的 Pod 规范创建一个 ReplicaSet。

当更新一个 Deployment 并修改副本数量时，它会把更新内容传递给下游的 ReplicaSet。

### 当配置发生了变化
需要更新 Pod 规范的时候，事情就有意思了。例如我们可能需要使用新版本的镜像（因为我们发布了新的版本），或者修改应用的参数（通过命令行参数、环境变量或者配置文件）。

在我们更新 Pod 规范时，Deployment 会用新的 Pod 规范创建新的 ReplicaSet。新的 ReplicaSet 的初始实例数量是 0。接下来 ReplicaSet 的实例数量会逐步提升，同时逐渐减少另一个 ReplicaSet 的尺寸。

可以想象一下，面前有个混音台，我们要让新的 ReplicaSet 淡入，同时把旧的那个淡出。

整个过程之中，请求被发送给新旧两个 ReplicaSet，用户不会感觉服务中断。

全景大致如此，其中还有很多小细节，让整个过程更加健壮。

### 损坏的 Deployment 以及就绪检测

如果我们推出了一个故障版本，因为 Kubernetes 会持续把旧 Pod 替换成新的（故障）版本，它可能会让整个应用坏掉（逐个 Pod）。

除非我们用上了就绪检测。

就绪检测是在容器规范中加入的一个测试过程。他是一个二进制测试，结果只有两个“能行”或者“不行”，这个测试会以指定的间隔被执行（缺省情况下是每 10 秒）。

Kubernetes 支持三种方式的就绪检测：

- 在容器内运行一个命令；
- 向容器发出一个 HTTP(S) 请求；
- 向容器发起一个 TCP 连接。

Kubernetes 会通过测试结果来了解容器及其所处 Pod 是否准备就绪可以接受流量。在我们推出新版本时，Kubernetes 会等到新 Pod 测试得到“就绪”结果之后，才会进入下一步。

如果一个 Pod 因为就绪检测持续失败，永远无法进入就绪状态，Kubernetes 也不会进入下一步。部署过程会停止，应用会继续使用老版本运行，直到我们解决了问题。

如果没有就绪检测，那么这个容器成功启动后就会被当成是就绪状态。所以最好能使用就绪检测来保障业务。

### 使用 Rollback 来从故障版本中快速恢复
在滚动更新过程中或之后的任何时间，我们都可以告诉 Kubernetes：“我改主意了，请回到这个 Deployment 的前一个版本。”，这个操作会切换新旧 ReplicaSet 的地位。在这个点开始，会提高旧版 ReplicaSet 的实例数量到指定数值，同时降低新版的的实例数量。

一般来说，并不限于新旧两个 ReplicaSet。归根结底，有一个 ReplicaSet 被视为“最新”版本，我们可以将这个版本作为目标 ReplicaSet，所谓目标，就是我们希望运行的，也是 Kubernetes 会逐步拉起的一个版本。同时也可以有任意多个其它版本的 ReplicaSet，对应旧版本。

例如我们在运行 10 个副本的版本 1 应用，然后开始推出版本 2。在某个时间点，我们可能有了 7 个版本 1、3 个版本 2 的 Pod 正在运行。如果我们不想等版本 2 完全推出，决定推出版本 3。在版本 3 部署的时候，我们又想回到版本 1。整个过程，Kubernetes 都会根据需要对各个版本的 ReplicaSet 中的副本数量进行调整。

## MaxSurge 和 MaxUnavailable
Kubernetes 不一定是一次更新一个 Pod 的。之前我们提到 Deployment 还有一些额外的参数，这些参数中包括了 MaxSurge 和 MaxUnavailable，这两个参数决定了更新过程的速度。

试想一下，推出新版本过程中的两个策略：

我们可能对应用的可用性非常谨慎，因此决定在关闭旧版本 Pod 之前，首先要启动新 Pod。只有新 Pod 启动、运行并就绪之后，才终结旧 Pod。

上这个假设中有个隐含条件就是我们的集群中是有剩余资源的。然而如果我们的集群已经满载，无法负担多余 Pod 的消耗，那么我们自然是希望首先关掉旧的，然后才启动新的。

MaxSurge 指出了我们在滚动更新时，可以有多少个额外的 Pod；而 MaxUnavailable 则代表在滚动更新时，我们可以忍受多少个 Pod 无法提供服务。这两个参数可以是 Pod 数量，也可以是 Deployment 的实例数量百分比；两个参数都可以设置为 0（但是不能同时为 0）。

接下来看看这两个参数的常见取值，以及背后的意图。

MaxUnavailable 设置为 0 意味着：“在新 Pod 启动并就绪之前，不要关闭任何旧 Pod”。

MaxSurge 设置为 100% 的意思是：“立即启动所有新 Pod”，也就是说我们有足够的资源，我们希望尽快完成更新。

这两个参数的却升值都是 25%，如果我们更新一个 100 Pod 的 Deployment，会立刻创建 25 个新 Old，同时会关闭 25 个旧 Pod。每次有 Pod 启动就绪，就可以关闭旧 Pod。每次有旧 Pod 完成关闭过程（释放资源），就可以创建另一个新 Pod 了。

## 演示时间
可以很方便的观察这些参数的作用。我们不需要编写自己的 YAML、定义就绪检测等东西。

我们需要做的事情只是，使用一个无效的镜像，例如一个不存在的镜像。这个容器永远无法启动，Kubernetes 也永远无法把它标记为就绪。

如果你有个 Kubernewtes 集群（Minikube 或者 Docker 桌面版的单结点集群都可以），可以在不同终端运行下面的命令，来看看发生了什么：
```
kubectl get pods -w
kubectl get replicasets -w
kubectl get deployments -w
kubectl get events -w
```
然后用下面的命令来创建、伸缩以及更新一个 Deployment：
```
kubectl run deployment web --image=nginx
kubectl scale deployment web --replicas=10
kubectl set image deployment web nginx=that-image-does-not-exist
```
会看到部署过程停顿了，但是还有 80% 的应用容量是可用的。

如果我们运行 kubectl rollout undo deployment web，Kubernetes 就会回滚到使用 nginx 镜像的旧版本。

## 理解选择器和标签
前面我们说过，ReplicaSet 的任务是确保有 N 个符合规范的 Pod。这其实并不完全。实际上 ReplicaSet 并不关心 Pod 的规范，它关心的只是标签。

换句话说，不论 Pod 运行的是 nginx 还是 redis 还是什么别的什么东西；所有的关注点都是，它们要有正确的标签。前面的例子中，标签大概是 run=web 以及 pod-template-hash=xxxyyyzzz 的形式。

ReplicaSet 包含了一个 selector 成员，内容是一个逻辑表达式，功能和 SQL 中的 SELECT 类似，用来选择符合要求的 Pod。ReplicaSet 保证 Pod 的数量正确，如有必要，就会新建或者删除 Pod，但是不会修改已经存在的 Pod。

这样会有个设想：可能可以手工创建带有这些标签的 Pod ，但是却用的不同镜像（或者不同配置），就能骗过 ReplicaSet 了。

粗看上去，这可能是个很大的潜在问题。但实际上，我们很难恰巧选择了正确的标签，这是因为标签中包含了根据 Pod 规范运算得出的哈希值。

### Service 负载均衡
选择器还用在 Service 上，这个对象负责 Kubernetes 的内外部的负载均衡。我们可以给 web 创建一个 Service：
```
kubectl expose deployment web --port=80
```
这个服务会有它自己的内部 IP 地址（ClusterIP），连接到这个地址的 80 端口会被负载均衡到这个 Deployment 所有 Pod 之中。

事实上这个连接的负载均衡范围是所有符合 Service 标签选择器的 Pod 中，例如这里对应的是 run=web。

在我们编辑 Deployment 并触发滚动时，就会创建新的 ReplicaSet。这个 ReplicaSet 会创建 Pod，新 Pod 标签会包含 run=web，所以这些 Pod 就会自动的接到流量。

这表明在滚动更新时，Deployment 不需要因为 Pod 的的启动停止，而去重新配置或者通知负载均衡器。负载均衡器通过 selector 自动的完成任务。

如果你好奇就绪检测的内幕：Pod 只有在所有成员容器都通过就绪检测之后才会作为有效的 Endpoint 被加入服务。换句话说，Pod 只有准备就绪之后才会开始接收流量。

## Kubernetes 部署的高级策略
有些事后我们希望在推出新版本时候还有更多的控制。

两个知名流行技术是蓝绿部署以及金丝雀部署。

### Kubernetes 中的蓝绿部署
在蓝绿部署中，我们希望立即把所有流量从旧版本切换到新版本，而不是象之前说的渐进切换。提出这种要求可能有几个原因：

我们不想混合新旧请求，希望能够尽可能清晰的从旧版本切换到新版本；
我们正在更新多个组件（例如 Web 前端和 API 后端），不想新版本前端和旧版后端发生联系；
如果出现问题，我们希望有能力尽快回滚，无需等旧版本容器重启。
在 Kubernetes 中，可以用创建多个 Deployment 的方式来完成蓝绿部署，通过对 Service 的 Selector 字段的控制来进行切换。

下面的命令会创建两个 Deployment：blue 和 green，分别使用 nginx 和 httpd 镜像：
```
kubectl create deployment blue --image=nginx
kubectl create deployment green --image=httpd
```
接下来我们创建一个 Service，起初不会发送任何流量：
```
kubectl create service clusterip web --tcp=80
```
然后我们更新 web 服务的选择器：kubectl edit service web。这个命令会从 Kunernetes API 中抓取服务对象的定义，在文本编辑器中打开。在其中查找：
```
selector:
app: web
```
把其中的 web 替换成 blue 或者 green 或者别的什么。保存并退出。kubectl 会把更新的定义推送给 Kubernetes API，然后 web 服务现在就会向特定的 Deployment 发送流量了。

可以用 kubectl get svc web 命令获取服务的地址，并使用 curl 进行访问。

我们用文本编辑器作出的变更，也可以完全使用命令行来完成，例如 kubectl patch 命令：
```
kubectl patch service web -p '{"spec": {"selector": {"app": "green"}}}'
```
蓝绿部署的好处是，流量切换几乎是立刻完成的，推出和回滚都可以很方便的通过更新 Serevice 定义来完成。

### 用 Kubernetes 完成金丝雀部署
有时我们不想让测试版本影响所有用户，即使是短时间也不行。所以我们可以部分推出新版本。例如我们部署新旧两组实例，1% 的流量发送给新版本。

接下来我们在新旧版本的监控数据中进行观察。如果情况允许，就可以向前推进；如果延迟、错误率或者其它什么东西看起来有问题，就回滚到旧版本。

由于 Kubernetes 的标签和选择器的机制，可以很简单的实现这种策略。

前面的例子中，我们修改了服务的选择器，接下来我们修改一下 Pod 标签。

例如设置服务的选择器，让它选择带有 status=enabled 的 Pod，然后给特定的 Pod 打上标签：
```
kubectl label pod fronted-aabbccdd-xyz status=enabled
```
也可以一次打上多个标签：
```
kubectl label pods -l app=blue,version=v1.5 status=enabled
```
删除标签同样简单：
```
kubectl label pods -l app=blue,version=v1.4 status-
```
## 结论

我们看到了一些用于安全部署的技术，其中的一些能够很方便的降低因部署造成的停机时间，这让我们可以在不担心影响用户的情况下提高部署频度。

有些技术给我们系上安全带，阻止问题版本影响服务。还有些别的服务让我们感觉安心。有点像主机游戏中的保存按钮——在尝试困难操作之前，我们知道如果出了问题，我们还可以回到从前。

Kubernetes 让开发和运维团队能够使用这些技术来提高部署的安全性。如果部署的危险系数降低，那么就可以更频繁地、渐进地进行部署，并可以更方便的观察变更的后果。

这一切都会让我们的新特性和修复特性能够更快面世，让我们的应用有更好的可用性。这也是实现容器化和持续交付的重要基础。
