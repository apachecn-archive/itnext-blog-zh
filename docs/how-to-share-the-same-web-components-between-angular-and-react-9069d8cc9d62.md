# 🔥如何在 Angular 和 React 之间共享相同的 Web 组件？🤔

> 原文：<https://itnext.io/how-to-share-the-same-web-components-between-angular-and-react-9069d8cc9d62?source=collection_archive---------0----------------------->

让我们看看我们是否可以构建与任何框架(如 Angular 和 React)兼容的可共享 Web 组件

![](img/8294f7ed7c039c2f1280efc5eeef3fba.png)

# 内容

*   [**简介**](#c939)
*   [**为什么可重用的 UI 组件很重要？**](#a63a)
*   [**Web 组件到底是什么东西？**](#c16c)
*   [**概念验证**](#604f)
*   [**原型 Angular、React 应用程序和 UI 库**](#6379)
*   [**集成 UI 组件与 React 和 Angular**](#62a2)
*   [**GitHub 链接**](#4bab)
*   [**结论**](#dddf)
*   [**你真了不起，谢谢❤️**](#e5dc)
*   [**了解更多**](#89c5)

# 介绍

有大量的 UI 库为你提供了一套行业通用的组件，比如按钮、下拉菜单、模态窗口。最受欢迎的例子之一可以是材料设计或自举。它们的问题是它们并不兼容所有的 UI 框架，所以你需要以某种方式集成它们，或者使用特定于框架的版本。这种解决方案在大多数用例中是不可接受的，尤其是当产品在增长，一致性比以前更重要的时候。 ***让我们一起来了解一下，如何构建可以在任何 UI 框架中重用的 UI 库？***

# 为什么可重用的 UI 组件很重要？

构建统一的、可重用的 UI 组件在许多场景中都很有用。随着前端世界快速变化的趋势，像今天 React 更受欢迎，但明天 Angular，或者发布一个新的框架，在性能和灵活性方面比它们都好。或者框架的一个新的主要版本与以前的不兼容，结果是你不能重用你的旧组件，从 AngularJs 到 Angular 2+的过渡就是这样。所有这些技术变化的情况都会频繁地影响前端，并产生一个 ***迁移*** UI 组件的问题。

另一个场景是当您在构建多个项目的大型企业产品上工作时。这些项目重用了相同的 UI 组件。此外，项目可能使用不同的 UI 框架。在这种情况下，统一的 UI 组件将非常方便。

此外，另一种情况发生在一些快速发展的项目中，团队决定开源一些服务或组件。如果你也考虑与世界分享你的 Ui 库，这可能会对你的产品产生积极的影响，并通过提升你的品牌和吸引新客户来回报你，你需要考虑构建一个框架无关的 UI 库。

最后一个，如果你是一个创造者。并且你可能是那个想要参与开源项目来创建你自己的库的人。出于任何原因，如改善因果报应，为未来的雇主创建你的投资组合或只是提高你的前端可信度。

# Web 组件到底是什么？

那么，在筛选了可能从统一 UI 组件中获益的不同用例之后，问题仍然存在， ***如何实现呢？*** 如果我们想要这样一个灵活的解决方案，也受各大浏览器的支持，我们就要看看 ***Web 组件*** 的方向了。这在前端并不是一个新词，web 组件有着悠久的历史，第一次是由 Alex Russel 在 Fronteers Conference 2011 上介绍的。后来，像谷歌这样的大型科技公司设法建立了一个开源库 Polymer，它简化了基于 ***Web 组件*** 的构建应用。最后是浏览器，这样的火狐开始默认支持 ***Web 组件*** ，直到现在，Web 组件兼容所有主流浏览器。

在本文中，我们不打算深入研究 Web 组件，我们只展示基本实现，并让它们可以跨不同的框架重用。

# 概念证明

我们将使用 React 和 Angular 实现两个简单的应用程序。应用程序包括一个按钮，Angular 应用程序中的**增量**和 React 应用程序中的**减量**，点击它将相应地增加和减少数值。这两个应用程序将共享一个叫做**计数器的 ui 组件。它所做的只是简单地从父节点获取值并显示出来。最终结果将如下所示:**

![](img/7e158fc557ef00195f4f8072c74c4348.png)![](img/7a97a53559ccfb198c454e8b01998c4e.png)

# Angular、React 应用程序和 UI 库的原型

让我们直接跳到实现。在本例中，我们将使用 **nrwl** cli 快速构建 monorepo 原型，其中包含两个应用和一个共享库。

首先，让我们创建一个工作区并进入文件夹:

```
npx create-nx-workspace web-comp-app
cd web-comp-app/
```

接下来，我们需要安装所有的依赖项:

```
npm install — save-dev @nrwl/angular
npm install — save-dev @nrwl/react
```

然后，我们需要生成一个共享库，并向其中添加可重用的 ui 组件。

```
nx g @nrwl/web:lib shared/ui-components
cd libs/shared/ui-components/src/lib
mkdir wc-counter-value
cd wc-counter-value
```

并创建文件`wc-counter-value.element.ts`

元素准备好之后，我们需要将它从共享库中导出:

```
web-comp-app/libs/shared/ui-components/src/index.ts
```

# UI 组件与 React 和 Angular 的集成

最后一步是整合，这是最有趣的部分。让我们从 Angular 开始，它必须增加计数器的值。

我们需要确保 angular 可以接受非 angular 元素，为此，我们需要注入`CUSTOM_ELEMENTS_SCHEMA`。

```
web-comp-app/apps/ng-app/src/app/app.module.ts
```

这就是我们需要做的集成，我们现在可以直接使用`wc-counter-value.element.ts`。最后是更新`app.component.ts`。

完美，现在是时候将我们的共享组件与 React 应用程序集成了。请转到:

```
web-comp-app/apps/react-app/src/app/app.tsx
```

然后直接注射`wc-counter-value.element.ts`

差不多就是这样，为了安全起见，你可以克隆回购自己尝试，享受。

# GitHub 链接

[](https://github.com/Vitashev/web-comp-app) [## GitHub - Vitashev/web-comp-app

### 这个项目是用 Nx 生成的。🔎智能，可扩展的构建框架 Nx 支持许多插件，增加了…

github.com](https://github.com/Vitashev/web-comp-app) 

# 结论

尽管在一个组织中拥有多个框架并不常见，但是这个例子展示了使用 web 组件的灵活性。**如果你对这个话题感兴趣，这篇文章收藏 800👏鼓掌，我将发布关于构建 web 组件的最佳实践。如果你学到了什么新东西，请在 medium 和** [**twitter**](https://twitter.com/EasyWebOrg) **上关注我。**

[](https://medium.com/@easy-web/subscribe) [## 每当维塔利·舍甫琴科发表文章时，就收到一封电子邮件。

### 每当维塔利·舍甫琴科发表文章时，就收到一封电子邮件。通过注册，您将创建一个中型帐户，如果您还没有…

medium.com](https://medium.com/@easy-web/subscribe) [](https://medium.com/@easy-web/membership) [## 通过我的推荐链接加入 Medium 维塔利·舍甫琴科

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

medium.com](https://medium.com/@easy-web/membership) 

# 你太棒了，谢谢你，❤️

佩服你们这些人。你的**喜欢👍**和**跟随** ❤️—转化为正能量，粉碎了动力。爱和尊重你们所有人，你们是了不起的人🙌

)(我)(们)(都)(不)(知)(道)(,)(我)(们)(还)(不)(知)(道)(,)(我)(们)(还)(是)(有)(些)(不)(知)(道)(的)(人)(,)(我)(们)(还)(不)(知)(道)(,)(我)(们)(还)(是)(不)(知)(道)(,)(我)(们)(还)(是)(不)(知)(道)(,)(我)(们)(还)(不)(知)(道)(,)(我)(们)(还)(是)(不)(知)(道)(,)(我)(们)(还)(不)(知)(道)(道)(理)(,)(我)(们)(还)(不)(能)(不)(知)(道)(理)(。)(她)(们)(都)(不)(知)(道)(,)(她)(们)(还)(不)(知)(道)(,)(她)(们)(还)(不)(知)(道)(,)(她)(们)(还)(不)(知)(道)(,)(她)(们)(还)(有)(些)(不)(知)(道)(吗)(,)(她)(们)(还)(是)(不)(知)(道)(,)(她)(们)(还)(不)(知)(道)(,)(她)(们)(们)(还)(有)(些)(不)(知)(道)(,)(她)(们)(们)(还)(不)(知)(道)(道)(,)(她)(们)(们)(们)(还)(不)(知)(道)(道)(,)(她)(们)(们)(们)(还)(不)(知)(道)(,)(她)(们)(们)(们)(们)(还)(有)(些)( )(他)(们)(都)(不)(知)(道)(,)(我)(们)(还)(不)(知)(道)(,)(我)(们)(还)(不)(知)(道)(,)(我)(们)(还)(不)(知)(道)(,)(我)(们)(还)(是)(不)(知)(道)(,)(我)(们)(还)(不)(知)(道)(,)(我)(们)(还)(不)(知)(道)(,)(我)(们)(还)(是)(不)(知)(道)(,)(我)(们)(还)(不)(知)(道)(,)(我)(们)(还)(不)(知)(这)(些)(事)(,)(我)(们)(还)(不)(能)(不)(知)(道)(理)(。 )(她)(们)(都)(不)(知)(道)(,)(她)(们)(还)(不)(知)(道)(,)(她)(们)(还)(不)(知)(道)(,)(她)(们)(还)(不)(知)(道)(,)(她)(们)(还)(有)(些)(不)(知)(道)(吗)(,)(她)(们)(还)(是)(不)(知)(道)(,)(她)(们)(还)(不)(知)(道)(,)(她)(们)(们)(还)(有)(些)(不)(知)(道)(,)(她)(们)(们)(还)(不)(知)(道)(道)(,)(她)(们)(们)(们)(还)(不)(知)(道)(道)(,)(她)(们)(们)(还)(不)(知)(道)(道)(,)(她)(们)(们)(们)(们)(还)(有)(些)( )(他)(们)(都)(不)(知)(道)(,)(她)(们)(还)(有)(些)(不)(知)(道)(的)(情)(况)(,)(她)(们)(还)(不)(知)(道)(,)(她)(们)(还)(有)(些)(不)(知)(道)(的)(情)(况)(,)(她)(们)(还)(有)(些)(不)(知)(道)(的)(情)(情)(况)(,)(她)(们)(还)(不)(知)(道)(,)(她)(们)(们)(还)(有)(些)(不)(知)(道)(的)(情)(情)(况)(。

# 了解更多信息

[](/frontend-interview-cheatsheet-that-helped-me-to-get-offer-on-amazon-and-linkedin-cba9584e33c7) [## 🔥帮助我获得亚马逊和 LinkedIn 录用通知的前端面试备忘单

### 如果你正在准备一个前端面试，想快速更新你的领域知识，这个备忘单将…

itnext.io](/frontend-interview-cheatsheet-that-helped-me-to-get-offer-on-amazon-and-linkedin-cba9584e33c7) [](/how-micro-frontend-changes-the-future-of-angular-bb4deb2cfdad) [## 🔥微前端如何改变 Angular 的未来？

### 让我们看看为什么 Angular 最适合微前端

itnext.io](/how-micro-frontend-changes-the-future-of-angular-bb4deb2cfdad) [](/building-angular-micro-frontend-with-ngrx-state-sharing-and-nx-cli-7e9af10ebd03) [## 🔥利用 NgRx 状态共享和 NX cli 构建角度微前端

### 如何在几乎不编码的情况下构建健壮的微前端架构；)

itnext.io](/building-angular-micro-frontend-with-ngrx-state-sharing-and-nx-cli-7e9af10ebd03)