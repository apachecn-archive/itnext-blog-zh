# 7 Kubernetes 最佳实践

> 原文：<https://itnext.io/7-kubernetes-best-practices-17f8634907b0?source=collection_archive---------4----------------------->

*无论你是刚刚开始使用 Kubernetes，还是已经熟悉了 K8。以下是使用 K8 时的七个最佳实践。*

![](img/dc24d1e6eb63082d183e7a6a9ec24ccf.png)

照片由[阿龙·加藤](https://unsplash.com/@aaronkato?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) 拍摄

随着越来越多的应用程序构建在云上，Kubernetes 迅速成为最受欢迎的开源技术之一。容器化现在是云原生开发的标准实践，作为一个集群编排工具，Kubernetes (K8s)简化了容器化基础设施的定义和操作。

K8s 极其强大，一旦设置好，会为你的团队解锁很多福利。K8s 可以让您的基础设施更容易开发和扩展。它可以提高您的产品承受故障和从故障中恢复的能力。在维护基础架构方面，它可以简化组织面临的挑战。

另一方面，Kubernetes 本身很复杂，学习曲线很陡。如果您正在寻找 Kubernetes 是否适合您的团队的信息，[请查看我们对 K8s 利弊的深入探索](https://www.plural.sh/blog/pros-and-cons-of-kubernetes/)。

为了帮助您降低复杂性和避免失败，我们确定了七个最佳实践，您可以在使用 Kubernetes 时应用到您的工作中。无论你的团队最近决定开始 Kubernetes 之旅，还是你已经走了很远，这些都将是有帮助的。

# 升级您的 Kubernetes 版本

运行最新版本的 Kubernetes 将确保您获得新功能、错误修复和安全补丁。Kubernetes 的过时版本可能会在您的系统中打开漏洞，您可能会发现更难获得对旧版本的支持。除非您有理由维护旧版本，否则您应该升级到最新的稳定版本- [，可从 Kubernetes here](https://kubernetes.io/releases/) 获得。

# 使用名称空间

名称空间允许您组织集群，并在域或功能之间建立逻辑划分。一旦设置了名称空间，就可以为每个名称空间定义策略和访问规则。名称空间简化了容器管理，降低了大规模失败的风险。

为了最小化风险，您应该实践最小特权原则:每个人都应该只拥有执行其职能所需的权限。例如，为了提高安全性和防止事故，您应该确保开发人员只能访问与其工作相关的名称空间。这样，如果有人的帐户遭到破坏，恶意代理就无法对您的整个基础架构造成严重破坏。类似地，开发人员不会意外地覆盖系统中驻留在其他名称空间中的部分。

要深入了解名称空间以及如何设置它们，您可以通读 K8s 文档。

# 设置基于职责的访问控制(RBAC)

为了更容易实现最小特权原则，可以使用基于角色的访问控制。

RBAC 使管理授予您的用户或服务帐户的权限和访问变得更加简单，减少了出错的可能性。 [Kubernetes 允许您在名称空间内或跨集群设置角色](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)，并定义它们的权限。然后可以将帐户链接到这些角色，确保他们只拥有这些角色授予的权限。

在上面的例子中，为了将开发人员限制在特定的名称空间中，开发人员将被赋予这些名称空间中的角色。这些角色授予的权限允许他们部署自己的代码，但不能更改集群的更广泛的操作。

定期维护组织的角色和权限对于确保仅授予适当的访问权限非常重要。与为每个帐户单独管理权限相比，角色使这种维护更加简单和安全。

# 使用标签组织集群

Kubernetes 中的标签允许您将键值对附加到 K8s 对象上。作为一个编排平台，Kubernetes 允许您定义对象来维护您的单个集群及其状态的抽象层。随着基础设施的增长，您最终也会拥有越来越多的对象。标签使管理这些对象变得更加容易。基本上，您可以使用它们来定义和跟踪元数据。例如，您可以使用标签来跟踪谁是 pod 的所有者。

更好的是，您可以使用选择器查询标签和批量管理对象。例如，如果您包括一个标签来跟踪每个 pod 被指定用于哪个环境，您可以通过命令行使用如下命令来查询您的所有 QA 对象:

$ ku bectl get pods-l " environment = QA "-显示标签

更多关于标签的信息和如何设置选择器的例子可以在[的 Kubernetes 文档](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)中找到。

# 使用基于 Git 的工作流

Kubernetes 的部署可能很复杂，因此拥有一个自动化的工作流将减少麻烦和错误。设置基于 Git 的工作流和 CI/CD 管道允许您为您的部署维护单一的真实来源；通过自动部署，您的系统将自动反映您的回购中的内容。GitOps 是一个常用的框架，用于组织和支持您的工作流。

如果在系统运行过程中出现任何问题，基于 Git 的工作流也会使回滚或重新部署变得更加简单。

# 设置自动监控

监控您的群集对于识别问题和控制资源使用至关重要。集群的问题会降低产品的性能，增加运营成本，在最糟糕的情况下，还会导致停机。监控将允许您更快地识别这些问题并了解其原因。

您应该设置自动监控来帮助您理解 Kubernetes 警报。像[普罗米修斯](https://prometheus.io/)和[格拉夫纳](https://www.plural.sh/applications/grafana)这样的工具将帮助你从数据中提取洞察力，让你专注于最重要的事情，并对你的运营做出更明智的决策。

# 设置网络策略

虽然允许您的容器与防火墙后的任何其他服务进行通信看起来足够安全，但是如果恶意行为者进入您的系统，这可能会带来风险。

默认情况下，您的容器应该拒绝任何流量，除非它来自特别允许的来源。K8s 文档提供了有关如何设置网络策略的更多信息。

# 继续完善你的实践

随着您的团队对 Kubernetes 越来越熟悉，继续完善您如何结合这些最佳实践。你也应该发展你自己的。每个团队都有不同的需求，但是为您的组织建立标准可以减少不必要的复杂性并防止错误。

这里还有最后一个提示:如果你正在云上部署一个开源应用程序，试着使用复数。

我们已经帮助世界各地的工程团队管理他们在 Kubernetes 上的应用。查看[我们的 Github](https://github.com/pluralsh/plural) 并按照我们的[文档](https://docs.plural.sh/)开始运行。

加入我们的 [Discord channel](https://discord.gg/pluralsh) 进行提问、讨论，并与社区的其他成员见面。

*原载于 2022 年 10 月 17 日*[*https://www . plural . sh*](https://www.plural.sh/blog/7-kubernetes-best-practices/)*。*