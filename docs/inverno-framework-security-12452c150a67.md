# Inverno 框架安全性

> 原文：<https://itnext.io/inverno-framework-security-12452c150a67?source=collection_archive---------1----------------------->

![](img/ba9f98a635a81ea534654610e4dd9732.png)

继 TechEmpower 最新发布的 [Web 框架性能指标评测](https://www.techempower.com/benchmarks/#section=data-r21&test=composite)证实了其良好的性能之后，最新发布的 [Inverno 框架](https://inverno.io)带有一个可扩展的安全 API 和一组模块，允许保护从独立应用到微服务应用的任何类型的应用。

应用程序安全是一个复杂的主题，涉及到许多问题，如认证、识别、授权、加密……以及多种技术来防止越来越复杂的攻击。一年来，许多与安全相关的规范已经发布，以解决这些问题，但在保护应用程序时，很容易迷失方向。

为了简化这项任务，Inverno Framework 安全 API 已经围绕一个明确的安全模型进行了设计。

本文旨在向您概述如何在 Inverno 应用程序中实现安全性，并介绍一些常见功能，如果您需要更深入了解，请参考 [Inverno 框架参考文档](https://inverno.io/docs/release/reference/html/index.html#security)。

# 安全模型

Inverno 安全模型基于三个主要概念:

*   **认证**包括对应用程序请求的认证。它基本上包括对请求中提供的任何类型的凭证(例如用户名/密码、令牌……)进行认证(而不是请求背后的实际实体)。
*   **标识**涵盖了认证请求背后实体的标识。我们可能并不总是拥有标识，因为应用程序不需要它，或者因为身份验证机制不允许获取身份信息(例如 OAuth2)。
*   **访问控制**涵盖了基于角色、权限的认证实体对应用程序中受保护服务或资源的访问控制……至于身份识别，访问控制取决于应用程序和认证机制，可能并不总是可用。

这三个概念是安全 API 的基础，实际上是正确保护对应用程序的访问所需要知道的全部内容。

# 安全经理

安全管理器(或 Web 应用程序的安全拦截器)是主要的安全组件，负责验证凭证，并使用身份信息和访问控制器创建结果安全上下文，然后可以使用访问控制器来保护通过应用程序对服务和资源的访问。

下面展示了如何创建一个能够认证登录凭证(用户名/密码)、解析被认证实体的实际身份的安全管理器和一个基于[角色的](https://en.wikipedia.org/wiki/Role-based_access_control)访问控制器:

在上面的代码中，用户验证器使用用户存储库来解析可信凭证，并使用登录凭证匹配器将它们与所提供的登录凭证进行匹配。用户存储库是一个安全的存储库，用户凭证存储在内存或外部数据存储(例如 Redis)中。由此，当应用程序需要身份信息和/或访问控制器时，可以指定身份解析器和/或访问控制器解析器。

如您所见，这种方法非常灵活，允许根据应用来认证不同种类的凭证、解析不同种类的身份和访问控制器。例如，可以更改或组合身份验证器来验证其他类型的凭证或扩展的登录凭证，以便实现双因素身份验证，而不影响身份和访问控制解析器。

# 安全上下文

安全管理器随后可用于认证凭证并获得相应的安全上下文:

在上面的代码中，要进行身份验证的登录凭据是使用用户名和原始密码创建的，但是也可以指定编码密码。然后调用安全管理器来认证凭证并获得安全上下文，因为安全管理器总是完全被动的。

然后，生成的安全上下文可以在失败的身份验证时被拒绝，在指定空凭据时被匿名，或者在成功的身份验证时被身份验证。这允许让应用程序决定在不同的用例中做什么。例如，以下代码显示了处理安全上下文的正确方式:

# 身份

如果安全管理器能够解决的话，安全上下文可以暴露被认证实体的身份。身份应该在需要身份信息时使用，例如获取电子邮件地址或任何其他关于被认证实体的有用信息:年龄、办公地点、URLs

实际的身份类型由安全管理器确定，并且可能取决于应用程序和身份验证机制。例如，LDAP 目录、简单的用户存储库或开放的 ID 令牌提供了关于被认证实体的不同信息。

# 访问控制

安全上下文还可以展示访问控制器，以基于其角色、许可或任何其他访问控制机制来控制被认证实体对受保护服务或资源的访问。

上面的安全管理器已经被配置为从认证用户的组中解析基于[角色的](https://en.wikipedia.org/wiki/Role-based_access_control)访问控制器。这是可能的，因为用户存储库中的用户可以放在组中，就像 LDAP 目录中的用户一样。正如您所看到的，对于身份，访问控制器的种类取决于应用程序和可用于解析它的信息。

基于角色的访问控制器用于确定经过身份验证的用户是否具有特定角色、一组角色中的任何或所有角色:

在上面的代码中，我们试图确定被验证的实体是否具有角色`admin`，当没有访问控制器可用时，该角色总是`false`。

安全模块还提供基于权限的访问控制，它支持参数化的权限，并通过考虑功能上下文来允许对访问的精确控制。例如，可以确定是否授予了访问特定类型资源的许可。

在上面的代码中，基于权限的访问控制器用于确定被认证的实体是否对`article`文档拥有权限`create`。请注意，如果缺少访问控制器，则会引发异常。

# 摘要

本文中描述的概念是 Inverno 应用程序安全性的基础，该安全性始终基于安全管理器中包含的身份验证、标识和访问控制，以便对凭据进行身份验证，并生成实际用于保护服务和资源的应用程序安全上下文。

安全模块提供各种实现和更具体的功能来帮助您保护独立应用程序和 Web 应用程序。可用特性列表相当完整，允许在应用程序中正确实现安全性，包括:

*   根据用户存储库(内存中，Redis)进行身份验证和识别
*   基于令牌的认证
*   使用消息摘要的安全密码编码， [Argon2](https://en.wikipedia.org/wiki/Argon2) ， [PBKDF2](https://en.wikipedia.org/wiki/PBKDF2) ， [BCrypt](https://en.wikipedia.org/wiki/Bcrypt) ， [SCrypt](https://en.wikipedia.org/wiki/Scrypt)
*   [基于角色的访问控制](https://en.wikipedia.org/wiki/Role-based_access_control)
*   基于权限的访问控制
*   JSON 对象签名和加密支持(RFC7515、RFC7516、RFC7517、RFC7518、RFC7519、RFC7638、RFC7797、RFC8037、RFC8812)
*   针对 LDAP 和活动目录的身份验证和识别
*   HTTP 基本(RFC7617)和摘要(RFC7616)身份验证
*   基于表单的身份验证
*   跨产地资源共享支持( [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) )
*   抵御跨站点请求伪造攻击( [CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery) )

一份完整的[文档](https://inverno.io/docs/release/reference/html/index.html#security)和几份[用户指南](https://inverno.io)也可以在获得 Inverno 应用的过程中帮助你。