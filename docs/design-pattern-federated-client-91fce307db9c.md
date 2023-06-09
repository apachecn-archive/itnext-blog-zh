# 设计模式:联邦客户端

> 原文：<https://itnext.io/design-pattern-federated-client-91fce307db9c?source=collection_archive---------4----------------------->

## 模块联合设计模式

![](img/a8a9b294b92ac4911c4590d06bc06478.png)

这是关于模块联合设计模式的系列文章的第一部分。由于 MF 是一种新技术，还没有被很好地理解，这些模式可能有助于增加清晰度和刺激创新。

目前，MF 是基于 Javascript 的，因此，*一些*模式只能在 Javascript 中实现，而其他模式是语言不可知的；但是在每一种情况下，模式本身，以及一般的 MF，都可以从它的实现中分离出来考虑。因为我关注的是 MF 对服务器的应用，所以有些模式只适用于后端，有些是同构的。我会在每篇文章的开头强调这些品质。

*联邦客户机*模式是同构的。它可以在浏览器或服务器上运行。这种模式做两件事:

*   封装集成逻辑
*   防止重大变更

当您导入一个远程模块时，MF 会确定该模块的依赖项，并将这些依赖项一起导入。在大多数情况下，导入远程模块并在本地调用它是一种有效的解决方案。但是，在其他情况下，该模块可能需要访问从您的位置无法访问的网络，或者它可能有太多的依赖项，因此导入不切实际。

在这种情况下，服务可以公布一个联邦包装器模块，该模块封装了一个 API 客户机并导出映射到它的函数。就导入这个模块的 app 而言，它只是进行本地的、进程内的函数调用。该模块包括用于将导出函数的自变量转换成通过网络传输的串行化有效载荷的逻辑。服务器可以动态地改变这个逻辑，将消费应用程序与负载格式或客户端本身的破坏性变化隔离开来。

由于服务提供商可以加载新的依赖关系并动态更新转换逻辑，因此不仅可以在几分钟内完成过去需要几个月才能完成的工作，还可以完成没有应用程序所有者参与的情况下不可能完成的工作(至少可以安排停机时间)，例如添加断路器、将客户端重新实现为 WebAssembly 模块或将传输协议从 TCP 切换到 UDP。

在这个系列中寻找更多的模式。欢迎评论和提问。