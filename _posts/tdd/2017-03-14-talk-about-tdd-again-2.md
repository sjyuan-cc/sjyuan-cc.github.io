---
layout: "post"
title: "让我们再聊聊TDD 续 – 人人都在做TDD"
date: 2017-03-14
categories: [eXtreme Programming]
tags: [TDD]
toXPSite: true
author: "刘冉"
origin: "https://insights.thoughtworks.cn/talk-about-tdd-again-2/"

---

* content
{:toc}


<!--brief-->
在[上一篇文章](http://insights.thoughtworkers.org/talk-about-tdd-again/)里面，通过对DHH的文章以及DHH和Kent Beck等讨论的分析，我阐述了对TDD的理解和分类，现在来继续聊聊TDD的实施和分层。

现在还有非常多的软件工程师在质疑TDD的可行性，比如太难不会、成本太高无法推动、意义不是很大等，但是他们却一直都在做着TDD，只不过没有意识到而已，这便是“不识庐山真面目，只缘身在此山中”。
<!--brief-->

![](http://insights.thoughtworkers.org/wp-content/uploads/2017/03/1-implementation-hierarchy-1024x715.jpg)

TDD的实施一般分为思维层面和技术层面。一般来说，思维层面上的实施成本较低、容易接受，但是缺点很多，比如难以传递、难以持续获得快速反馈等；而技术层面上的实施一般成本较高、不容易被人接受，但是优点更多，比如可以获得快速反馈、更容易传递和协作等。而现实世界中TDD的实施一般分为三个阶段，即无意识的TDD、被动通过技术实现的TDD、以及有意识和主动通过技术实现的TDD。

## 第一阶段：无意识的TDD

对于软件开发人员，当他们拿到一个新的软件需求时，首先会思考如何实现，其中包括当前软件架构、业务分解、实现设计、代码分层、代码实现等。然后通过思考和设计所得到的产出物来驱动代码实现，进而在代码实现中会思考如何通过一个或多个函数或者算法来实现业务逻辑。所以软件系统的实现要先通过意识层面的思考，再进行技术层面的工作。

![](http://insights.thoughtworkers.org/wp-content/uploads/2017/03/2-technology-consciousness.png)


当开发人员思考和设计这些函数或者方法的时候，一般都会思考它们有哪些参数，然后想象将这些参数换成真实的数据后传递进去，会得到怎样的返回值。好一点的开发人员会思考如何处理异常输入和异常返回值。

这类思考其实已经是意识思维上的TDD，它帮助开发人员先在大脑里面设计并验证代码实现，甚至帮助其重构代码。所以很多开发人员都在无意识的情况下做着TDD。

比如在一个银行系统里面，开发人员拿到一个需求，需要开发一个通过手机APP转账的功能。

1. 首先开发人员会基于当前的软件架构思考：是开发一个全新的模块来处理这个业务？还是基于当前架构中的某个模块来添加代码进行处理？
2. 当确定架构和设计之后，就开始思考具体的代码实现，比如类的设计、方法的设计或者函数的设计等。当开发“将钱从原帐号转出”这个功能前，开发人员会思考：这个功能需要支持当钱从原帐号中转出成功后，原帐号中的余额等于原始余额减去转出金额。进一步有些程序员还会设计一些用来验证功能的实例，比如帐号中的原始余额是999.99，转出111.11，那么剩余的金额就应该是888.88。
3. 在这样思考之后，开发人员便开始根据自己大脑中的测试逻辑和用例来驱动和辅助开发过程。在代码开发完毕之后还会想一些办法来验证一下所实现的功能是否符合预期，比如人工使用之前的或者新的测试用例再测试一下。如果验证正确，就会认为自己开发的功能正确了，并交给测试人员进行测试。
其实开发人员在开发前思考测试逻辑和用例的过程就是在做TDD了。

很多做业务分析的BA和测试分析前移的QA也同样在无意识的做着TDD(注：在[前一篇文章](http://insights.thoughtworkers.org/talk-about-tdd-again/)中已说明TDD包含ATDD)，比如分析验收条件、写出验收文档等。只不过这些AC和验收文档可能写得不是很明确或者不是很好，比如不是实例化需求等，但是本质上已经是TDD了。

只不过是初级的无意识的TDD，可能有的人做得好，有的人做得不好，而且没有明确的产出来协助和规范这个测试驱动开发方式，也缺乏快速反馈、度量、传递和协作等。因此从无意识到有意识将是做好TDD的一个重要过渡。

## 第二阶段：被动通过技术实现TDD

当有一部分软件工程师意识到了TDD的意义和普遍存在性之后，就开始准备解决思维上的TDD的缺点。而解决这些问题的方法就是在技术层面上用代码来实现TDD，用明确的代码来协助和规范开发人员的测试驱动开发行为，来度量他对业务逻辑以及代码实现的理解度。通过将他的理解传递给以后的维护人员，让他的理解能重复被使用，以及和其他人协作开发。

但是现实中很多开发人员的认识不足以及技术能力不够，就算管理层支持并且主动推动TDD，最终由于开发人员设计和选取的测试用例合理性很差，导致驱动出来的代码有效性差，测试用例无法体现出SBE(Specification by example)导致易读性差，对于自动化测试框架和测试编写不熟悉导致开发速度很慢等，往往是被动的在技术层面上去实现TDD，所以出现了各种怨言，各种抵触，进而导致技术层面上的TDD很难以大规模实施。

由于意识层面上的难易程度和工作量都比技术层面上相对较小，所以前者实施起来相对容易一些，而后者则相对较难，所以如果通过了各种手段强行实施TDD，而没有主动去摆正做TDD的意识，甚至没有足够的技术能力，那么这样的TDD就是一个倒三角，非常容易倒塌。

![](http://insights.thoughtworkers.org/wp-content/uploads/2017/03/3-Inverted-triangle.png)

TDD倒三角

所以，如果不希望技术层面上的TDD随时倒塌，就需要把这个倒三角补全，才能更好的、长久的实施TDD。

## 第三阶段：有意识和主动通过技术实现TDD
为了大规模以及有效的实施TDD，首先要突破思维意识的局限，认识到TDD的普遍存在性和适用性，不要害怕和排斥TDD这种思维和开发模式。

其次要主动学习，并刻意练习TDD的技术实现，提升自己的技术能力，从而在技术层面能更容易的实现TDD，摆脱被动TDD的困境。其中学习的方法包括阅读TDD相关的书籍和文章，书籍包括《测试驱动开发》、《重构》、《BDD In Action》以及《系统思考》等，从而充分理解TDD优点和局限。

对于刻意练习，一定要长时间坚持去做，让其成为一种习惯。如果在项目中没有合适的环境去练习，还可以通过一些第三方的TDD练习系统去做刻意练习，比如[Cyber-dojo](http://www.cyber-dojo.org/)。只有大量的刻意练习才能让你在真实的代码编写过程中去思考和理解TDD，去运用你通过学习得到的知识，最终才能做到有意识和主动的通过技术去实现TDD，TDD的倒三角才能变成一个稳定的砖块，然后哪里需要往哪里搬。

![](http://insights.thoughtworkers.org/wp-content/uploads/2017/03/4-TDD-brick.png)

TDD砖块

## 总结
综上，大部分开发人员都应该在做TDD，只不过他们是无意识的或者被动的去实现的，只有少部分是有意识和主动的去实现的。既然人人都在做TDD，那么我们为什么不能和黑客帝国里面的Neo一样选择红色药丸来认清楚现实，主动拥抱TDD，并通过大量的刻意练习去改变自己的工作习惯，让TDD成为自己工作习惯的一部分，这样才能更好的提升软件质量，大大降低软件维护成本。不管你信不信，反正我信了。
