---
title: DevOps是什么
source: https://zhuanlan.zhihu.com/p/562036793
author:
published:
created: 2026-04-28
description: 导读：DevOps中的Dev指的是Development（开发），Ops指的是Operations（运维），用一句话来说，DevOps就是打通开发运维的壁垒，实现开发运维一体化。 一、DevOps是什么？从哪里来？ DevOps的概念DevOps一词的来自…
tags:
  - clippings
---

> 导读： [DevOps] 中的Dev指的是Development（开发），Ops指的是Operations（运维），用一句话来说，DevOps就是打通开发运维的壁垒，实现开发运维一体化。

**一、DevOps是什么？从哪里来？**

**DevOps的概念**

DevOps一词的来自于Development和Operations的组合，突出重视软件开发人员和运维人员的沟通合作，通过自动化流程来使得软件构建、测试、发布更加快捷、频繁和可靠。

DevOps概念早先升温于2009年的欧洲，因传统模式的运维之痛而生。

![](https://pic3.zhimg.com/v2-295076e8a18db2e78c508dbb64c0c6ce_1440w.jpg)

**DevOps是为了填补开发端和运维端之间的信息鸿沟，改善团队之间的协作关系。** 不过需要澄清的一点是，从开发到运维，中间还有测试环节。DevOps其实包含了三个部分：开发、测试和运维。

![](https://pic3.zhimg.com/v2-1ad76fabae75c488b809fb171fbe85e6_1440w.jpg)

换句话说，DevOps希望做到的是软件产品交付过程中IT工具链的打通，使得各个团队减少时间损耗，更加高效地协同工作。专家们总结出了下面这个DevOps能力图，良好的闭环可以大大增加整体的产出。

![](https://pic4.zhimg.com/v2-ad6f2e6fd6ad6cd5c31cff2b295cbbd1_1440w.jpg)

**历史变革**

由上所述，相信大家对DevOps有了一定的了解。但是除了触及工具链之外，作为文化和技术的方法论，DevOps还需要公司在组织文化上的变革。回顾软件行业的研发模式，可以发现大致有三个阶段：瀑布式开发、敏捷开发、DevOps。

DevOps很早就有人提出来，但是，为什么这两年才开始受到越来越多的企业重视和实践呢？因为DevOps的发展是独木不成林的，现在有越来越多的技术支撑。 [微服务架构](https://zhida.zhihu.com/search?content_id=213029637&content_type=Article&match_order=1&q=%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9E%B6%E6%9E%84&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3Nzc1MzAyMzAsInEiOiLlvq7mnI3liqHmnrbmnoQiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyMTMwMjk2MzcsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.sD-OAvy-5ENsOotYnHY3WNPah_-vKAXgXSGz7Xpyenw&zhida_source=entity) 理念、 [容器技术](https://zhida.zhihu.com/search?content_id=213029637&content_type=Article&match_order=1&q=%E5%AE%B9%E5%99%A8%E6%8A%80%E6%9C%AF&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3Nzc1MzAyMzAsInEiOiLlrrnlmajmioDmnK8iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyMTMwMjk2MzcsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.oe7gkW4a-YA9w0vXn8-UepPxFUJGymW6cm7cFTleTxg&zhida_source=entity) 使得DevOps的实施变得更加容易，计算能力提升和云环境的发展使得快速开发的产品可以立刻获得更广泛的使用。

![](https://pic4.zhimg.com/v2-600a09447dff16807d92ec9f227eade3_1440w.jpg)

（注：上图摘自红帽副总裁Ashesh Badani的一次新闻分享会）

**二、DevOps的几个关键问题**

**好处是什么？**

**DevOps的一个巨大好处就是可以高效交付，这也正好是它的初衷。** [Puppet](https://zhida.zhihu.com/search?content_id=213029637&content_type=Article&match_order=1&q=Puppet&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3Nzc1MzAyMzAsInEiOiJQdXBwZXQiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyMTMwMjk2MzcsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.Rgnj9tmGhHbfTuqk74EEPiq58MFZymcjl3V7lDoZG94&zhida_source=entity) 和DevOps Research and Assessment (DORA) 主办了DevOps调查报告，根据全球4600位各IT公司的技术工作者的提交数据统计，得出高效公司平均每年可以完成1460次部署。

与低效组织相比，高效组织的部署频繁200倍，产品投入使用速度快2555倍，服务恢复速度快24倍。在工作内容的时间分配上，低效者要多花22%的时间用在为规划好或者重复工作上，而高效者却可以多花29%的时间用在新的工作上。所以这里的高效不仅仅指公司产出的效率提高，还指员工的工作质量得到提升。

**DevOps另外一个好处就是会改善公司组织文化、提高员工的参与感。** 员工们变得更高效，也更有满足和成就感；调查显示高效员工的雇员净推荐值（eNPS:employee Net Promoter Score）更高，即对公司更加认同。

![](https://pic3.zhimg.com/v2-94e1b1ea90618d92717631920f885b32_1440w.jpg)

**快速部署同时提高IT稳定性。这难道不矛盾吗？**

快速的部署其实可以帮助更快地发现问题，产品被更快地交付到用户手中，团队可以更快地得到用户的反馈，从而进行更快地响应。而且，DevOps小步快跑的形式带来的变化是比较小的，出现问题的偏差每次都不会太大，修复起来也会相对容易一些。

![](https://pic2.zhimg.com/v2-d4f87e361e4435001f3d90303fcffc7f_1440w.jpg)

因此，认为速度就意味着危险是一种偏见。此外，滞后软件服务的发布也并不一定会完全地避免问题，在竞争日益激烈的IT行业，这反而可能错失了软件的发布时机。

**三、DevOps会兴起？**

**为什么会继续火下去？**

**条件成熟：技术配套发展**

技术的发展使得DevOps有了更多的配合。早期时，大家虽然意识到了这个问题的，但是苦于当时没有完善丰富的技术工具，是一种“理想很丰满，但是现实很骨感”的情况。DevOps的实现可以基于新兴的容器技术；也可以在自动化运维工具Puppet、 [SaltStack](https://zhida.zhihu.com/search?content_id=213029637&content_type=Article&match_order=1&q=SaltStack&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3Nzc1MzAyMzAsInEiOiJTYWx0U3RhY2siLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyMTMwMjk2MzcsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.kcofBL896Ve1nagBgg7EEM46YzJlgf3UOFnkaZ8cY5M&zhida_source=entity) 、 [Ansible](https://zhida.zhihu.com/search?content_id=213029637&content_type=Article&match_order=1&q=Ansible&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3Nzc1MzAyMzAsInEiOiJBbnNpYmxlIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MjEzMDI5NjM3LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.5IPO4MQ3bpjD70n4xidygRA8YDiqq3F4hM2YOatYhn4&zhida_source=entity) 之后的延伸；还可以构建在传统的 [Cloud Foundry](https://zhida.zhihu.com/search?content_id=213029637&content_type=Article&match_order=1&q=Cloud+Foundry&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3Nzc1MzAyMzAsInEiOiJDbG91ZCBGb3VuZHJ5IiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MjEzMDI5NjM3LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.HhmcM5ZqjXNHF6pbJHWvMTodesQeD5eB4syN_JLJpwY&zhida_source=entity) 、 [OpenShift](https://zhida.zhihu.com/search?content_id=213029637&content_type=Article&match_order=1&q=OpenShift&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3Nzc1MzAyMzAsInEiOiJPcGVuU2hpZnQiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyMTMwMjk2MzcsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.n4ftzRB9Ql3fTLPUpEKaewIbmis3tOx0VgSEGB7aODE&zhida_source=entity) 等PaaS厂商之上。

**来自市场的外部需求：这世界变化太快**

IT行业已经越来越与市场的经济发展紧密挂钩，专家们认为IT将会有支持中心变成利润驱动中心。事实上，这个变化已经开始了，这不仅体现在Google、苹果这些大企业中，而且也发生在传统行业中，比如出租车业务中的Uber、酒店连锁行业中的Airbnb、图书经销商Amazon等等。能否让公司的IT配套方案及时跟上市场需求的步伐，在今天显得至关重要。

DevOps 度报告给出了一个运维成本的计算公式：  
停机费用成本 = 部署频率 \* 版本迭代失败概率 \* 平均修复时间 \* 断电的金钱损失

**来自团队的内在动力：工程师也需要**

对于工程师而言，他们也是DevOps的受益者。微软资深工程师Scott Hanselman说过“对于开发者而言，最有力的工具就是自动化工具”（The most powerful tool we have as developers is automation）。

工具链的打通使得开发者们在交付软件时可以完成生产环境的构建、测试和运行；正如Amazon的VP兼CTO Werner Vogels那句让人印象深刻的话：“谁开发谁运行”。（You build it, you run it）

**四、实现DevOps需要什么？**

**硬性要求：工具上的准备**

上文提到了工具链的打通，那么工具自然就需要做好准备。现将工具类型及对应的不完全列举整理如下：

- 代码管理（SCM）：GitHub、GitLab、BitBucket、SubVersion
- 构建工具：Ant、Gradle、maven
- 自动部署：Capistrano、CodeDeploy
- 持续集成（CI）：Bamboo、Hudson、 Jenkins
- 配置管理：Ansible、Chef 、Puppet、SaltStack、ScriptRock GuardRail
- 容器： Docker 、LXC、第三方厂商如AWS
- 编排： Kubernetes、Core、Apache Mesos、DC/OS
- 服务注册与发现：Zookeeper、etcd、Consul
- 脚本语言：python、ruby、shell
- 日志管理：ELK、Logentries
- 系统监控：Datadog、Graphite、Icinga、Nagios
- 性能监控：AppDynamics、New Relic、Splunk
- 压力测试：JMeter、Blaze Meter、 [loader.io](https://link.zhihu.com/?target=http%3A//loader.io)
- 预警：PagerDuty、pingdom、厂商自带如AWS SNS
- HTTP加速器：Varnish
- 消息总线：ActiveMQ、SQS
- 应用服务器：Tomcat、JBoss
- Web服务器：Apache、Nginx、IIS
- 数据库：MySQL、Oracle、PostgreSQL等关系型数据库；cassandra、mongoDB、redis等NoSQL数据库
- 项目管理（PM）：Jira、Asana、Taiga、Trello、Basecamp、Pivotal Tracker

在工具的选择上，需要结合公司业务需求和技术团队情况而定。

1 版本控制系统

Git

Git 是一个开源的分布式版本控制系统。如果您需要更多信息，请参阅 [git-scm.com/about](https://link.zhihu.com/?target=https%3A//git-scm.com/about) 。

GitHub

GitHub 是一个 Git 存储库托管服务。

Bitbucket

Bitbucket 是 Atlassian 提供的基于 Git 的源代码存储库托管服务。

Apache Subversion（SVN）

Apache Subversion 是一个软件版本号和版本控制系统，根据 Apache 许可以开源形式分发。

Azure DevOps 服务器

Azure DevOps Server 是微软公司提供的版本控制产品。

还有更多的版本控制技术，例如 Gitlab、Mercurial、Bazaar、AWS CodeCommit、Perforce Helix Core 和 SourceForge。

2 容器化

Docker

Docker 是执行系统级虚拟化的容器化软件。该软件的开发者是 Docker，Inc。该软件首次发布于 2013 年。它使用 Go 编程语言编写。

**容器：** 容器是软件的标准单元，它将代码及其所有依赖项打包，使得软件可以从一个环境快速可靠地运行到另一个环境。

**容器映像：** Docker 容器映像是一个轻巧、独立的可执行软件包，其中包含运行应用程序所需的一切：代码、运行时、系统工具、系统库和配置。容器映像在运行时成为容器，对于 Docker 容器，映像在容器 Docker 引擎上运行时成为容器。它适用于 Linux 和基于 Windows 的应用程序。不论基础架构如何，容器化软件的运行方式始终相同。容器将软件与环境隔离开来，尽管在开发和模拟阶段之间存在差异，但是容器能确保软件均可一致地工作。

**Docker 引擎：** Docker 容器技术于 2013 年作为开源 Docker 引擎的方式发起。Docker 的技术之所以独特是因为它专注于开发人员和系统操作人员的需求，将应用程序依赖项与基础架构分离开来。

Docker 技术可从 Docker 及其开放源代码获得。所有主要数据中心供应商和云服务提供商都将其集成到云技术中。其中，许多提供商利用 Docker 来提供基于容器的 IaaS 产品。此外，领先的开源无服务器（serverless）框架也利用 Docker 容器技术。

亚马逊 ECS

亚马逊公司的弹性容器服务（Elastic Container Service，简称 ECS）是一项编排服务，它支持运行 Docker 容器以在亚马逊 AWS 上执行容器化的应用程序。它消除了对单独的容器编排软件的安装和管理需求。尽管 ESC 是免费的，但亚马逊会对其使用的资源收费。

AWS Fargate

AWS Fargate 使您无需管理服务器或集群即可执行容器。亚马逊公司根据运行 Fargate 所使用的内存和虚拟 CPU 资源收取费用。

谷歌云平台

谷歌云为您提供了运行容器的不同选项。它们是谷歌 Kubernetes 引擎（用于容器群集管理）、Google 计算引擎（用于虚拟机和 CI/CD 管道）和 Google 应用引擎灵活环境（用于完全托管的 PaaS 上的容器）。谷歌提供了可用于生产的执行容器化应用程序所需的基础架构。谷歌根据所使用的资源收费。

LXC

LXC 是 Linux 容器的缩写，Linux 容器是一种 OS 级虚拟化方法，用于执行使用单个 Linux 内核的控制主机上的多个独立 Linux 系统（容器）。这是 GNU LGPL 许可下的开源免费工具。它可以在 GitHub 库中找到。

微软 Azure

类似于亚马逊 ECS，对容器管理不收取任何费用，但对您使用的内容收取费用。

Apache Mesos

Apache Mesos 由 Apache 软件基金会开发，是一个处理计算机集群的开源项目。它于 2016 年首次发布，并使用 `C++` 编程语言编写，免费使用。

Docker-compose

Docker Compose 用于运行多容器应用程序。Docker-compose 描述互连服务组，这些服务组共享软件依赖关系并被编排和缩放在一起。您可以使用一个 YAML 来配置您应用程序的服务。然后，使用 `docker-compose up` 命令，从您的配置中创建并启动所有服务。

3 容器编排

随着大公司对其应用程序进行容器化，并将它们迁移到云中，对容器编排解决方案的需求不断增长。有许多可用的解决方案，尽管并不详尽，下面的列表提供了一些可用的容器编排工具和服务。

Kubernetes

Kubernetes 是一个开源平台，它支持容器化服务的部署、扩展和管理的自动化。Kubernetes 最初由谷歌开发，由云原生计算基金会（Cloud Native Computing Foundation）维护。有许多付费或免费的产品，但其中最受欢迎的是 Kubernetes。

部署 Kubernetes 为我们提供了一个集群。Kubernetes 集群由一组工作机（称为节点）组成，这些节点上运行容器化的应用。每个群集至少有一个工作节点。每个节点由控制平面（control plane）管理，并包含运行 Pod 所需的服务。每个 Pod 是容器的逻辑主机。工作节点托管作为应用程序工作负载组件的 Pod。控制节点管理群集中的工作节点和 Pod。控制平面和节点可以是物理机或虚拟机。控制平面和节点运行在多台计算机上并不罕见。

Docker swarm

使用 Docker Swarm 模式，我们可以定义服务的期望状态，而 Docker 将维护该状态。Docker 企业版已将 Kubernetes 与 Swarm 集成在一起。Docker 现在为编排引擎的选择提供了灵活性。Docker 引擎 CLI 用于创建大量的 Docker 引擎，可在其中部署应用程序服务。

Minikube

Minikube 允许您在本地计算机上使用 Kubernetes，并允许您在计算机上使用单节点 Kubernetes 集群。

Rancher

Rancher 是使用容器编排的开源平台，被称为 cattle。它使您能够利用 Kubernetes、Swarm、Mesos 等编排服务。Rancher 提供了管理容器所需的软件，组织无需使用一套不同的开源技术从头开始构建容器服务平台。Rancher 允许管理在客户指定的提供商上运行的 Kubernetes 集群。

AWS EKS

亚马逊的完全托管的 Kubernetes 服务。

AKS（微软 Azure 的全管理 Kubernetes 服务）

Azure 容器实例（Azure Container Instance，简称 ACI）是微软 Azure 提供的基本容器编排服务。Azure 服务结构（ Azure Service Fabric）是微软 Azure 提供的开源容器编排器。

Marathon

Marathon 是一个可在 Apache Mesos 上运行大规模容器的框架。

Nomad

Nomad 是 HashiCorp 提供的容器和工作负载协调器。

4 IT 自动化

Jenkins

Jenkins 是开源、免费、与平台无关的自动化服务器。Jenkins 提供了几个插件来支持构建、部署和自动化任何项目。它是一个独立的基于 Java 的程序。它支持 Windows、Mac 和其他类似 UNIX 的操作系统。Jenkins 可以通过本机系统软件包 Docker 安装，甚至可以由安装了 Java 运行环境（Java Runtime Environment，简称 JRE）的任何计算机独立运行。

Chef

Chef INFRA 是用于自动化基础架构配置的平台。这将确保每个系统被正确和一致地配置。在此平台上，基础架构被定义为代码。Chef INFRA 将确保配置策略灵活、可版本控制、可测试并且易于阅读。Chef 是基于合同的付费平台。

Puppet

它可以连接云提供商、DevOps 工具和其他 API。根据来自现有 DevOps 工具的信号，传递触发工作流以协调对下游服务的操作。它使您可以通过基于 YAML 的配置自定义和扩展工作流程。它具有一个集成库，可以更轻松地添加步骤。借助 [Puppet Enterprise](https://zhida.zhihu.com/search?content_id=213029637&content_type=Article&match_order=1&q=Puppet+Enterprise&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3Nzc1MzAyMzAsInEiOiJQdXBwZXQgRW50ZXJwcmlzZSIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjIxMzAyOTYzNywiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.7FkTxQC1Mpg0qLsg75tnA62WB8V8MsTDZEW-nBI5ka0&zhida_source=entity) ，您可以交付和管理任何云、基础架构或服务。Puppet 是基于合同的付费平台。

Ansible

Ansible 是 RedHat 的开源 IT 自动化工具。Ansible 可以配置系统、部署软件以及协调更高级的 IT 任务，例如持续部署。Ansible 使用剧本（playbooks）做配置管理和多机部署系统。

Terraform：\*\*Terraform 是 Hashi Corp. 提供的开源基础架构即代码软件工具。Terraform 是用于构建、更改和管理基础架构的工具。Terraform 将云 API 编码为声明式配置文件。您可以使用 Terraform 通过称为 HashiCorp 配置语言（HashiCorp Configuration Language，简称 HCL）的配置语言来管理环境，以实现人类可读的自动化部署。

5 协作

Jira

Jira 是一种工具，旨在帮助团队进行项目管理，错误跟踪和问题跟踪。简单来说，它是一个问题跟踪器。Jira 被大公司广泛用于软件开发和软件测试。它是由澳大利亚 Atlassian 公司创建的基于 Web 的商业授权产品，于 2002 年首次发布，广泛流行且具有可有限使用的免费版本。很多敏捷团队使用 Jira 软件，其设计适用于 Scrum 和看板等方法。Jira 还被金融服务、零售、软件、汽车、非营利组织和政府广泛使用。

Slack

Slack 是 Slack Technologies 开发的专有业务通信平台。Slack 提供了许多 IRC 风格的功能，包括按主题组织的持久聊天室、私人组、直接消息将文件保存在一个地方等。Slack 声称，与电子邮件相比，它更加快捷、井井有条，并且更加安全。

Monday

[Monday.com](https://link.zhihu.com/?target=http%3A//Monday.com) 是公司用于任务管理、项目协作、过程跟踪和沟通的项目管理工具。

谷歌工作区（Workspace）

谷歌工作区是谷歌基于应用的生产力平台。谷歌将消息、日历、文件共享、聊天、视频会议和团队组织工具放在一起，并免费提供给其所有用户。

6 监控和可视化

Datadog

Datadog 主要是针对云应用程序的监视工具。它为您的所有云应用程序、服务器和网络提供详细的指标。它可以轻松地与其他自动化工具集成，例如 Ansible、Chef、Puppet 等。Datadog 是按主机收费的服务。

Prometheus（普鲁米修斯）

Prometheus 是用于事件监控和报警的免费软件应用程序。Prometheus 拉取指标（键 / 值），并将数据存储为时间序列，从而允许用户以实时方式查询数据和告警。它以给定的时间间隔与目标进行通信，以收集指标、汇总数据、显示数据、甚至在达到某些阈值时发出警报。

Grafana

Grafana 是开源的可视化和分析软件。它使您可以查询、可视化、告警和探索  
指标，无论这些指标存储在哪里。简单的说，它为您提供了将时间序列数据库（time-series database，TSDB）数据转换为精美的图形和可视化效果的工具。

Grafana 通过提供相关数据来帮助我们跟踪用户行为、应用程序行为、在生产环境或预生产环境中弹出错误的频率、弹出错误的类型以及上下文场景。Grafana Cloud 是由 Grafana 实验室运营的高可用、快速、完全托管的 OpenSaaS 指标平台。Grafana 企业版是 Grafana 的商业版本，其中包括开源版本中未提供的附加功能。

Loki

Loki 是受 Prometheus 启发的水平可扩展、高可用的多租户（multi-tenant）日志聚合系统。它的设计具有很高的成本效益，并且易于操作。与其他日志系统不同，它不索引日志的内容，而是为每个日志流设置一组标签。

大多数 DevOps 工具免费提供，并且它们中的大多数能够无缝地协同工作，这使我们能管理对自动化、持续集成、持续交付和持续改进的不断增长的需求。

**软性需求：文化和人**

DevOps成功与否，公司组织是否利于协作是关键。开发人员和运维人员可以良好沟通互相学习，从而拥有高生产力。并且协作也存在于业务人员与开发人员之间。业务人员非常清楚他们希望在最小化可行产品中实现什么，工程师们就按需交付，不做多余工作。

这样，工程师们使用通用的平台（即打通的工具链）得到更好的一致性和更高的质量。此外，DevOps对工程师个人的要求也提高了，很多专家也认为招募到优秀的人才也是一个挑战。

**五、DevOps的采用现状**

**哪些公司在用？**

DevOps正在增长，尤其是在大企业中：调查发现，DevOps的接受度有了显著提高。74%的受访者已经接受了DevOps，而去年这一比例为66%。目前，在81%的大企业开始接受DevOps，中小企业的接受度仅为70%。

那么具体而言都有些公司在采用DevOps呢？Adobe、Amazon、Apple、Airbnb、Ebay、Etsy、Facebook、LinkedIn、Netflix、NASA、Starbucks、Target（泛欧实时全额自动清算系统）、Walmart、Sony等等。

**他们怎么实施的？**

首先，大企业正在自下而上接受DevOps，其中业务单位或部门（31%）以及项目和团队（29%）已经实施DevOps。不过，只有21%的大企业在整个公司范围内采用了DevOps。

其次，在工具层面上，DevOps工具的用量大幅激增。Chef和Puppet依然是最常用的DevOps工具，使用率均为32%。Docker是年增长率最快的工具，用量增长一倍以上。Ansible的用量也有显著增加，使用率从10%翻倍至20%。

![](https://pica.zhimg.com/v2-9770a3b7cce7f203ea9a08e53204e2aa_1440w.jpg)

并且调查还发现不到半数（43%）的公司在使用诸如Chef、Puppet、Ansible或Salt等配置工具；然而使用配置工具的公司更有可能同时使用多个工具。25%的受访者使用两种或更多配置工具，只使用一种工具的比例为18%。其中Chef和Puppet是最常用的组合：使用Chef的组织中有67%同时也使用Puppet，类似的，使用Puppet的组织中也有67%同时使用了Chef。

![](https://pic3.zhimg.com/v2-0e914e7a9fd365785d3c8d57006ba6fe_1440w.jpg)

发布于 2022-09-07 11:19