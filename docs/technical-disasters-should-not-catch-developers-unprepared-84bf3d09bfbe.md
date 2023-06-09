# 技术灾难不应该让开发者措手不及

> 原文：<https://itnext.io/technical-disasters-should-not-catch-developers-unprepared-84bf3d09bfbe?source=collection_archive---------2----------------------->

## 不是会不会出错，而是什么时候出错

![](img/5e7725f71df9e71496ab86c8653b2216.png)

照片由[从](https://www.pexels.com/@shvets-production?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)[派克斯](https://www.pexels.com/photo/man-falling-carton-boxes-with-negative-words-7203956/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄制作

> *“如果灾难性的结果是可能的，或者你无法判断不利的一面，那就远离它。”—彼得·贝韦林*

如果你在软件项目上工作了足够长的时间，你将遭受一场技术灾难。构建将会崩溃，服务器将会爆炸，环境将会变得腐败，某个有香肠手指的人将会删除数据。

如果一个软件项目可能会出错，那么在某个时候它也会出错。技术出错，人类犯错，设计有缺陷，人们离开，事故发生的概率是他们会发生(被称为[墨菲定律](https://en.wikipedia.org/wiki/Murphy%27s_law)

软件开发团队需要在出现问题时快速恢复。这就是最佳实践的目的，这就是为什么我们有源代码控制、DevOps 和文档。

如果开发人员没有准备好快速恢复，他们就没有做好自己的工作..

# **Covid 不是黑天鹅**

我在看关于 COVID 的视频— [美国的疫情政策:失败、成功和教训](https://marginalrevolution.com/marginalrevolution/2022/03/us-pandemic-policy-failures-successes-and-lessons.html)

> 这不是黑天鹅事件。这是一个完全可以预见和预测的事件。我们知道这将会发生…然而，我们还没有准备好。亚历克斯·塔巴罗克

科学家警告说，一种类似 Covid 的病毒，一种致命的流感将在某个时候发生。这在过去发生过，将来也可能发生。每个国家都有责任做好准备。

2015 年 4 月，比尔·盖茨在 TED 演讲— [比尔·盖茨:下一次爆发？我们还没准备好。曾有过诸如非典、禽流感等预警。](https://www.youtube.com/watch?v=6Af6b_wyiwI)

然而，当 Covid 出击时，世界还没有准备好，并陷入了被动。

# **蔚蓝的心态**

开发者应该遵守这样的口头禅— **如果它会出错，它就会出错**。我称之为蔚蓝心态。

Azure 不会试图阻止服务器崩溃，因为不可能每次都阻止服务器崩溃。

相反，Azure 专注于快速恢复。如果服务器坏了，我们几乎可以立即弹出另一个。

Azure 使一切都可以脚本化，以使 DevOps 能够在需要时快速恢复和回滚。

# **最佳实践**

开发灾难是一个棘手的问题，它会扰乱计划并使开发推迟。最佳实践应该最小化灾难，使开发团队能够快速恢复。

我们并不总是意识到这一点，但是软件开发的最佳实践避免了开发团队被灾难摧毁。

*   备份使我们能够恢复数据并最大限度地减少数据丢失
*   确保我们没有单一的失败来源。
*   我们可以捕捉错误并有重试机制。
*   源代码控制会备份代码版本，因此我们可以回滚
*   DevOps 使我们能够快速恢复普通环境
*   每夜构建发现构建问题
*   单元测试和自动化测试在代码更改后发现错误
*   警报保护性地警告用户服务停止并减少停机时间
*   DevOps 减少了手动部署错误
*   文件阻止个人离开

# **软件开发灾难**

我曾经参与过软件项目，在这些项目中，事情变得一团糟，个人不小心删除了一些东西，环境变得腐败，没有人能找出原因。

重要的不是找出该责怪谁。重要的是我们如何尽快从这种情况中恢复过来。

我们部署了多种最佳实践的项目，恢复简单明了，只需一天时间。它仍然会干扰我们，但我们可以快速轻松地重置环境。

这些项目采用了更牛仔的方法，包括开发人员的英勇工作，提出创新的方法来修复它，然后在工作时间之外花费数小时的精力来修复它。

低质量的开发团队需要一个英雄，虽然每个人都在祝贺所有参与的人，但我把它当作一个警告。这一次我们很幸运，我们没有进行适当的开发。下一次，我们可能就没有这么幸运了，我不想参与到一个由于糟糕的开发实践而导致失败的开发中。

当你看到英雄开发人员时，这不是一件值得庆祝的事情，这是糟糕的开发实践的症状。这是一个警告，你需要改进你的开发过程，否则开发之神会把你打倒。

# **结论**

开发人员无法阻止软件开发中的所有错误、失误、难题和问题。在复杂的开发环境中，预测所有的问题是不可能的。

当你意识到你不能解决所有问题时，你可以专注于用最少的努力快速恢复。

对于软件开发来说，不是事情是否会出错，而是什么时候出错。你不能阻止所有问题的发生，但当问题发生时，你可以做好恢复的准备。

技术问题不是黑天鹅事件，可以也应该预测。开发者应该做好准备。