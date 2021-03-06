---
layout: post

title: "契约测试"
date: 2020-09-15
categories: [Agile]
tags: [AGILE, AGILE-TESTING]
column: AGILE-TESTING
sub-tag: "common"

author: "袁慎建"

brief: "
测试替身最常见的使用场景之一是与外部服务进行通信。这些服务通常由不同的团队在维护，它们可能会受到缓慢且不可靠的网络的影响，甚至自身可能也不可靠。这就是为什么测试替身很方便的原因，它可以让你的测试免受这些影响。但是，对测试替身进行测试始终会存在一个问题，即替身是否能准确地表示外部服务？如果外部服务更改了契约又会怎么样？
"

---

* content
{:toc}


[测试替身]({{site.url | append: '/test-double'}})（[TestDouble](https://martinfowler.com/bliki/TestDouble.html)）最常见的使用场景之一是与外部服务进行通信。这些服务通常由不同的团队在维护，它们可能会受到缓慢且不可靠的网络的影响，甚至自身可能也不可靠。这就是为什么测试替身很方便的原因，它可以让你的测试免受这些影响。但是，对测试替身进行测试始终会存在一个问题，即替身是否能准确地表示外部服务？如果外部服务更改了契约又会怎么样？

![](https://martinfowler.com/bliki/images/contractTest/sketch.png)

解决此问题的一种好方法是使用替身进行测试的同时，定期运行一套单独的契约测试，从而能够检查所有调用测试替身的地方，其返回的结果是否跟外部服务的调用返回的结果相同。如果这些契约测试中的任何一个失败，则意味着你需要更新测试替身，甚至可能因为服务契约的变更需要更新代码。

这些测试不必作为常规部署流水线（deployment pipeline）的一部分来运行。因为你的常规流水线通常基于代码更改的节奏工作，而这些测试是基于外部服务更改的节奏来的。通常每天只运行一次就够了。

跟常规测试不太一样的地方是，契约测试中的失败不一定会破坏构建。但是，它应该能够促使团队去做一些事情来让两边再次保持一致。团队可能需要更新测试和代码，让它们与外部服务保持一致。另外，团队很可能需要找外部服务维护者一起来讨论变更并告知他们变更对其他应用程序产生了什么样的影响。

如果将此服务用作生产环境应用程序的一部分，与外部服务提供商的沟通就显得尤为重要了。在这种情况下，契约的变更可能会中断生产环境中的应用程序，从而触发紧急修复并与供应商团队进行紧急沟通。

为了降低契约被意外破坏的概率，你可以采用“消费者驱动契约”的方式，它很管用。基于这种方式，你可以为服务供应商团队提供一份契约测试的副本，从而方便他们将其作为构建流水线的一部分来运行。

当测试这种外部服务时，通常最好针对外部服务的测试实例进行测试。有时候如果你只能使用生产环境的实例，你就需要与服务供应商交谈，让他们知道发生了什么，并要特别小心测试正在做的事情。


契约测试会检查对外部服务调用的契约，但不一定会检查确切的数据值。Stub通常会使用一个特定日期作为响应快照，因为它关心的是数据的格式而不是实际的数据值。在这种情况下，即便实际数据发生变更，契约测试任然需要检查日期格式是否相同。


[SelfInitializingFake](https://martinfowler.com/bliki/SelfInitializingFake.html)
是构造这些测试替身的最佳方法之一。


#### 声明
本文翻译自Martin Fowler的文章*ContractTest*：

- 原文链接： [ContractTest](https://martinfowler.com/bliki/ContractTest.html)
- 原文作者： [Martin Fowler](https://martinfowler.com/)
- 发表时间： 2011年1月12日
