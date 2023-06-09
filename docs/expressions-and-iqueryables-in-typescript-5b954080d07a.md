# TypeScript 中的表达式和 IQueryables

> 原文：<https://itnext.io/expressions-and-iqueryables-in-typescript-5b954080d07a?source=collection_archive---------1----------------------->

![](img/e5ba5388d49804eb744d6c830d3c7b62.png)

照片由 [imgix](https://unsplash.com/@imgix?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

W 在现代(web)开发中，我们已经看到越来越多更好的方法将我们的应用程序与外部数据源连接起来。我们已经看到 REST 是如何占领世界的，GraphQL 成为一个主要的流行词，OData 被 SAP 和 Navision 这样的大公司采用。虽然查询数据的协议已经大规模增长，但我们还没有看到 JavaScript 方面的强劲增长。当然，有各种各样的库围绕一个协议包装了一些 stringbuilder，但是它们中的大多数并没有比过去十年中优秀的老式 SOAP 客户端生成器走得更远。

在这篇文章中，我将展示一种查询外部数据源的方式，这种方式接近于 LINQ 和 C#的 IQueryables。我们将使用表达式树来描述我们的查询，就像它们是一种真正的语言一样，并为各种数据提供者提供通用的连接器。所有这些将导致一个客户端，在那里我们像在 TypeScript 中编写普通谓词和 lambdas 一样编写查询。

# **抽象语法树**

我们客户机的主干将是查询的抽象语法树。在本文中，我将把自己限制在如下所示的过滤操作上，以免让读者陷入代码中(每个以前使用过表达式树的人都应该能够确认这些东西可能非常庞大)。

在本文的剩余部分，我将使用这个谓词作为例子

AST 存在于一个包含其他表达式的表达式中。对于我们的例子，我们可以将表达式类型定义为树中所有不同表达式的有区别的并集。其定义如下所示。在这里你可以看到我们有 3 个表达式，它们都在一个左表达式和一个右表达式上操作。这些被称为二进制表达式。我们还有一个用于访问谓词参数属性的表达式类型，即索引表达式。最后，我们有一个包含值的表达式。

AST 只是一个表达式，对于我们的例子&&

对于这些表达式，我们可以创建构造函数，这样我们可以更容易地创建表达式树，并避免嵌套对象声明的圣诞树:

使用这些构造函数，我们可以创建如下所示的示例表达式。正如你所看到的，当树变大时，很快就变得难以阅读，这与用常规编程语言编写表达式的方式相去甚远。为了实现我们针对外部系统编写代码的目标，就像我们只是编写普通的日常代码行一样，我们需要做更多的工作。

从下一章开始，我将把重点放在创建公共接口上，我们将根据它来编写我们的表达式。在这里，我们将看到如何创建一个真正的表达式生成器。

# 访问**属性**

如果我们仔细看看我们的示例表达式，我们会发现它从我们当前测试谓词所针对的项目中选择一个属性开始:`b => b.Id …`。如果这是普通的 JavaScript，`b`将是集合中传递给`filter`函数的实际对象。但是我们的实体存在于服务器上，所以我们不能把它们都取出来，然后一个一个地测试它们(我们可以，但是这会破坏构建它的整个目的)。所以我们需要一个类似于`Blog`的东西，但实际上是构建 AST 的节点来访问属性。我们想要实现的是`b => b.Id`返回实际上与`createIndexExpression(‘Id’)`相同。然后，我们可以使用该节点继续构建表达式，直到得到我们想要的谓词。

为此，我们可以使用 JavaScript 中的`Proxy`特性，它允许我们通过一种方法捕获与对象的所有交互。为此，我们需要创建一个带有`get`方法的处理程序，当我们访问我们假装的事物的属性时，将调用该方法。使用这个处理程序，我们可以创建所需类型的代理:

如果我们运行`fakeBlog.Id`，我们将为`id`字段获得一个带有‘Id’的索引表达式。有了这些，我们就可以通过访问项目的一个属性来创建表达式树了。下一步是将这个表达式包装在一个构建器中，这样我们就可以重新创建`b => b.Id.Equals …`。

# **表达式生成器**

构建器的任务是允许我们用一个新的表达式来构建当前的表达式。所有二进制表达式将把当前 AST 放在新 AST 的左边，并从它们的输入中为右边创建一个新表达式。注意，所有不同的节点只有一个构建器。这是因为我们没有基于当前类型创建不同实例所需的运行时类型信息。对于等号(`==`)运算符，表达式生成器可能如下所示:

如果我们之前制作的代理将表达式包装在这个构建器中，我们可以编写`b => b.Id.equals(1)`，它将返回一个以`==`为根节点的实际表达式树！这一概念可应用于我们希望支持的所有行动，并导致以下结果:

处理程序定义将被更新以使用构建器，如下所示:

现在，我们可以用一种方便的方式编写表达式，就像我们在日常编程语言中所习惯的那样。文章开头的示例表达式现在可以写成这样:

❤❤❤

# **类型安全表达式树**

在我们当前的实现中，我们允许创建像`b => b.Id.includes(‘foo’)`这样不可能的表达式。这个表达式并不代表一个有效的谓词，它完全是无意义的。如果我们不允许构造这样的表达式，构建器使用起来会方便得多。首先，我们为表达式可以计算的每种类型的值创建接口:`boolean`、`string`和`number`。(例如，`==`会产生一个布尔值，如果我们运行它，那么`==`的计算结果就是这个值)。之后，我们重写了`Fake`类型，以正确地将模型的属性映射到表示它们类型的构建器:

注意，操作符的返回类型也是类型特定的接口，以确保我们不能使用 equals 来比较 a `StringExprBuilder`和`BoolExprBuilder`。更新后的`Fake`类型如下:

# **IQueryable**

我们图书馆面向公众的界面将是`IQueryable<T>`。它将数据源包装在这个接口中，并公开构建查询的方法。在我们的例子中，这将限于`where`和`toArray`。它将是异步的，因为数据源可能是外部的，并将通过 HTTP 进行查询。`IQueryable`也是懒惰的，这意味着它将在内部存储查询，直到`toArray`被调用并被迫执行查询。

我们可以创建的这个接口的实现分为两大类:外部的和内存中的。外部数据源的实现是像 OData 和 GraphQL 这样的协议的实现。内存中的实现将是 JavaScript 中数组和其他数据集合的实现。我将向您展示两个示例:一个用于 OData，一个用于数组。

# **OData IQueryable**

对于 OData，我们需要将 AST 编译成查询的过滤操作。这是通过一个递归函数完成的，该函数将表达式树的每个节点转换为一个字符串:

有了它，我们可以为 OData 模型实现`IQueryable`接口。在 queryable 中，我们有一个数组，其中包含传递给`where`函数的所有表达式。当我们调用`toArray`时，我们用 and 操作符将它们组合成一个查询字符串。

使用它来运行我在本文开始时介绍的谓词看起来像这样:

# 内存中 IQueryable

对于`IQueryable`的内存版本，我们需要一点运行时间来运行 TypeScript 中的表达式。这也是一个递归函数，但这次我们返回表达式的结果，而不是编译后的字符串:

这样我们可以在`IQueryable`接口中包装一个数组。这里，我们还将保留我们在 OData 中引入的查询的惰性。

使用它看起来像这样，如下所示。请注意，通过我们强大的抽象，我们可以同时使用数组和 OData 端点，就好像它们是同一个东西一样。基于这两个例子，人们可以很容易地想象这对其他数据提供者是如何工作的。

# **结论**

在本文中，我展示了 TypeScript 中的一些概念和特性，可以用来利用查询语言中的最新发展。在这篇文章中，我把自己限制在`where`，但是同样的原则也适用于选择属性、扩展关系等。当然，还有很多数据提供者可以用于我没有提到的地方。

# 资源

该项目的完整源代码可以在 [Github](https://github.com/WimJongeneel/ts-iqueryable) 上找到。对于那些对扩展其他查询操作符的概念感兴趣的人，我可以推荐这篇文章:

[](/type-safe-client-side-queries-in-typescript-3906eb14250e) [## TypeScript 中的类型安全客户端查询

### 用于客户端编程的静态类型语言(如 TypeScript)的引入极大地促进了客户端编程的发展。

itnext.io](/type-safe-client-side-queries-in-typescript-3906eb14250e)