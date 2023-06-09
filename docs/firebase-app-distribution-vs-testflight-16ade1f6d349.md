# Firebase 应用程序分发与测试飞行

> 原文：<https://itnext.io/firebase-app-distribution-vs-testflight-16ade1f6d349?source=collection_archive---------3----------------------->

## 测试移动应用

![](img/6fd82dcab7987afa1971de8ab2c234af.png)

纳丁·沙巴纳在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

也许软件开发最重要的部分是从用户那里获得反馈。对于移动应用程序开发来说，有多种有效的方法可以做到这一点。最流行的两个是 Firebase 应用程序分发和测试飞行。以下是每种方法的优缺点以及我们使用它们的目的。

# 试飞

Testflight 是苹果向愿意测试你的应用的用户分发 iOS 应用的解决方案。从苹果公司来说，这一直是一个精简、无痛的过程，然而仍然有一些缺点。以下是主要优势:

*   没有测试人员的配置文件步骤——这意味着管理用户非常容易。
*   为测试人员提供简单的 bug 报告/截图共享特性。
*   易于开放给整个社区与公众共享链接。
*   没有额外的注册。
*   如果你在测试完成后感到满意，你可以直接提交到应用商店。

主要缺点如下:

*   在新版本发布之前，需要一个(测试版)苹果审查——这通常相当快，但可能需要长达一周的时间，这可能会令人沮丧。
*   测试人员需要下载 Testflight 应用程序。
*   测试人员数量有限——尽管这对于 10，000 名用户来说相当大，但是大公司可能需要更多的测试人员。
*   不跨平台—这是因为 Firebase 应用程序分发是跨平台的。

# Firebase 应用程序分发

Firebase 是谷歌为应用程序开发提供的产品套件。应用分发是其中的一部分。由于各种原因，它很容易设置并且很有用。以下是主要优势:

*   跨平台测试——这意味着你可以将你的 Android APK 和你的 iOS 版本上传到同一个服务，并分发给合适的用户。
*   可以测试该应用的用户数量没有限制。
*   如果您已经使用 Firebase analytics，则很容易实现
*   更容易比较和管理不同的测试版本

主要缺点是:

*   为新用户更新预置描述文件很麻烦，这意味着您必须为每个新用户更新 iOS 预置描述文件，以便用户安装您的应用程序。
*   用户必须创建一个帐户。
*   项目中增加了额外的 Firebase 库。
*   不像试飞那样无缝的体验。

# 用例

我们使用 Firebase 应用程序分发和 Testflight 来收集用户反馈，我们的内部政策是当应用程序使用暂存或开发 API 而不是实时 API 时，使用 Firebase 应用程序分发。这是一个额外的安全层，以确保我们不会意外发布使用错误 API 的应用程序。

你可能有类似的情况，或者你可能更喜欢其中一个，两者都是收集用户反馈的切实可行的解决方案。

*最初发表于*[*【https://www.brightec.co.uk】*](https://www.brightec.co.uk/blog/firebase-app-distribution-vs-testflight)*。*