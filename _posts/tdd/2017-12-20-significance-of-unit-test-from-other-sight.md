---
layout: post

title: "从另一个角度告诉你单元测试的意义"
date: 2017-12-20
categories: [TDD]
tag: [TDD, Test, Unit Test]

author: "袁慎建"

brief: "
微服务架构带来的复杂性（右边部分）已经很大程度上得到了解决，常见的解决方案是在开发团队中植入DEVOPS。比如在ThoughtWorks中的某些团队，DEVOPS成为Team不可或缺的成分。

<br/><br/>

我们把注意力放在左边部分。开发人员关注更多的是`开发`，每个服务由一个小的Team负责开发，Team正在极力往`服务自治`的方向靠拢。测试人员可能更加关注`测试`，尤其是契约测试伴随着业界对集成测试（UI测试）的痛斥声而崛起。`消费者驱动契约测试`的演讲比比皆是，我也没有例外，在某Account上做了一次 *微服务架构下的测试应对策略* 的分享。在分享中，我赶时髦提倡用契约测试取代集成测试，但是细节中没有忽略的一个核心点：*单元测试*。这也是本文我要分享的重点。
"

---

* content
{:toc}

---

当下微服务如火如荼，各个团队在争先恐后推出微服务，不论在概念上还是在实践上，如果自己没有跟微服务挂上钩，便会被贴上`落伍`的标签。我们在推微服务的时候，我们说微服务架构具备如下优势：

- 架构灵活，能够应对复杂的业务需求。
- 独立部署，大大提高CI/CD的效率。
- 服务自治，支持技术栈多元化。
- ......


这些特征恰恰是单点应用无法具备的，因此微服务架构在广大的呼声下逐渐承接了单点应用的替代工作。随着容器技术的成熟，使用Docker重建一个应用的成本趋近于零。而K8S/Rancher在生产上的广泛应用，很大程度解决了容器管理的难题。调用链分析工具（ZipKin）、ELK+Kibana再配合系统监控工具（Prometheus），就连微服务架构带来的部署运维的复杂性也得到了大大的改善。更加乐观的是，众多云平台（AWS, GCP, Azure等）正在试图打造解决部署运维难题的一体化Paas服务，让应用开发商更加专注于业务上。

如果将软件生命周期大致划分成两部分：

![]({{ site.url }}{{ site.img_path }}{{ '/tdd/lifecycle-of-sofeware.jpg' }})

我们认为左边部分正在享受着微服务架构的益处，而右边部分在遭受着微服务架构复杂性的折磨。

微服务架构带来的复杂性（右边部分）已经很大程度上得到了解决，常见的解决方案是在开发团队中植入DEVOPS。比如在ThoughtWorks中的某些团队，DEVOPS成为Team不可或缺的成分。

我们把注意力放在左边部分。开发人员关注更多的是`开发`，每个服务由一个小的Team负责开发，Team正在极力往`服务自治`的方向靠拢。测试人员可能更加关注`测试`，尤其是契约测试伴随着业界对集成测试（UI测试）的痛斥声而崛起。`消费者驱动契约测试`的演讲比比皆是，我也没有例外，做了一次 [*微服务架构下的测试应对策略*]({{ site.url }}{{ '/test-strategy-to-meet-microservice-architecture/' }}) 的分享。在分享中，我赶时髦提倡用契约测试取代集成测试，但是细节中没有忽略的一个核心点：*单元测试*。这也是本文我要分享的重点。

---

## 基本最无敌

>单元测试是根，是基本，基本最无敌

单元测试存在于测试金字塔的底端，撑起了整个金字塔，编写它是开发人员的职责。微服务架构让服务更加独立小巧，*这意味着我们不用为小巧的代码库编写单元测试了吗？*微服务架构提倡服务与服务之间通过契约测试来集成，*这意味着我们只用编写契约测试就足够了吗？*

*假设我们以正确的姿势在践行微服务的相关技术实践。*

