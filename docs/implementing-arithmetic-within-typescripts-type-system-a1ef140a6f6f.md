# 在 TypeScript 的类型系统中实现算法

> 原文：<https://itnext.io/implementing-arithmetic-within-typescripts-type-system-a1ef140a6f6f?source=collection_archive---------1----------------------->

![](img/0c618560b9b0490c024d6ad1e58ba9af.png)

照片由[米卡·鲍梅斯特](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我在工作中经常使用 TypeScript，并且我喜欢创建精细优雅的类型来准确地模拟我正在解决的问题的约束。我发现只要有足够的独创性(和探索)，我就能实现我所需要的。然而，有时我会遇到这样的情况，我需要的类型超出了 TypeScript 当前的能力，我发现自己被迫编写类型断言，如果 TypeScript 更具表达性，我会觉得这是不必要的。

偶尔 TypeScript 会发布一个版本，增加额外的功能来克服我必须明确修补的一些限制。整个 4.x 发布系列就是一个这样的例子，它使以前不可能的构建类型的方法成为可能。为了纪念最近发布的 4.1 版本，我想介绍一些最新的特性，并通过展示我们如何使用它们来创建一个算术系统，来展示我们现在拥有的能力。

显然，如果我们给自己提供了 TypeScript 的全部功能，那么在 TypeScript 中做算术是微不足道的，所以我将把我们的解决方案限制在我们完全可以在类型系统中做的事情上。这意味着我们没有能力使用像`+`和`-`这样的简单操作符，我们构建的任何东西都必须使用 TypeScript 的方法来组合和操作类型。换句话说，我们将把自己限制在我们能对`type SomeType = ...`宣言做些什么。

我们的目标是拥有像`Add`这样的类型，它可以接受两个数字文字类型并确定它们的和。所以我们希望`Add<3, 5>`为我们生成文字`8`。

让我们看看建立我们的算术系统需要哪些重要的特征。

# 初始基础

为了构建基于类型的算法，我们将在很大程度上依赖于 TypeScript 的一些老的熟悉特性:

*   条件类型
*   类型推理
*   通用约束

我们需要在此列表中添加在 4.x 发布周期中添加的某些功能:

*   条件类型中的递归
*   可变元组
*   模板文字类型

虽然对这些特性的深入讨论超出了本文的范围，但是在我们说明它们在我们的系统中的使用时，有必要进行一个简短的讨论，以确保一些共同点。

一个**条件类型**仅仅是形式`SomeType extends AnotherType ? TrueType : FalseType`的某种东西。这是一种向类型添加 if/else 结构的方法，这样我们就可以根据`extends`条件的求值方式在两个不同的类型之间切换。

**类型推理**是一个在条件类型表达式中使用的强大内省工具。它的主要目标是从一个复杂类型中提取出组成它的其他类型。一个人为的例子是从数组中提取类型:`type Flatten<T> = T extends (infer U)[] ? U : T`。然后我们可以调用`Flatten<[number, string, object]>`来获取数组元素的底层类型(`number | string | object`)。

泛型约束给了我们一种创建泛型类型的方法，同时指定泛型期望用什么类型来实例化。因此，`type Flatten<T>`允许`T`成为任何可能的类型，而`type Flatten<T extends any[]>`要求`T`是可分配给数组的类型。

**递归条件类型**允许条件类型的一个分支引用自身，并通过条件类型的逻辑递归任意次。但是，像所有递归一样，它必须停止，所以一个分支需要进行终止检查。在 4.1 版本之前，尝试递归类型会给我们带来错误`Type alias 'SomeType' circularly references itself`。

一个**可变元组**赋予我们在另一个元组中自由使用扩展操作符的能力，以便创建一些新的元组类型，直接对应于其组件的级联类型。因此我们可以获取一个元组`type Booleans = [boolean, boolean]`并派生出一个新的元组`type Derived = [...Booleans, string]`，其类型为`[boolean, boolean, string]`。

最后，**模板文字类型**使用模板文字语法从其他字符串文字类型形成字符串文字类型。我们可以采用类似`type ClickEvent = 'click'`的类型，并派生出它的事件处理程序类型`type ClickEventHandler = `on${ClickEvent}`` ( `'onclick'`)。

# 快速概述我们的方法

在高层次上，我们操作数字文字类型的目标(正如我们前面所说的，我们不能使用典型的算术运算符)直接取决于我们将它们与一些更容易操作的类型相关联的能力。所以我们的基本方法是:

1.  将每个数字文字映射到该长度的元组
2.  使用新发现的可变功能操纵元组
3.  将结果元组映射回它的数字文本对应物

假设我们的算法的基础是元组，我们自然会被限制在对自然数实现算法。元组没有小数或负长度的概念，因此我们同样没有能力处理这些类型的数字。这种无意义的输入与我们无关，因为我们将看到如何保证我们的系统只对非负整数进行操作。

我们将使用第 1-3 点的机制来实现最基本的算术运算(尽管我们*可以*实现更多的运算):

*   添加
*   减法
*   增加
*   分开
*   以…为模

# 构建系统

## 基本公用事业

我们完成目标所需的基本类型是满足我们概述中列出的第 1 点和第 3 点的类型。尽管它们对我们的项目至关重要，但它们是相当简单的类型。我们将从构建点 3 的类型开始。

```
type Length<T extends any[]> = 
    T extends { length: infer L } ? L : never;
```

为了将一个元组映射回一个数字，我们利用了这样一个事实，即它们可赋给数组，具有一个`length`属性，因此我们将它`infer`出来。

第 1 点的类型(将一个数字映射到一个元组)稍微复杂一些。

```
type BuildTuple<L extends number, T extends any[] = []> = 
    T extends { length: L } ? T : BuildTuple<L, [...T, any]>;
```

让我们稍微解释一下这种类型。`BuildTuple`接受一个数字文字类型`L`，它应该是最后一个元组的长度，以及一个类似数组的类型`T`，它将最终成为那个元组。我们首先检查我们的`T`元组是否具有期望的长度`L`，如果是，则返回它。如果没有，我们向 tuple 添加一个元素，并递归到另一个迭代`BuildTuple`(不过这次是用增加的 tuple)。以这种方式，我们不断增加元组的大小，直到它达到期望的长度。

在行动中，我们的类型给我们以下结果:

```
let length: Length<[number, string, string, boolean]>; // `4`
let tuple: BuildTuple<5>; // `[any, any, any, any, any]`
```

## 基本算术

有了这些基本类型，我们现在可以构建我们的第一个算术类型:`Add`和`Subtract`。

```
type Add<A extends number, B extends number> = 
    Length<[...BuildTuple<A>, ...BuildTuple<B>]>;type Subtract<A extends number, B extends number> = 
    BuildTuple<A> extends [...(infer U), ...BuildTuple<B>]
        ? Length<U>
        : never;
```

`Add`是一个简单的类型，它构建两个元组，每个元组是其中一个输入的长度，组合这些元组，并确定结果元组的长度。

`Subtract`对表达式`A — B`建模(因此`A`需要大于`B`以使结果为自然数)，并通过构建一个长度为`A`的大型元组并将其分配给一个由长度为`B`的元组和一些余数元组(我们的`infer U`)构建的可变元组来执行其工作。这个`U`元组弥补了`A`和`B`的值之差，所以我们返回它的长度。

在代码中使用这些，我们得到以下结果:

```
let five: Add<3, 2>; // `5`
let one: Subtract<3, 2>; // `1`
```

## 更复杂的工具

为了构建`Multiply`和`Divide`的类型，我们需要一些比我们现有的更强大的实用程序类型。由于乘法实际上只是重复的加法，我们将需要一个能够累加我们的`Add`类型的多次迭代结果的类型。

```
type MultiAdd<
    N extends number, A extends number, I extends number
> = I extends 0 ? A : MultiAdd<N, Add<N, A>, Subtract<I, 1>>;
```

这种类型的输入表示我们想要重复相加的数字`N`,存储每个`Add`操作结果的数字`A`,以及跟踪还剩下多少次迭代的数字`I`。

例如，执行`8 * 3`将意味着`8`被加上自身的`3`倍，我们应该将这两个数分别赋给`N`和`I`。`A`是蓄能器，应该从`0`开始。在每次迭代中，我们从`I`的当前值中减去`1`，并将`N`加到`A`的当前值中。最终，`I`将向下工作到`0`，此时存储在`A`中的值是我们两个数的乘积。

(欧几里得)除法同样是减法的重复使用，所以我们可以构建一个类似的实用程序来处理它。然而，因为我们需要在`A`小于`B`的任何时候终止我们的重复减法例程，所以它的实现需要额外的实用程序。

```
type EQ<A, B> =
    A extends B
        ? (B extends A ? true : false)
        : false;type AtTerminus<A extends number, B extends number> = 
    A extends 0
        ? true
        : (B extends 0 ? true : false);type LT<A extends number, B extends number> = 
    AtTerminus<A, B> extends true
        ? EQ<A, B> extends true
            ? false
            : (A extends 0 ? true : false)
        : LT<Subtract<A, 1>, Subtract<B, 1>>;type MultiSub<
    N extends number, D extends number, Q extends number
> = LT<N, D> extends true
    ? Q
    : MultiSub<Subtract<N, D>, D, Add<Q, 1>>;
```

让我们分析一下我们在这里做什么。

简单地检查两种类型是否可以互相分配。如果是这样，它们一定是同一类型，所以我们断定它们是相等的。这意味着`EQ<1, 3>`是`false`，而`EQ<3, 3>`是`true`。

`AtTerminus`检查`A`或`B`是否为`0`，如果是则指示`true`。这是为了以后，当我们从`A`和`B`中减去时，我们确保我们不会迭代超过`0`，从而使两个中的一个为负(因为这些也不是为我们的目的而定义的)。

`LT`是一个小于操作。它检查我们是否已经到达终点(意味着`A`或`B`或两者都是`0`),如果没有，重复地从每个输入中减去`1`。如果我们已经到达终点，那么它检查`A`和`B`是否相等。他们相等的意思是`A`是*不是*小于`B`(由于`4`不小于`4`)，所以我们返回`false`。但是如果它们不相等，那么只有一个命中了`0`，所以如果`A`命中了，我们就返回`true`。

最后，`MultiSub`取一个数`N`、一个除数`D`和一个商`Q`，该商表示在每次迭代中从`N`中减去了多少次`D`。一旦`N`变得小于`D`，我们的减法就停止，我们返回`Q`我们递归的次数。

例如，做`8 / 3`意味着`N`和`D`应该分别被分配`8`和`3`。`Q`将从`0`开始(因为我们在`MultiSub`的第一次迭代中没有减去任何东西)，并且每次我们从`8`中减去`3`，我们就增加`Q`，直到`LT`检查为`true`。此时`Q`代表我们除法的商。

## 更复杂的算术类型

我们现在可以实现乘法和除法类型了。因为大部分工作是由实用程序完成的，所以这些工作相当简单。

```
type Multiply<A extends number, B extends number> = 
    MultiAdd<A, 0, B>;type Divide<A extends number, B extends number> = 
    MultiSub<A, B, 0>;type Modulo<A extends number, B extends number> = 
    LT<A, B> extends true ? A : Modulo<Subtract<A, B>, B>;
```

注意`Modulo`不需要任何工具，因为我们只需要知道在我们重复的减法练习之后剩下了什么，而不需要知道我们通过递归调用迭代了多少次。

使用这些类型给出了以下示例:

```
let twentyEight: Multiply<7, 4>; // `28`
let one: Divide<7, 4>; // `1`
let three: Modulo<7, 4>; // `3`
```

## 确保输入是自然数

到目前为止，我们的泛型类型约束只要求输入是一个数字，而不是一个自然的数字。这意味着，如果我们试图使用自然数以外的东西进行任何算术运算，我们都会得到一个编译错误。

```
let one: Add<3, -2>;
let onePointSeven: Subtract<3, 1.3>;
```

这些声明中的任何一个都会出错并告诉我们`Type instantiation is excessively deep and possibly infinite`。考虑到我们上面所说的元组的长度既是离散的又是正的，这是有意义的，因此这些无效数字类存在于由这两个属性产生的“间隙”中。利用上面的无效数字，`-2`会一直在 `L`的起始值后面*，而`BuildTuple`的每一次迭代只是扩大了`-2`和`L`之间的差距。当构造一个长度为`1`的元组并转到长度为`2`的元组时，`1.3`总是会被忽略。在任一情况下，`BuildTuple`从未被告知终止，也从未被告知终止。*

现在，虽然我们可以让 TypeScript 编译器在我们提供无效数字时抛出一个错误，但这并不理想；我们应该明确地考虑这些情况，甚至不要试图在遇到它们时执行算术。

我们处理这个问题的方式是使用 TypeScript 的新模板文字类型。我们的目标是查看我们的输入是否与负数或小数模式匹配。所以让我们创建我们的类型来检查每一种情况。

```
type IsPositive<N extends number> = 
    `${N}` extends `-${number}` ? false : true;type IsWhole<N extends number> = 
    `${N}` extends `${number}.${number}` ? false : true;
```

这两个泛型类型都用某种类型`N`实例化(由于泛型约束保证它必须是一个数字，所以能够被插入到模板文字类型中)，然后将从`N`派生的模板文字类型与特定模式进行比较。

在第一种情况下，模式是以负号开始，后跟一个`number`的任何东西。不像上面我们的**初始基础**部分，我们从一个显式字符串文字`'click'`中派生出一个事件处理程序，这是更加通用的，将匹配*任何*负数。第二个模式确定`N`是否匹配任何两个有效的`numbers`，中间有一个小数位(`1.3`，因为`1`和`3`是有效数字)。

*说句题外话，我们的模板字面类型* `*-${number}*` *会根据需要匹配* `*-1*` *，但也会匹配* `*--1*` *。原因是* `*1*` *和* `*-1*` *都可以赋值给* `*number*` *，所以当我们在模板文本中使用* `*number*` *时，我们实际上是在*的每一个可能的数字*上加了一个减号，而不管最终的模板文本类型在我们的上下文中是否有意义。类似地，我们的十进制模式将匹配类似于* `*1.3.1*` *的东西，因为* `*1.3*` *和* `*1*` *都可以赋值给* `*number*` *，我们的模式在它们之间天真地添加了一个小数点。*

*由于我们的* `*N*` *类型被限制为* `*number*` *，不能有效地构造双重否定或多重小数，这些边缘情况在我们的系统中不会出现问题，我们可以放心地忘记它们的存在。这只是出于完整性的考虑。*

这些类型的简单例子给了我们

```
let negativeNumber: IsPositive<-13>; // `false`
let positiveNumber: IsPositive<5>; // `true`let float: IsWhole<1.3>; // `false`
let round: IsWhole<13>; // `true`
```

现在我们可以在这些基础上检查数字是否有效。

```
type IsValid<N extends number> = 
    IsPositive<N> extends true
        ? (IsWhole<N> extends true ? true : false)
        : false;type AreValid<A extends number, B extends number> = 
    IsValid<A> extends true
        ? (IsValid<B> extends true ? true : false)
        : false;
```

使用我们的`AreValid`类型，我们可以创建所有算术运算的安全版本，方法是将它们包装在一个条件类型中，如果我们的任何一个输入无效，这个条件类型就会失效。

```
type SafeAdd<A extends number, B extends number> = 
    AreValid<A, B> extends true ? Add<A, B> : never;type SafeSubtract<A extends number, B extends number> = 
    AreValid<A, B> extends true ? Subtract<A, B> : never;type SafeMultiply<A extends number, B extends number> = 
    AreValid<A, B> extends true ? Multiply<A, B> : never;type SafeDivide<A extends number, B extends number> = 
    AreValid<A, B> extends true ? Divide<A, B> : never;type SafeModulo<A extends number, B extends number> = 
    AreValid<A, B> extends true ? Modulo<A, B> : never;
```

这给了我们想要看到的结果:

```
let sum: Add<3, 4>; // `7`
let badSum: Add<3, 4.5>; // `never`
let anotherBadSum: Add<3, -4>; // `never`
```

# 我们系统的局限性

虽然现在有可能在 TypeScript 中实现自然数和算术，但它也有一些警告。

首先，递归条件类型确实有递归深度限制。这是一个明显的限制，因为如果没有它，实例化像`BuildTuple<-1>`这样的类型将会挂起编译器，因为我们将永远找不到扩展`{ length: -1 }`的元组，我们的递归将永远不会结束。这个限制对于我们自己的算术系统有一个真正的含义:我们不能直接创建一个长度大于`46`的元组(试图执行`BuildTuple<47>`会抛出我们前面看到的“过深”错误)。由于这个限制，我们的大多数类型都不能生成任何大的数。

其次，随着这些递归条件变得越来越复杂，它们的计算成本也会很高。太大太复杂的递归类型会降低编译器的速度。甚至 4.1 版本的[发行说明](https://devblogs.microsoft.com/typescript/announcing-typescript-4-1/#recursive-conditional-types)也说应该少用这些以避免性能下降。

最后，我们整个系统的一个小缺陷是关于`Subtract`。虽然它*不应该*能够接受一对差值为负的数字，但事实上它接受了，而且不是我们预期的结果。将`Subtract`实例化为`Subtract<1, 2>`给出的结果类型不是`-1`(我们知道无论如何都无法生成它)，而是`number`。不幸的是，像我们对`Safe*`类型所做的那样，防止这种情况的唯一方法是保证`A`总是小于`B`。但是由于我们的`LT`类型依赖于`Subtract`来确定这样的排序，我们最终陷入了困境，因为在`Subtract`中实现`LT`会给我们一个无限循环引用。所以我们不得不接受这样一个事实，我们不能完全防止`Subtract`的无效输入。

# 结论

我们展示了如何利用 TypeScript 的最新特性来构建自然数和算术系统。我们的实现仅仅触及了可能的表面，还有许多有趣的东西我们遗漏了(检查素性、相对素性、生成约数、*等等)。读者可以拿起它进行实验。*

我会留下一个链接，链接到[TypeScript playground](https://www.typescriptlang.org/play?noUncheckedIndexedAccess=true&useDefineForClassFields=true&noUnusedLocals=true&noUnusedParameters=true&esModuleInterop=false&declaration=false&experimentalDecorators=false&emitDecoratorMetadata=false&target=99&jsx=0&pretty=true&noErrorTruncation=true&isolatedModules=false&useUnknownInCatchVariables=true&exactOptionalPropertyTypes=true&noFallthroughCasesInSwitch=true&noImplicitOverride=true&noPropertyAccessFromIndexSignature=true&ts=4.4.4#code/PTAEFUBcEsBtsgT1EgDgUwM4Cg3tADLoB2A5pABYA8AKqOgB6QkAmmoAhsYgNoC6APlABeUHUbNibUAG9QsEuQoAuUNGIAzdACdCoAL6gA-HtXF0ANx0BubLkQZQAIQCucFjReoFVAvSas7MQuALYARjoANGL+ktJcvHwioPxCwtigmTESgbLyipSqfvoZWWUmNKVlmaqu7p7e6L7RPAB07TTRCYK29o4Asi6wMACCLCxUAHKxucHhUaAjM1JBoRHa0QCSy9Jz62lVoNs5K6AADIfli5dZqmMTk9EjQie7azo31cagg8PQ91MnuNAYsBNEAMouMKQbQcADGkCom2iAEYBAJPtUzJYbHY8ItIDQdCF1C5MFQlq9VvMNs4dtT9iJDpSAqcLl9MiYYS50JjVAAKJz087fbn4VQaDiwTDoACUvXxAFEAIoU6JOA5lFlxdhOTEmQXCpZc7Q80ASqUy2V80CS6XoBUOfAEGgU4V7BZCqmgD3aTVZEaE4mk8kjdUvVnSMX60AqtXOCM6lCm3kc6omO0yzFlAXa3JnUUp822y1ym0uqiQ6GwhHxtEQqEw+GIpyo9GOgZDGBVqbu960gAifZp0WVw8Z6TKFceoAHidy0bTJmVNoByrb44+aauv27UMrjZriJnc+iA6BE3XoDRGO3tx9OO0Hfwm0wAAUAPaYBDQKy972+v6mQAAYACQyJM+jAcKwEALTgb6UExpmqYcqoYrPkcmAAOoUB+PjTAB-ZAaAYEQVBMEIf2+itFRNJIUuJb2jaGF4k6WEAGpStADybn6TJlK+n7fjAf6TPOpyLhyBqvrh+FNOJwpioWZoWva1ppmpMqYSM2joFx8ATHmpy+uqfEka+Bk8RSElRimMb8pZ3ETBqSlFiaqlMVaNoob0ICLNoCAUCE6AwHCUooE6OD4gCxlvCOdJETSJFEGQlBUG07R1LAHheD4zzRO0rTZbljRUBqPRsY4VZNrWcUMp65kCVkJUNPltnsJlrT8uoWi6OAsqFVlbg5W1TQVTGqVKFQ4C3mhD5WE+VX4Lu0DeIgbpJesZlbToJGrbF0RnOGmEDr+PFNPVPr9jtkYNfxk5ZKtPZhs4R0CJh-QfiwQwfptd3XQlXoA4BzWZBWr2ud6UlfCYIw2l9P2wH9NVHvGGonXY-nghwWikC4HDaCw6AsJwgWUCFYURXg0XsTjWixXxt1JqDj2ZLp+nOejHXJjyMaHQmNrmItmH0+gqPNv9LM3YlIPEWDAWc4Z3NuXzjES3V4ZC4+ou4+gq3rVLswy8D0vJQrHNWUZ4aq6hsM-F2a2wBtkNzV82Ii8toBi2dFgXUbJkm01bOK1bKvQ-ZjG+-7rva57fT4GLiO-QH8XbbLZsTsyelh67tsxsnyPc3HNhAA)，那里有整个系统的布局，随时可以使用，以防有人想扩展我们已经建立的功能，以及有相同内容的 [Gist](https://gist.github.com/ryandabler/8b4ff4f36aed47bc09acc03174638468) 。