---
layout: post

title: "集成测试"
date: 2020-09-20
categories: [Agile]
tags: [AGILE-TEST]
column: AGILE-TEST
sub-tag: "common"

author: "袁慎建"

brief: "
集成测试用来确定那些独立开发的软件单元相互连接时是否正常工作。即使受到软件行业普遍标准的影响，集成测试这个术语也已经变得模糊不清。因此在我的文章中，我会谨慎地使用它。特别是，很多人认为集成测试的测试范围一定很宽泛，然而其实集成测试也可以在更小的范围内完成。
"

---

* content
{:toc}



集成测试能够验证那些独立开发的软件单元相互连接时是否正常工作。即使受到软件行业普遍标准的影响，集成测试这个术语也已经变得模糊不清。因此在我的文章中，我会谨慎地使用它。特别是，很多人认为集成测试的测试范围一定很宽泛，然而其实集成测试也可以在更小的范围内完成。

在详细谈论这个话题之前，我们先来了解一些历史。我第一次了解集成测试是在1980年代，那时候软件开发主流思想还是瀑布模型。在当时，一个大项目会有独立的设计阶段。在该阶段，设计人员设计出系统中各个模块的接口和行为，然后把这些模块分配给开发人员进行开发。一个程序员负责开发一个模块的现象也很常见，只是由于模块太大了，他们可能要花费数月的时间来开发。开发人员各自独立完成这些工作后就交给QA进行测试。


单元测试是第一部分测试，开发人员按照设计阶段制定的规范，使用单元测试测试自己开发的模块。完成单元测试后再做集成测试，在集成测试时，他们会将各个模块集成到整个系统或者一些重要的子系统中。


集成测试，顾名思义，目的是 **测试那些单独开发的模块集成到一起后是否可以按预期工作**。它是通过启动多个模块并针对这些模块运行更高级别的测试来确保它们能够很好地一起工作。这些模块可以是单个可执行文件的一部分，也可以是单独的一部分。

从2010年代的观点来看，当时有两件不同的事情被混为一谈了：

- 测试单独开发的模块是否可以一起正常工作。
- 测试由多个模块组成的系统是否按预期工作。

它们很容易被搞混淆，毕竟如果你不将frobile模块和twibbler模块集成到单个环境中，并运行同时能够覆盖这两个模块的测试，你是没法测试他们能否在一起正常工作的。


2010年代的观点提出了一个替代方案，这个在1980年代很少有人去考虑。 在新的替代方案中，我们只测试frobile模块中与twibbler模块有交互的那一小部分代码，并使用测试替身（[TestDouble](https://martinfowler.com/bliki/TestDouble.html)）替换掉twibbler模块。按照这种方式来测试frobile和twibbler两个模块的集成，只要twibbler模块的测试替身是值得信赖的，那么我们就可以在不启动完整的twibbler实例的前提下测试所有跟twibbler模块的交互行为。 如果它俩只是单个应用程序中的两个独立模块，处理起来还不算复杂。但是如果twibbler是一项单独的服务，就要复杂得多。twibbler服务有自己的构建工具、环境和网络连接。针对单独的服务，我们可以使用[mountebank](http://www.mbtest.org/)这类工具来提供测试替身，此时测试替身可以是进程内的，也可以是进程间跨网络的。


使用测试替身进行集成测试的方式存在一个核心问题是 -- 测试替身是否值得信赖。为了更好地保证这一点，我们可以借助[契约测试（Contract Test）](https://martinfowler.com/bliki/ContractTest.html)分别对测试替身和被测试目标进行测试。


结合使用窄集成测试（narrow integration tests）和契约测试（contract tests），我可以自信地与外部服务集成，而无需单独运行基于外部服务真实实例的测试，这就大大简化了我的构建过程。即便这样做了，团队可能还会对所有真实服务进行某种形式的端到端系统测试。但如果是这样，那写测试只是最终的冒烟测试，它要测试路径范围少很多。它还有助于提升[生产质量保证](https://martinfowler.com/articles/qa-in-production.html)方面的能力。如果该能力足够成熟，我们可能根本就不需要端到端的系统测试。

![](https://martinfowler.com/bliki/images/integrationTesting/sketch.png)


问题在于，我们对集成测试的理解，至少存在有两个不同概念。

窄集成测试（narrow integration tests）

- 仅测试与外部服务存在交互的那部分代码
- 使用进程内或进程间（远程）的测试替身代替外部服务
- 包含许多小范围的测试，它的范围通常不比单元测试大（并且通常使用与单元测试相同的测试框架）


宽集成测试（broad integration tests）

- 需要实时运行所有服务，同时需要大量的测试环境和网络访问权限
- 测试要覆盖所有服务的代码路径，而不仅仅是存在交互的代码片段

大多数的软件开发人员都将“集成测试”理解为“宽集成测试”，一旦他们碰到使用窄集成测试的同仁时，难免会心生困惑。

如果你只编写过宽集成测试，建议你也了解一下窄集成测试风格。它可能会显着提高你的测试速度
、易用性和弹性。由于窄集成测试的范围较小，所以运行速度会更快。因此，你可以在[DeploymentPipeline](https://martinfowler.com/bliki/DeploymentPipeline.html)的早期阶段运行，一旦出现问题就能提供更快、更早的反馈。


这就是为什么我对“集成测试”保持警惕的原因。当我在某篇文章看到这个概念时，我会进一步寻找更多的上下文，因此我通常清楚作者的要表达的真正含义。 [注1]如果我要谈论宽集成测试，我更喜欢使用“系统测试”或“端到端测试”。 对于窄集成测试，我还没有找到更好的名字，不过我确实在使用它（但是我用“窄”来帮助读者理解这些测试的特点）。



#### 注释
1. 注1：尽管我更喜欢将定义聚焦在单独构建的模块的交互上，但我确实偶尔会看到“集成测试”用来表示比单元测试更大的内容。 对于一些孤立型（solitary）单元测试的实践者，我已经发现他们会把社交型（sociable）单元测试描述为“集成测试”。


#### 声明
本文翻译自Martin Fowler的文章UnitTest：

- 原文链接： [IntegrationTest](https://martinfowler.com/bliki/IntegrationTest.html)
- 原文作者： [Martin Fowler](https://martinfowler.com/)
- 发表时间： 2018年1月16日