CI上会伴随每次提交都触发单元测试、Service测试（API测试）、契约测试，所有测试通过后开始独立部署，如果我们的契约测试写的足够好，便可以自信地独立部署。如果Service测试覆盖的足够全，便可以自信地说代码缺陷率很低。此时我们可能认为单元测试业务价值低，不必过多关注。

*回到现实，实际情况可能是这样子的。*

CI上有契约测试的Stage，但也是草率编写，甚至契约测试因为没人维护而被默认忽略。Service测试写了大量的Case，导致测试运行时间被拖长，Build效率大大降低。由于大量的Servcie测试的存在导致单元测试被过度轻视，再加上无效的测试充斥着代码库。这几点不但扼杀了服务*独立部署* 的特性，而且增加了开发部署的工作量。

再者，根据康威定律：`系统架构取决于组织架构，它俩息息相关`。不少团队怀揣微服务架构的梦想，却在老一套组织架构的驱使下渐行渐远。最后迫不得已，将原来一个大Team按照功能模块拆成几个小Team，将代码库粗暴地拆分成多个，每个开发人员同时往所有的代码库中提交代码。

微服务架构的优势会驱使团队在一开始就高架微服务，无视业务需求复杂度，走一遍Event Storming，来一场DDD活动，确定几个服务便开始搞下去。而微服务架构提倡的是演进式架构，某些团队却因为各种原因一直停留在起初确定的那几个服务下开发下去，拆分的不合理性和演进性没有得到体现。这足以扼杀微服务架构*能够应对复杂业务需求* 的特性。


微服务架构本身并没有错。归根结底是业务的复杂性很难被驾驭。我们说DDD可以帮助做微服务设计，于是我们都来学习*Eric Evans* 的DDD，可它却不能有效解决以下几个问题：

- 如何进行领域建模？
- 如何划分限界上下文？
- 如何在实现层面定义对象？


所以，我们学习了DDD还是不会DDD。但有一点毋庸置疑，我们每个人（DEV）都会编写单元测试。我们在试图驾驭微服务架构的路上摒弃了陈旧的集成测试、掌握了新的契约测试，而任何时候我们都应该始终抓住根本：***编写有效的单元测试来为我们的系统保驾护航。***

---

## 三个维度看单元测试
我们不会说单元测试是灵丹妙药，对于100%覆盖率我们也应该持有保留态度。但在一个微服务架构基础设施还不完善、开发人员能力参差不齐、DDD能力不足以应对复杂业务的情况下，单元测试是性价比最高的实践。

### 能力建设
一个具备开发经验的开发人员，基本上都会编写单元测试。即便不会，可以通过培训来快速达成。从学习曲线上看，单元测试很容易上手（方法难以被测试另当别论），拥抱Java老大的JUnit就是一个很好的例子。所以在一个团队中，我们可以过培训、[*Pair*]({{ site.url }}{{ '/first-impressive-agile-experience-in-thoughtworks/#pair' }}) 快速让开发人员具备编写单元测试能力。

测试即文档，对于新上项目的开发人员，可以通过阅读单元测试来了解业务需求，并且不会对一系列具备复杂数据安装的Service测试产生恐惧感。


### 生产效率
在那些重Service测试而轻单元测试的项目中，Service测试里的数据安装缺少易用的脚手架，实际上编写出来的诸多Service测试犹如行尸走肉，不但没有测试出缺陷，还降低了测试运行速度，拉长了反馈时间。

实践证明，很多缺陷完全可以通过单元测试来发现，测试金字塔提出者*Martin Fowler* 强调 *如果一个高层测试失败了，不仅仅表明功能代码中存在bug，还意味着单元测试的欠缺。因此，无论何时修复失败的端到端测试，都应该同时添加相应的单元测试。* 而越早发现发现Bug，造成的浪费就会越小，单元测试本身就能够提供了快速反馈的机制。


