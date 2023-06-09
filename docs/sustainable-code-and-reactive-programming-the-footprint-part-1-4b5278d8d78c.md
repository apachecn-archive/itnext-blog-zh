# 可持续代码和反应式编程——足迹(上)

> 原文：<https://itnext.io/sustainable-code-and-reactive-programming-the-footprint-part-1-4b5278d8d78c?source=collection_archive---------5----------------------->

![](img/9cf64f474128b95be0aa42c9eb4399b3.png)

## 介绍反应式编程试图解决的问题

软件无处不在。智能汽车、冰箱和微波炉也是如此。即使我们不去想它，我们的日常生活也依赖于它。随着它不断扩展并占据我们的世界，找到让它清晰可读，最重要的是让它简单明了的方法就变得至关重要。在大规模管理复杂性时实现简单性是开发人员每天面临的主要挑战。

通常，当我们想到可持续发展，我们联想到食品行业，柴油汽车，大型船舶，能源和住房部门的问题。但不太明显的是，软件开发的各种体系结构模式，特别是算法，可能会有非常不同的碳足迹。我们并不是不关心这个问题——每个人都希望自己的智能手机使用更长的电池续航时间，或者从云服务提供商那里拿到更少的账单。但是，像没完没了的空值检查或意大利语代码这样的事情如何影响二氧化碳排放和我们关键基础设施的安全性，就不那么明显了。在本系列文章中，我将介绍 Reactive Programming，并解释为什么许多专家认为应该更广泛地实践它来创建可持续的应用程序。

