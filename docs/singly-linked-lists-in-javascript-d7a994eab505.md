# JavaScript 中的单链表

> 原文：<https://itnext.io/singly-linked-lists-in-javascript-d7a994eab505?source=collection_archive---------1----------------------->

我接触过的大多数工程师都讨厌工程面试。一些人认为许多问题不实用或者*“我永远不会在工作中用到这个”*。

我的看法？我认为要成为一名伟大的工程师，你需要能够掌握你所使用的工具。给定一个行为描述(例如，一个规范)，我们应该能够使用我们的编程语言知识来构建软件。

所以事情是这样的，我们拿一个编程面试的问题，把它做得“实用”或者至少设置一个听起来很实用的场景哈哈。

有人记得那些旧的令人起鸡皮疙瘩的恐怖书吗？你会读一页纸，然后决定你想做什么:

![](img/452dddd799f3370d9846b2b498c8b314.png)

"你看到鳄鱼向你游来，你会怎么做？"

“你跑了，翻到 32 页”

如果你要建立一些软件来表达这个游戏本，你会有一个类似于“单链表”的东西。在计算机科学中，这是一种数据结构，它包含一系列“节点”(在我们的例子中是操作)，这些节点“链接”到另一个节点。

看起来这个数据结构有两个组成部分。我们有

*   鸡皮疙瘩书的节点列表
*   列表中的单个节点

## 节点

让我们实现节点。我们需要节点拥有哪些信息？

可能有一些我们想要存储的元数据，我们可以称之为“**数据**”，我们可能需要下一个节点是什么，让我们称之为“**链接**”。

```
class Node {
  constructor(metadata) {
    this.data = metadata;
    this.link = null;
  }
}
```

就是这样！很简单，对吧？

## 名单

好了，现在让我们来实现这个列表。当你思考这个列表时，它应该能做什么样的事情？

*   ***应该可以向列表中添加一个节点***
*   ***应该能够从列表中删除一个节点***
*   ***应该能理解列表的长度***
*   ***应该可以在*** 列表里面找到某个节点
*   ***应该可以看到列表开始的节点是什么***

有了这些标准，让我们来实现它:

我们开始了！我们有一个长度属性，我们将其初始化为零，我们还没有向列表中添加任何东西！出于同样的原因，firstNode 也为 null。

1.  ***应该可以给列表添加一个节点***

让我们走一遍。当你添加一个节点到这个列表中时，我们首先需要检查这个列表是否为空。我们可以通过检查列表长度或者是否有 firstNode 来检查这一点。如果列表为空，我们实例化一个新的节点，并将其添加到列表中。

如果列表是**而不是**空的，那么我们需要做更多的工作。我们要做的是在列表中找到一个没有链接到另一个节点的节点。这就是我们要将新节点作为链接附加到的节点。你可以用一个 while 循环来找到这个节点，一旦我们找到一个没有链接的节点，我们就从这个循环中脱离出来，给这个节点分配那个链接。

**2*。应该可以从列表中删除一个节点*T5**

让我们来看一下这个:

首先，我们验证我们想要删除的节点确实存在于列表中。

如果我们删除第一个节点，我们需要取消设置第一个节点，并减少列表长度。

如果我们要删除任何其他节点，我们需要从第一个节点开始遍历所有节点，并替换连接到要删除的节点的节点的链接。之后，我们递减列表并返回我们删除的节点。

***3。应该能理解*列表的长度**

到目前为止，我将为我们的方法添加一点封装。什么是封装？封装用于隐藏类中数据对象的状态。这样，我们将替换我们的增量/减量和对 or 长度值的检索。

***4。应该能够在列表*** 中找到特定的节点

最后要落实的事情！让我们编写一些代码来查找一个节点:

找到节点并不太难。首先我们验证我们的参数，然后遍历每个节点，直到我们找到节点并返回它。

现在就用吧！

吼吼！

免责声明，可能有更好更干净的方式来写这个！

老实说，在我写作的这一点上，我意识到我可能不会用链表来构建一个让人起鸡皮疙瘩的游戏，但是谁知道呢？