### 卓越态度
追求卓越是一个优秀程序员必备的态度。优秀的程序员除了能够编写Clean Code，还应该能够编写Clean Test。而Clean Code的基本特征之一是易于测试。单元测试可以充当一个设计工具，它有助于开发人员去思考代码结构的设计，让代码更加有利于测试。知名的开源代码库从来不会缺乏单元测试，而给与他们自信的也正是这些可观的单元测试覆盖率。

考虑到成本与收益比，我们不必保证100%的覆盖率。因为随着覆盖率提升，单元的测试的价值越来越低，而编写的成本却越来越高。所以相比于100%这个漂亮的数字，我们应该去追求那不到100%的单元测试的有效性。

---

## 夯实根基
>单元测试能为代码库保驾护航的前提是它本身应该有效可靠。

编写单元测试的能力容易培养，但编写有效的单元测试却需要不断地刻意练习，甚至一个有多年经验的Senior开发人员也不一定能够时刻编写出有效的单元测试。

让单元测试有效的一个很好的方式是尽可能让我们的被测代码具备良好的可测性。要做到这点，我们需要尽可能的在编码的过程中掌握必要的代码设计原则。就拿面向对象的编程编程语言来讲，我们在编写代码的时候要时刻思考*Robert C. Martin* 提出的`SOLID`原则：

- SRP(Single Responsibility Principle)，单一职责原则
- OCP(Open Closed Principle)，开放封闭原则
- LSP(Liskov Substitution Principle)，里氏替换原则
- ISP(Interface Segregation Principle)，接口分离原则
- DIP(Dependency Inversion Principle)，依赖倒置原则

同时我们应该尽量避免编写`STUPID`代码：

- Sington，单例
- Tight Coupling，紧耦合
- Untestability，不可测
- Premature Optimization，过早优化
- Indescriptive Naming，胡乱命名
- Duplication，重复代码

在做设计和编写代码的时候多思考我们是不是在践行`GRASP`原则：

- Controller，控制器
- Creator，创造者
- High cohesion，高内聚
- Low coupling，低耦合
- Polymorphism，多态
- Indirection，中介
- Information expert，信息专家
- Protected Variations，受保护变化
- Pure fabrication，纯虚构

以上这些原则需要在编码中不断地刻意练习，除了阅读针对性的书籍，在团队中积极组织 [*Code Review*]({{ site.url }}{{ '/first-impressive-agile-experience-in-thoughtworks/#code-review' }})、推动 [*Pair*]({{ site.url }}{{ '/first-impressive-agile-experience-in-thoughtworks/#pair' }}) 来互相学习和改进是一个更有效的方式。

良好的代码设计让我们的单元测试更加容易编写，而要编写有效的单元测试，我们应该对以下几个维度的测试坏味道保持敏锐的嗅觉：

- 可读性：*基本断言*、*附加细节*、*冗长安装*、*逻辑分隔*、*魔法数字*、*过度断言* 等。
- 可维护性：*重复*、*条件逻辑*、*参数化混乱*、*残缺路径*、*永久性临时文件*、*弱不禁风* 等。
- 可靠性：*被注释*、*歧义注释*、*永不失败*、*轻率承诺*、*降低期望*、*有条件的测试* 等。

---

## 写在最后
我们在试图驾驭微服务架构的路上，应该时刻守住根本，让单元测试这项成本低、收益高的实践为我们高层测试打好地基。*如何设计良好可测的代码* 以及 *如何编写有效的单元测试* 更是值得每一位追求卓越的程序员去深入学习和实践。

如果你还在思考*为什要写单元测试？*推荐阅读我的文章 [*一枚程序员眼中的单元测试*]({{ site.url }}{{ '/unit-test-view-from-a-programmer/' }})。

>***程序员福利***
>
>JUnit测试框架已经升级到JUnit 5，官方用户指南已被译成中文，欢迎你以 [*JUnit 5 中文用户指南*]({{ site.url }}{{ '/junit5/user-guide-cn' }}) 作为学习JUnit 5的起点。




