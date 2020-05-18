
# 关于NextCloud的一些有趣发现

## 联邦政府云项目

2018年，德国联邦政府认为来自美国的软件公司化为了很多联邦政府的钱：
- 2017年，联邦政府支付了7400万欧元的软件许可费给微软 （联邦政府月30万人，人均月250欧元）
- 2015-2019年，联邦政府总共支出2.5亿欧元，用户使用专有（非自控）软件，

联邦政府发布了联邦云，寻求替代解决方案，构建内部文件分享和协作平台，并且采用Nextcloud的开源产品； nextcloud与Dropbox的服务能力相当，用户可以通过这些软件轻松分享文件，照片等等

联邦云有联邦政府的IT服务商ITZBund运营。

NextCloud位于斯图加特，与2016年从开源项目ownCloud发展；而来
- 可以随时查看代码，并检查安全性差距（强调透明，公开）
![NextCloud](https://cdn.prod.www.spiegel.de/images/6212d26d-0001-0004-0000-000001276776_w718_r2.0795180722891566_fpx44.62_fpy54.94.jpg)
- 可以安全在在数据中心运行，并接受定制化（根据需求修改代码）

NextCloud是德国针对美国提供商（Mircosoft）提供的典型反模型（我理解反模型就是替代品的意思）产品；微软出售软件使用许可，但客户几乎不了解其代码，并且只在自己的数据中心或者特殊合同的合作伙伴数据中心运营期服务。

针对联邦政府建设联邦政府私有云的标书，微软，Google，Amazon没有参与

# NextCloud市场宣传战略
## SaaS是一个冒险的解决方案
- 大多数消费者级解决方案（例如Dropbox或Office 365）在设计时都没有考虑到隐私法规和安全问题，无法将来自消费者和企业的数据混合在一起，分布在全球的数据中心中。 云服务提供商可能会根据美国《云计算法（FBI Cloud Act）》对企业IT工作负载进行处理，这意味着您的业务数据可能会根据美国司法系统的命令而泄漏，而通常不会向您透露。
- Nextcloud提供安全至上的解决方案，通过私有云解决方案以及本地和受信任的提供商提供的托管公共云解决方案完全控制数据的位置和访问策略。
## 客户需要100％确定性
- 通过电子邮件发送数据或使用公共SaaS文件共享解决方案发送数据并不能为敏感数据提供太多安全性。 加密非常复杂且使用繁琐，从而降低了员工工作或犯错所带来的实际收益。
- 将数据保存在您自己的基础架构上，或保存在受信任的本地私有或公共云提供商处，意味着您可以始终保持控制。 只有这样，您才能向客户确切显示其敏感文件的位置，监管机构将不合规的情况控制到最小。
## 兼容Mircrosoft Office
- 与OnlyOffice集成，确保操作习惯，用户体验与Microsoft Office保持一致

# NextCloud产品分析

## NextCloud Hub

- Enterprise oriented products
- Highlights:
  - Hosted on-premises
  - 100% open source
  - Very easy to use
  - Integration in infrastructure
  - Security and encryption

### NextCloud Files

- Enterprise File Sync and Share
- Available on desktop, mobile and web
- Highlight： power search, comment and lock 

### NextCloud Talk

- Call, chat and web meetings
- Available on mobile and browser
- On-premises, private audio/video conference and text chat, screen sharing and SIP integration

### NextCloud Groupware

- Calendar, Contacts & Mail
- Online collaboration
- Highlight： fast and easy to use

### Key capabilities

- Nextcloud Flow enables users to automate repetitive tasks and optimize business processes.
- Access existing storage silos like FTP, Windows Network Drives, SharePoint, Object Storage and Samba shares seamlessly through Nextcloud.
- Seamlessly edit office documents together with others, take notes while in a video call.
- Manage users locally or authenticate through LDAP / Active Directory, Kerberos and Shibboleth / SAML 2.0 and more
- Secure data with powerful file access control, multi-layer encryption, machine-learning based authentication protection and advanced ransomware recovery capabilities
- Extend Nextcloud further with a wide variety of apps on our app store or build your own

## NextCloud At Home

A Server At Home, Self-hosted productivity platform

From Community, Open for People

- 个人数据无处不在：但几乎所有数据都存储在六家公司中，并且每个国家/地区的名称都不同（Facebook，微信，Vk，Google，微博，腾讯，微软）。 他们了解用户的住所，寻找的内容，与谁交谈，购买的商品，美食爱好以及甚至用户的日常想法及计划。
- 数据代表客户的身份，并且很容易被滥用：诸如政治家发现用户的欺诈行为；男友被当地女警骚扰；用户的资产被他人觊觎；犯罪集团诱惑吸毒成瘾者等；
- 隐私是一种权利，是民主的基础：运行自己的私有Nextcloud服务器是最佳解决之道！

# NextCloud的重要合作伙伴

口号： Hosting solutions. Keep all your files in one safe, private place.

- 不仅仅是Host的云盘：按照数据量纲进行销售，但是包装有数据管理的应用，软件及办公服务，主打个人办公，家庭场景，企业办公场景。提供在线协作软件，域名服务，SSL及网络支持，及其他开源软件应用。
- 所有销售包，不限流量：
- 网络服务主要合作商：VERISIGN（威瑞信），美国一家专注于多种网络基础服务的上市公司，位于弗吉尼亚州赖斯顿。该公司将他们的业务统称为“智能基础设施服务”

## About Cloudamo
- Offers a new open source solution for cloud storage, which is situated in the EU, US, Canada, Singapore.
- Focuses on the open source product ownCloud and Nextcloud, which offers a broad variety of functions and programs
- Serves notable companies from all over the world as well as a mass-market of individuals.


