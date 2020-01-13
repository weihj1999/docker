# Kubernetes的机遇，挑战在2019年升级
原文：

https://www.sdxcentral.com/articles/news/kubernetes-opportunities-challenges-escalated-in-2019/2019/12/?utm_campaign=website&utm_source=sendgrid&utm_medium=email

作者：

Dan Meyer

![大容器](https://www.sdxcentral.com/cdn-cgi/image/w=736,h=368,fit=scale-down,f=auto,q=85/https://www.sdxcentral.com/wp-content/uploads/2019/12/Ericsson-Reveals-Container-Based-5G-%E2%80%98First%E2%80%99-With-Telstra.jpg)

如果说2018年是Kubernetes进入主流的一年，那么2019年就是现实的一年。现实是，尽管Kubernetes很棒，但也很难。

Kubernetes生态系统通过保持按季度发布平台更新的步伐，在推动市场方面发挥了通常的作用。 这种馈送(feeding)帮助Kubernetes继续推动了云市场的发展。 但是，持续的安全性和商业化挑战表明，没有挑战的增长不会到来。

## Kubernetes 2019：生态爆炸（Ecosystem Explosion）

Kubernetes继续吸引几乎所有与云空间相关的公司的兴趣。 最近在圣地亚哥举行的KubeCon + CloudNativeCon活动就证明了这一点，该活动吸引了12,000多名与会者。 与上次在北美举行的活动相比增加了50％

包含开源项目的Cloud Native Computing Foundation（CNCF）在其第一份Project Journey报告中发现，Kubernetes有315家公司为该项目做出了贡献，“在该项目的整个生命周期中已有数千家公司做出了承诺。” 较CNCF在2016年初采用该项目之前做出的贡献增加了15个。

自CNCF采纳以来，Kubernetes包括个人贡献者在内，共有约24,000位贡献者，148,000个代码提交，83,000个拉取请求和110万个贡献。 CNCF执行董事Dan Kohn在接受采访时解释说：“这是第二或第三高速度的开源项目，具体取决于您对Linux和React的计算方式。”

## 安全方面的惊喜（Security Surprises）

伴随着这种增长，人们越来越关注平台安全性。 对于希望将Kubernetes深入运营的企业来说，这仍然是最大的担忧之一。

过去一年发现了许多引人注目的安全漏洞，这些漏洞测试了对该平台的整体信心，从而阻碍了这种发展。

发现的最令人困扰的漏洞可能是Kubernetes kubectl命令行工具中的一个，该工具允许对Kubernetes集群运行命令来部署应用程序，检查和管理集群资源以及查看日志。 如果遭到破坏，该漏洞可能使攻击者可以使用受感染的容器在用户的工作站上替换或创建新文件。

此特定漏洞的最大挑战是，该漏洞是在今年早些时候发现的，即使在已发布修补程序修复该漏洞之后，该漏洞仍然存在。 与Kubernetes产品安全委员会合作的乔尔·史密斯（Joel Smith）在邮件中写道：“该问题的原始解决方案尚未完成，并且发现了一种新的利用方法。”

最近，发现了一个API漏洞，如果利用该漏洞，攻击者将可以发起被称为“billion laughs”攻击的拒绝服务（DoS）黑客。

CNCF积极采取行动以消除安全隐患。 今年，它发布了安全审核，发现了容器编排平台中的数十个安全漏洞。 其中包括5个高严重性问题和17个中等严重性问题。 已解决这些问题的修补程序。

Kubernetes的**整体规模和操作复杂性**被认为是造成这些安全漏洞的关键原因。

审计解释说：“评估团队发现，Kubernetes的配置和部署并不简单，某些组件具有令人困惑的默认设置，缺少操作控制和隐式定义的安全控制

它还发现，广泛的Kubernetes代码库缺少详细的文档来指导管理员和开发人员建立可靠的安全状态。

审计人员指出：“代码库既庞大又复杂，代码的大部分包含最少的文档和大量依赖关系，包括Kubernetes外部的系统。” “在代码库中有很多逻辑重新实现的案例，可以将其集中到支持库中，以降低复杂性，简化补丁程序并减轻代码库各个区域的文档负担。”

尽管存在这些顾虑，但审计确实发现Kubernetes确实简化了“与维护和操作群集工作负载有关的困难任务，例如部署，复制和存储管理。”基于角色的访问控制（RBAC）的使用还为用户提供了一种途径 增加安全性。

## 上市（Go-to-Market）

增强安全性组件是Kubernetes生态系统的一项重要任务，但不是唯一一个继续阻碍更广泛部署的问题。虽然似乎每个人都想采用Kubernetes，但对于许多人来说，这仍然是一个复杂的挑战。

对于某些已经能够利用这种复杂性来推动其业务发展的供应商来说，这个特殊的问题是件好事。 Kubernetes在2019年见证了通过并购或风险投资向知名品牌和初创公司投入的数十亿美元。

这种增长的亮点包括IBM斥资340亿美元收购了今年结束的Red Hat，以及VMware花费了数十亿美元来增强其Kubernetes资产。

虽然有些人设法用Kubernetes捞金（Strike Gold），但其他人却在其阴影下挣扎。

Docker公司开发了可引发Kubernetes革命的开源容器平台，但由于无法在日益拥挤的市场中脱颖而出，最近被迫出售其专注于Kubernetes的企业管理业务。

分析师指出，Docker Inc.简化容器采用的努力也是其失败的一部分。 “从某种意义上说，Docker几乎是其自身成功的受害者，” 451 Research的研究分析师Jay Lyman最近告诉SDxCentral。 “它使容器民主化，并使它们更易于使用。”

其他人也感到同样的压力。

Mesosphere是最早发布容器编排平台及其Marathon产品的供应商之一，该产品在DC / OS内运行，并将其更名为D2IQ。 这一举动是在将重点从帮助公司建立其云原生基础架构转变为在生产环境（IQ）中运行该基础架构的“Day Two”（D2）挑战。

**规模较小的初创公司Containership也屈从于此，宣布由于Kubernetes的崛起而无法通过运营获利**，该公司即将关闭商店。 这包括试图将其Containership Cloud运营转向更注重Kubernetes的平台的失败尝试。

## 走向边缘（Edging Toward the Edge）

Kubernetes可能使某些人难以竞争，但这并不意味着仍然没有更大的增长空间。在2019年获得发展势头的Kubernetes区域处于边缘。

Mobile Experts的最新报告预测，到2024年，边缘计算市场将增长10倍。它指出，边缘计算趋势已从集中式超大规模数据中心扩展到分布式边缘云节点，其中近边缘数据中心的资本支出占最大份额 市场。

许多供应商对Kubernetes的内核进行了重新包装，其方式允许平台在资源受限的环境中运行。 轻量级瘦身非常重要，因为与数据中心或网络核心位置相比，边缘位置受资源限制更大。

Rancher Labs，CDNetworks和Edgeworx等供应商都推出了基于可在这些环境（资源受限环境）中生存的Kubernetes变体构建的平台。

其他供应商一直在努力使用完整的Kubernetes平台。

Mirantis去年将Kubernetes插入其Cloud Platform Edge产品中，以允许操作员部署通过统一管理平面连接的容器，虚拟机（VM）和裸机（PoP）的组合。

同样，IoTium去年更新了其基于远程管理的Kubernetes的边缘云基础架构。 该平台将Kubernetes放置在可以位于节点内部的边缘位置。 该公司使用在IoTium的SD-WAN平台上运行的完整版本的Kubernetes。

还有一个KubeEdge开源项目，该项目支持与Kuberenetes界面一致的边缘节点，应用程序，设备和群集管理。这可以帮助边缘云完全像云集群一样运行。


## 5G （必谈话题）

完整的Kubernetes堆栈也正朝着5G部署发展。

Linux基金会的LF Networking小组在KubeCon + CloudNativeCon北美活动上演示了Kubernetes支持的端到端5G云本地网络的现场演示，该演示展示了未来开源电信部署的前景。

Linux基金会社区和生态系统开发副总裁Heather Kirksey说，由于围绕网络问题和Kubernetes的工作量越来越大，因此该演示非常重要。 容器编排平台的任务是管理支持5G网络承诺所需的基于容器的基础架构。

“我们正在拥抱云原生和新应用程序，我们想让这里的人们知道我们为什么要与云原生开发者社区合作，”柯克西说。 “让这个社区对电信感到兴奋，并与我们合作促进网络发展，这是一个挑战。”

VMware产品与开发副总裁Craig McLuckie在接受SDxCentral采访时表示，Kubernetes专注于5G电信。 McLuckie曾在Google工作，曾在其Compute Engine和最终成为Kubernetes项目的平台上工作，他说5G对于Kubernetes社区和社区的代码库如何解决这一问题将是一个奇妙而有趣的挑战。 ”

过去的一年确实的确表明，尽管Kubernetes有了一定的地位，但它仍然是发展和机遇的强大中心。 现在最大的挑战将是生态如何应对2020年的成功和机遇。