[杰勒德·贝瑞](https://en.wikipedia.org/wiki/G%C3%A9rard_Berry)——法国计算机科学家在他的论文《T4 实时编程:专用或通用语言》中已经在 1989 年提出了一些实际的现实问题，反应式编程范式在允许同步和异步执行模型时应该解决这些问题:

> 实时编程是一项重要的工业活动。工厂、飞机、汽车和各种各样的日常生活对象都是或将是计算机控制的。实时程序接收外部中断或读取连接到物理世界的传感器，并生成命令作为其输出。在此过程中，它们必须在外部固定的时间约束内对输入做出反应。对于大多数实时程序来说，安全性是一个至关重要的问题。在这个领域，一个 bug 可能会带来极端的后果。

今天，这一点比以往任何时候都更加正确。我们进入了一个时代，每样东西都通过软件得到增强。这种智慧是有代价的。巨大的网络系统，如果编程不当，可能会因为基础设施的某个不相关部分未能提供预定的输出而经历严重的故障。在我强烈推荐大家阅读的文章《即将到来的软件启示录》中，许多计算机科学专家接受了 James Somers 的采访，就编程的现状发表了他们的看法。

[](https://www.theatlantic.com/technology/archive/2017/09/saving-the-world-from-code/540393/) [## 即将到来的软件启示录

### 一小群程序员想在灾难来临之前改变我们的编码方式。在…期间有六个小时

www.theatlantic.com](https://www.theatlantic.com/technology/archive/2017/09/saving-the-world-from-code/540393/) 

人们一致认为，现代软件注定会失败，因为普通项目的规模和复杂性对于任何一个程序员或团队来说都是不可理解的。他们把它命名为**蒙眼** **开发**。

> 问题是程序员很难跟上他们自己的创作。自 20 世纪 80 年代以来，程序员的工作方式和他们使用的工具几乎没有什么变化。有一个人数不多但越来越多的合唱团担心现状是**不可持续的** ……程序员就像试图戴着**眼罩**下棋的棋手——他们太多的精神能量都花在试图描绘棋子在哪里，几乎没有任何剩余时间去思考游戏本身。

Bret Victor 认为软件开发实践的主要问题是程序员与他们试图解决的实际问题脱节。文本编辑器和过时 40 年的计算机科学概念阻碍了发展。在他史诗般的演讲“发明原理”中，他强调了编程实践中即时性的重要性。

bret Victor——基于原则的发明

在这个视频中，Bret 认为，作为程序员，我们在道德上有责任批判性地思考我们自己的工作，甚至质疑这个职业的基本支柱。我们应该努力减少瓶颈，发现只有我们自己才能看到的问题。即使这意味着自动化我们自己的工作场所——减少不必要的复杂性，使软件系统更加直观和面向目标应该是我们的主要目标。所有这些听起来可能相当激进，但是通过不断进入和接受更高层次的抽象，我们可以创造出具有即时性的工具，鼓励创造力和实验性，并对世界产生更大的影响。

# 朝向反应性

在过去的十年中，开发人员在更直观的软件架构方面取得了一些进展。从专业的角度来看，函数式编程似乎终于进入了主流。创建了许多新的库和方法来管理我们代码库中的状态和信息流。这些框架每天都在涌现，为思考复杂性提供了新的思路。反应式编程是最近开始从各种开发人员社区获得更多采用的想法之一。这不是一个新概念，而是一个可以产生巨大影响的老方法。在整个系列文章中，我将详细分析和讨论它。

另一方面，在专家型系统中，我们看到了简化编程的工具，并提供了集成开发环境的可能未来的一瞥。虚幻引擎及其蓝图就是一个很好的例子。蓝图使新手和有经验的游戏开发者都能够创建可视脚本来处理事件驱动的编程任务。这些脚本被编译成代码，可以在游戏产品中运行，不会出现严重的性能问题。如果你想看看这个介绍是什么样的，请查看:

[Grasshopper 3D](http://www.grasshopper3d.com/) 和 [Dynamo BIM](http://dynamobim.org/) 是面向架构师的非常流行的参数化软件开发工具的两个例子。通过这些高级脚本接口，我们可以看到复杂的参数化 3D 结构在画布上相互连接时立即出现。 [ANSYS SCADE](https://www.ansys.com/products/embedded-software/ansys-scade-suite) 是用于关键任务控制系统的类似解决方案。还有更多[可视化编程语言](https://en.wikipedia.org/wiki/Visual_programming_language)像 [NoFloJs](https://noflojs.org) 、 [Scratch](https://scratch.mit.edu) 等。如今，几乎每个领域都开发了类似的编码工具和方法。编写文本指令不再是成为程序员的先决条件，但是在这样一个世界中，架构模式变得更加重要。

几乎所有这些环境都有共同点。它们将特定的字段抽象成简洁易读的函数。对这些脚本所做的更改会立即反映在代表正在构建的产品的编辑器中。代码被抽象成具有明确定义的输入和输出的小功能块。这些脚本是松散耦合的，具有成功处理规模和失败场景的潜力。如果一个块未能传递结果，并不一定意味着整个操作都会失败。即时性使得在开发代码时更容易发现代码中可能的错误。块只能通过它们的输出影响其他块。在大多数这样的系统中，你甚至不能设置任何全局状态，所以不变性是默认的，这种系统的所有黑盒都可以被视为纯函数。流经脚本的信息非常清晰可见，因此对程序员来说是可预测的。如果这一切听起来对你来说很熟悉，那么你是完全正确的。许多[函数式编程](https://en.wikipedia.org/wiki/Functional_programming)原则被用来实现这些脚本环境。如果在我们的文本优先编辑器和语言中实现，这种可视化编程语言提供了反应式编程模式可能采取整个实践的一瞥。

我甚至认为虚幻引擎中使用的蓝图实现了视觉效果，可以帮助我们形成反应式编程的心理模型。在游戏引擎中，就像在反应式编程中一样，时间是至关重要的，所以一定要检查蓝图是如何工作的。如果你从未见过它的实际操作，我保证你会觉得它很酷。我希望这个关于复杂性和一些开箱即用的编程方法的讨论足够有趣，能够启发你开始挖掘反应式编程的更具体的细节。

# 什么是反应式编程？

[Andre Staltz](https://twitter.com/andrestaltz) (反应式编程的倡导者之一)做了一个演讲，我认为对任何刚进入这个领域的人来说都是一个很好的参考。Andre 在他的演讲中用一句话有效地定义了反应式编程:

> 反应式编程——当被改变的模块负责定义该改变时。

请务必阅读他的文章[你错过的反应式编程简介](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754)。模块化代理的思想涵盖了围绕反应性发生的一切。无论是 Excel 单元格、纯函数还是虚幻引擎蓝图模型中的组件。任何被改变的模块都包含了定义改变含义的所有逻辑。这是反应式编程满足的基本规则。当实现异步数据流模型时，它允许断开的功能代理一致地工作，而不会泄漏或改变外部状态。异步数据流和组件级的关注点分离使得这些系统具有高度的可伸缩性。

将代码分割成像模块一样包含所有状态的代理也允许它们的行为由输入和输出表面一致地表达。这些容器在整体架构中使用黑盒还是白盒原则并不重要，重要的是它们保持与沙盒原则一致，这允许它们形成一致的、防故障的、高度可扩展的和交互式的系统。如果你想更多地了解这种模式的优势，请查看[凯文·韦伯](http://kevinwebber.ca/)的这篇精彩博文:

[](https://blog.redelastic.com/what-is-reactive-programming-bc9fa7f4a7fc) [## 什么是反应式编程？

### 探索反应式应用程序开发的指导原则。

blog.redelastic.com](https://blog.redelastic.com/what-is-reactive-programming-bc9fa7f4a7fc) 

总结时，凯文指出

> 反应式编程不仅仅是另一种趋势，而是现代软件开发人员学习的范例**。无论您选择哪种语言或工具包，将可伸缩性和弹性放在首位以实现响应性是满足用户期望的唯一方式。**

这种编程方法吸引了足够多的关注，值得拥有自己的宣言，在撰写本文时，已经有 22061 个人签名。确保检查它，了解它的重要性，如果你认为这些想法对编程实践很重要，就在上面签名。

[](https://www.reactivemanifesto.org/) [## 反动宣言

### 大系统由较小的系统组成，因此依赖于其组成部分的反应性质。这个…

www.reactivemanifesto.org](https://www.reactivemanifesto.org/) 

总的来说，反应式编程为我们提供了一种新的思考代码的方式。信息流构成了这种模式的本质。流经这些流的数据可以在各种管道功能中进行转换和过滤。这些事件还会触发其他事件流，这些事件流分支、被合并到干线或再次分支，直到通过这些流传输的信息到达其最终目的地。所有这些都意味着异步事件模型。

你可以想象一个物流系统，其中初始资源不是原材料，而是数据。它在系统的特定部分被开采，被运输到工厂，工厂对它进行定制、成型和再成型，直到它以最终产品的形式交付给最终用户。通过各种运算符功能，您可以描述时序和异步流起着非常重要作用的物流系统。在反应式编程中，你对待数据就像对待农业中的作物一样。一年中的某些时刻是收获庄稼的最佳时机。这些事件可能以不同的间隔发生。你可能想将原材料运输和储存在特殊的储存设施中。其他生产者可能希望它们在可变大小的块中按周计划被交付。再往下，不同的工厂可能使用不同的时间管理系统，生产更多不同的产品。最终用户也可能对时间表有严格的期望。如果出现延迟，系统应该能够适应意外问题，并尽可能具有弹性，如果出现此类问题，供应链的所有部分也应该能够取消合同。

我试图让这个想象的场景听起来复杂，但是这个依赖模型仍然比一般开发团队每天管理的要原始几千倍。从所有这些中得到的重要信息是，反应式编程允许非常有效地表达这种关系，从而简化了对我们的应用程序进行推理时的心理开销。您将获得一个特殊的词汇来以声明的方式描述这个系统，而不需要在回调地狱中结束。

在接下来的章节中，我将通过研究反应式编程的各种实现，并通过绘制有助于理解其各种优势的心智模型，进一步解释什么是反应式编程。