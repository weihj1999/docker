# MariaDB X4将智能交易引入开源数据库
摘录于：[TechTarget](https://searchdatamanagement.techtarget.com/news/252476908/MariaDB-X4-brings-smart-transactions-to-open-source-database?track=NL-1816&ad=931991&src=931991&asrc=EM_NLN_122619158&utm_medium=EM&utm_source=NLN&utm_campaign=20200123_MariaDB%20X4%20converges%20database%20use%20cases)


开源数据库供应商MariaDB使用新功能更新了其旗舰平台，以实现事务和分析数据库的融合。

MariaDB从它的MySQL数据库根源开始已经走了很长一段路。开源数据库供应商发布了其新的MariaDB X4平台，为用户提供了“智能交易”技术，以支持分析和交易数据库。

MariaDB成立于2009年，由MySQL的原始创建者Monty Widenius创立，作为MYSQL的替代品，因为Widenius对Oracle正在采用开源数据库的方向持有怀疑态度。

Oracle在2008年通过收购Sun Microsystems收购了MySQL。现在，到2020年，MariaDB仍然使用核心MySQL数据库协议，但是MariaDB数据库在X4平台更新中体现的其他方式上已出现重大分歧。

数据高级分析师James Curtis表示，MariaDB X4版本于1月14日发布，使该技术成为云原生讨论的焦点。 451 Research的AI和分析

“他们实施了许多更改，包括新的和改进的存储引擎，但是引人注目的是对体系结构进行了调整，使行和列的存储在更深的层次上融合在一起，这一变化很可能吸引许多客户 ”，柯蒂斯说。

![MariaDB X4](https://cdn.ttgtmedia.com/rms/onlineImages/mariaDB_maxscale_desktop.jpg)

## MariaDB X4智能事务融合了数据库功能

MariaDB产品营销高级总监Shane Johnson说，在过去三年中，与MySQL的分歧加剧了。 他指出，在最新版本中，MariaDB增加了Oracle数据库兼容性，而MySQL不包括该兼容性。 

此外，MariaDB的旗舰平台还提供了数据库防火墙和动态数据屏蔽功能，这两个功能均旨在提高安全性和数据隐私性。 但是，MariaDB与MySQL之间最大的区别在于MariaDB支持可插拔存储引擎的方式，这些引擎在X4更新中获得了新功能。

Johnson解释说，以前使用可插拔存储引擎时，用户将使用InnoDB存储引擎部署MariaDB实例用于事务性用例，而使用ColumnStore列存储引擎部署另一个实例以进行分析。 

在早期版本中，“更改数据捕获”过程使这两个数据库同步。 在MariaDB X4更新中，事务和分析功能已通过MariaDB称为智能事务的方法进行了融合。 

约翰逊说：“因此，当您安装MariaDB时，您将获得所有现有的存储引擎以及ColumnStore，从而使您可以混合和匹配以使用行和列数据进行交易和分析，”。

## MariaDB X4调整云存储

MariaDB X4中的另一个新功能是能够更有效地使用云存储后端的功能。 

约翰逊说：“每种存储介质都针对不同的工作负载进行了优化。” 

例如，约翰逊（Johnson）指出，亚马逊网络服务（Amazon Web Service）的S3因其高可用性和高容量而非常适合分析。 他补充说，对于具有基于行的存储的事务性应用程序，Amazon Elastic Block Store（EBS）更合适。
 
在MariaDB X4平台中混合并匹配EBS和S3的能力使用户可以更轻松地在数据库中整合分析和事务性工作负载。 

约翰逊说：“ X4的更新并不意味着您可以一直在云中运行MariaDB，因为您一直可以做到这一点，而可以通过智能事务运行它并针对云存储服务进行了优化。” 

## MariaDB数据库即服务（DBaaS）即将到来

MariaDB说，它计划今年进一步扩大其产品组合。 

MariaDB表示，核心的MariaDB开源社区项目当前的版本为10.4，其计划包括智能交易功能的10.5版将在未来几周内首次亮相。

新的智能交易功能已经在MariaDB Enterprise 10.4更新中引入。 MariaDB Enterprise Server具有更多的配置设置和针对企业用例的强化。

完整的MariaDB X4平台在MariaDB MaxScale数据库代理的基础上又走了一步，该代理提供了自动故障转移，事务重播和数据库防火墙以及开发人员构建数据库应用程序所需的实用程序。 

Johnson指出，传统上，新功能通常会首先出现在社区版本中，但是碰巧的是，在此期间，MariaDB开发人员能够更快地将这些功能添加到企业版本中。 

MariaDB计划今年推出新的DBaaS产品。用户已经可以自己将MariaDB部署到选择的云中。 MariaDB还具有托管服务，可为MariaDB环境提供全面管理。

约翰逊说：“有了托管服务，我们为客户提供了一切服务，我们在他们选择的云上部署了MariaDB，我们将对其进行管理，管理，操作和升级。” “今年我们将拥有自己的数据库即服务，这将提供更好的选择。”
