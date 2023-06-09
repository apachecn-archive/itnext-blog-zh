# 让我们写一个 Brainfuck 编译器(用 JavaScript)

> 原文：<https://itnext.io/lets-write-a-brainfuck-compiler-aaa7cfd449b0?source=collection_archive---------0----------------------->

为只有八个命令的编程语言编写一个编译器。会很有趣的，我保证！

![](img/3a85eb3de64931b9d05526bbf2d3c6f7.png)

首先:什么是“脑残”？！嗯，Brainfuck 是一种深奥的编程语言，仅由 8 个命令组成:`> < + - . , [ ]`。

等等，一门编程语言怎么可能只有八个字符？嗯，计算机只使用了`0`和`1`，所以我们还有另外 6 个字符可以使用！

Brainfuck 是一种图灵完全编程语言，所以你真的可以用它做大量的事情！但是它是如何工作的呢？

Brainfuck 对一组存储单元进行操作，其中每个单元从零开始。你可以把它想象成一盘干净的无限大的磁带。第一个单元格上有一个指针，它只能执行八个特定的命令:

*   `**>**`将指针移动到下一个单元格。
*   `**<**` 将指针移动到上一个单元格。
*   `**+**` 递增当前存储单元。
*   `**-**` 递减当前存储单元。
*   `.` 将结果单元格发送到标准输出。
*   `**,**`存储单个字符输入到单元格中。
*   `**[**` 如果单元格值为`0`，则跳过匹配的`]`
*   `**]**` 如果单元格值不是`0`，跳回匹配的`[`

哇，太棒了，一个聪明的程序看起来怎么样？

```
++++++++[>++++[>++>+++>+++>+<<<<-]>+>+>->>+[<]<-]>>.>---.+++++++..+++.>>.<-.<.+++.------.--------.>>+.>++.
```

试着猜猜它的意思！这是 Brainfuck 里的一个“Hello World”！

# 理解脑残

假设我们想要将“`a`”打印到标准输出。知道了"`a`"的 ASCII 字符是`97`，我们可以将 97 个`+`命令放入一个`.bf`文件，然后编译它:

这将完美地工作；它会在我们的屏幕上打印出一个漂亮的“`a`”。但是如果用一个循环来实现就太好了！所以让我们写下来:

什么？的。他妈的是。正在发生。给你。让我们来分析通量:

没那么难！我们如何用 JavaScript 实现这一点呢？

# 编写编译器

编译器本身很简单。让我们从定义我们的`compile`函数开始:

首先，我们创建一个包含 10 个`0`的数组。最初的 Brainfuck 实现有 30000 个内存单元，但目前我们可以使用 10 个单元；我们以后再增加。

然后我们声明一个指针`ptr`，它代表我们正在工作的单元格。我们之前说过，从第一个小区开始(小区`0`)。

现在，对于程序中的每个字符，我们将增加/减少或移动磁带上的指针。

让我们通过运行以下命令对此进行测试:

```
console.log(compile(">>++"));// => [0, 0, 2, 0, 0, 0, 0, 0, 0, 0, 0]
```

太好了，成功了！现在让我们实现另外两个命令:

现在，您可以通过运行以下命令来测试编译器:

```
console.log(compile(">>,."));
```

它将在您的浏览器上打开一个提示窗口，并将其 ASCII 值存储在第二个存储单元中。此时，它会将其值打印到控制台。例如，如果你在输入提示中输入“`c`”，你将在你的控制台上返回`c`。

现在是实现循环的时候了:

循环有点棘手。首先，我们必须声明三个新变量:

*   会告诉我们现在是否在一个愚蠢的循环中
*   `loopStack`，一个数组，将保持跟踪每个 Brainfuck 循环
*   `innerLoops`，保持活跃 Brainfuck 循环次数的计数器。

我们还必须重构我们的`for`循环:我们已经切换到一个经典的类似 C 的`for`循环，因为我们需要程序的当前索引。在测试命令本身之前，我们需要检测我们是否在 Brainfuck 循环中。如果我们在一个循环中，并且当前命令是`[`，让我们将另一个循环添加到我们的`innerLoops`计数器中。如果当前命令是`]`，让我们检查这是否是我们的最后一个嵌套循环:如果是，让我们退出循环，否则递减`innerLoops`计数器。太好了，现在让我们转到命令`switch/case`。如果当前命令是`[`并且当前存储单元等于零，我们开始一个循环。否则，将当前命令索引压入`loopStack`数组。如果命令是`]`并且当前存储单元不等于零，让我们将当前索引设置为等于我们的`loopStack`的倒数第二个元素。否则，移除`loopStack`中的最后一个元件。信不信由你，你只是写了一个完整的 Brainfuck 编译器！我们用一些程序来测试一下吧！

你好世界:

[![](img/e05f00ed2cddfd2907284cb397168c3d.png)](https://github.com/sponsors/micheleriva)