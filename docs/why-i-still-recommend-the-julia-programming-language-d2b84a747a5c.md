# 为什么我仍然推荐 Julia 编程语言

> 原文：<https://itnext.io/why-i-still-recommend-the-julia-programming-language-d2b84a747a5c?source=collection_archive---------1----------------------->

## 尤里·维什涅夫斯基对朱莉娅的批评有效吗？

我被要求对尤里·维什涅夫斯基在他的文章[“为什么我不再推荐茱莉亚”](https://yuri.is/not-julia/)中提出的批评做出回应。作为茱莉亚的超级粉丝和《T4:把茱莉亚作为第二语言》的作者，我觉得有义务严肃对待对茱莉亚的批评。

我认为尤里提出了很多有效的观点，但我真的认为现在下结论还为时过早。Julia 还是一门很年轻的语言，生态系统还不成熟。不成熟的库和工具中存在缺陷并不是什么新鲜事。我做 iOS 开发的时候，Swift 还很新，经常处理一些模糊的问题。我在做手机 app 的时候遇到了大量蹩脚的 Objective-C 库。

然而，与我遇到的所有 JavaScript 垃圾相比，所有这些都相形见绌，尤其是在移动开发方面。尽管如此，许多人还是坚持使用，因为这是他们喜欢并希望使用的工具集。

大多数 Julia 库还不成熟，贡献者很少。与拥有大量开发人员和大量资源的其他平台上的成熟库相比是不公平的。[阿帕奇箭头](https://github.com/apache/arrow-julia)就是一个很好的例子。Julia 在 C++参考平台之前就获得了特性的完整性，拥有一个大型开发团队，有着领先的优势和更多的资源。除非我记错了，朱莉娅只有一个人做这项工作。

如果你让一个人打败了整个团队，我认为朱莉娅应该被放一马，因为她在流行的库中有更多的 bug。朱莉娅目前根本没有其他语言所拥有的资源。老实说，我不确定这是不是一个有效的说法。当你有了可以做更多事情的库时，将会有更多的死角需要处理。Julia 倾向于提供极其灵活多样的库。

为什么 PC 平台的问题比 Mac 平台多？因为硬件上的差异要大得多。更多配置选项。当比较 iOS 和 Android 时，情况也是如此。当我还是一名移动开发人员时，我确实记得测试 Android 软件有多难，因为我们要处理太多的屏幕尺寸和形状因素。

问题是作为一名开发人员，你能实现什么，你能控制什么？我必须说我比尤里·维什涅夫斯基有更积极的经历，但我也可能以一种非常不同的方式使用朱莉娅。我更像是一个老派的软件开发者。这意味着我使用朱莉娅的方式比其他人更保守。我避免太多聪明的诡计。我比其他人更多地使用类型注释，并试图将对库的依赖降到最低。

当选择要使用的库时，我试图避开复杂性。我更喜欢我容易理解的东西，如果需要的话，我可以修改它。人们感受到的痛点不同。对我来说，各种化身的复杂性总是主要的敌人。我确实在很大程度上相信“越差越好”的 Unix 哲学。在我看来，略微错误和简单比非常复杂和正确要好。

原因如下:

> 如果我能掌握代码，我就能修复它。不正确的代码可以转换成正确的代码。在软件不断变化的世界中，正确性通常是一种暂时的属性。今天有效的明天就可能被打破。因此，当问题出现时，解决问题的难易程度比在这种情况下正确与否更重要。

这是我发现朱莉娅有利的地方。我发现在 Julia 中解决问题比在许多其他语言中容易得多。一个重要的原因是 Julia 提供的极好的 REPL 环境，以及它与语言优势的契合度。我最近一直在 Go 中运行调试器，每次我在传统的静态语言中使用调试器一次一行地调试代码时，我都会想起调试器对于高质量的 REPL 环境来说是多么糟糕的替代品。

但这不仅仅是 REPL 的问题。使 Julia 易于使用的是它非常实用而又不过分。当输出完全依赖于输入时，分析代码变得容易多了。情况并非总是如此，但是 Julia 开发人员擅长标记变异函数，所以您可以很容易地意识到变异输入的函数。

底线是，我不是说尤里·维什涅夫斯基是错的，只是我重视不同的东西。对我来说，拥有简单易用的工具是最重要的。Julia 比今天的许多主流语言更容易使用，我不认为这只是我内心的 Julia fanboy 在说话。我帮助过放弃 Python 的初学者，也帮助过热爱 Julia 的人。非专业开发人员，他们想做一些简单的东西，但是无法处理 Python 生态系统的复杂性。

当然，Python 的生态系统非常成熟，这意味着你得到的包 bug 更少。但另一方面，要应对的环境要复杂得多。Python 2.x vs 3.x 问题， [Anaconda vs PIP](https://www.anaconda.com/blog/understanding-conda-and-pip) ， [pipenv](https://pipenv.pypa.io/en/latest/) ，virtualenv， [venv](https://docs.python.org/3/library/venv.html) ，[JAX](https://github.com/google/jax)vs[PyPy](https://www.pypy.org)vs[Numba](https://numba.pydata.org)， [Cython](https://cython.org) 不胜枚举。使用 Julia，您可以在一个标准包中获得包管理、模块、依赖项和环境。对于初学者来说，这要容易得多。

Julia 在组合包时所面临的问题在 Python 包中非常常见。他们并不总是能很好地合作，解决这些问题并不容易。

这种复杂性存在于许多语言中，尤其是我在职业生涯中花了很多时间使用的语言:C++。像 Scons、Ninja 和 CMake 这样的构建工具不胜枚举。它们的复杂性很快就达到了需要专家来维护的程度。当我作为一名 C++开发人员工作时，我从未真正理解过我所处理的构建系统。在 Xcode 中设置构建并不困难，但有点像黑盒，你会很快迷失在编译器选项、库路径、链接器选项、资源目录、二进制签名等等。

C++中的依赖管理通常很糟糕。当两个库依赖于第三个库的不同版本时，你很快就会遇到麻烦。长期以来，C++标准库一直很小，分发库也很笨拙，这导致了像 Qt 这样的大型库的传统，它们不一定能很好地与其他库兼容。在任何中等规模的项目中，处理 3-5 个不同的不兼容的字符串类似乎是正常的事情。

结果是你花了很多时间编写粘合代码来连接所有这些不同的库。这是朱莉娅的一个有趣的想法。处理如此多的 Julia 库的几乎任何类型的灵活性使得测试 Julia 库并确保它们在所有可能的组合中工作如此困难。尤里·维什涅夫斯基正确地指出这是一个问题。然而，他从未真正阐述过没有这种灵活性的含义。反 Julia 是 C++，我一点也不相信这是一个合适的选择。当每个人都必须编写胶合代码时，你就增加了出错的机会。每个项目基本上都是一遍又一遍地解决同一个问题。

相反，对于 Julia 来说，当一个集成问题被发现时，它会以一种有益于所有项目的方式得到解决。毕竟，胶合代码并不是任何人都可以作为可重用组件发布或分发的东西。朱莉娅在许多方面都在做一些以前没有大规模做过的事情:

> Julia 正在帮助创建一种通用的、可重复使用的胶水代码。从本质上来说，这很难做到正确，但一旦解决了，好处是巨大的。

粘合代码可能非常庞大。Godot 游戏引擎的创造者们尝试使用 Lua、JavaScript、Python 和其他语言作为他们的脚本语言。最终，他们选定了自己的语言 GDScript。事实证明，与其他脚本语言所需的粘合代码行相比，完全实现 GDScript 所需的代码行更少。

这里还有一个例子:太空火箭需要大量的软件来控制它们。美国宇航局太空发射系统(SLS)通过购买各种模块并将它们集成在一起解决了这个问题。SpaceX 从零开始打造一切。同样，美国宇航局编写的胶水代码最终比整个 SpaceX 控制软件还要大。

粘合代码很容易忘记，但它可能是大规模的，并且可能是错误和问题的主要来源。是的，不得不在 Julia 库中归档 bug 或自己添加补丁可能很糟糕，但当你这样做时，你也帮助了别人。当你修改粘合代码时，除了你自己，你并没有真正帮助任何人。没有重用。

尤里·维什涅夫斯基的批评是朱莉娅社区关注薄弱部分和改进的机会，但我不认为这代表棘手的问题。我们许多人都很享受 Julia 带来的生产力和编程乐趣的显著提高，这是实实在在的。许多人在用 Python、Matlab、Fortran、R 或其他语言碰壁后，找到了 Julia，这些语言无法提供他们所寻求的性能或生产力。就个人而言，作为一名开发人员，C++几乎让我精疲力尽，而 Julia 是让编程再次变得令人愉快的语言之一。我认为现在放弃朱莉娅还为时过早。我们仍然有一个小社区，但它有很大的发展潜力。

朱莉娅真正需要的是让谷歌、微软、苹果、亚马逊或 IBM 这样的大公司以某种方式做出贡献。我认为，更多有行业经验的专业开发人员会大大提高质量。朱莉娅直到现在几乎完全依赖于学术界和科学界。

很抱歉，对于这个问题，我真的没有更多实质性的内容可以补充。客观地衡量 Julia 图书馆的质量将是一项艰巨的任务。与 Python 项目或 C++项目相同的年数相比，用 5 个人年开发的 Julia 项目质量如何？阿帕奇阿罗的经验表明，我们常常以苹果和橘子的比较而告终。我们冒着在功能上比较类似库的风险，却没有意识到 1/10 的资源是用来制作 Julia 版本的。那么，我们能指责朱莉娅版本多了 50%的错误吗？

我认为这个问题的后续应该选择一些流行的库，比较 Julia 和竞争语言之间的能力、资源、流行程度和 bug 的严重性。