---
layout: post

title: "没有测试保护的重构叫重构吗？"
date: 2020-06-05
categories: [eXtreme Programming]
tags: [REFACTORING-QUESTION-100]
column: REFACTORING-QUESTION-100
sub-tag: "Basic Concept"

author: "袁慎建"

brief: "
百问重构系列问答。
"

---

* content
{:toc}

---

问题是在问没有测试保护的重构叫Martin Folwer原滋原味的重构吗？那的先来回顾一下老马对重构的定义：

1. 名词：在不改变软件外在的行为基础上，对代码结构做出的调整，从而提高代码的可理解性。
2. 动词：在不改变软件外在的行为基础上，调整代码的内部结构，从而提高代码的可理解性。

如果你对代码进行了重构，如果破坏了系统的功能，那你这个只能叫修改，要想跟重构近一点，可以叫重写。当然，如果你很幸运，修改没有破坏系统的功能，那你就是在做重构了。

且慢，重构的过程中，我肯定会有破坏系统功能的时候呀，Martin Folwer和Kent Beck都难以保证不破坏系统功能的短暂时期吧。所以，如果按照原始的定义，那些说自己在做重构的人，有胡吹的嫌疑了。

所以，即便你有测试保护，你还是会破坏系统功能啊，但是你有测试，这些测试能不能让你快速知道系统功能是否破坏了？是你要思考的一个重点。

**所以，重构有没有测试保护不重要，重要的是你重构之后，有没有一个快速反馈的机制。这个机制可以让你重构的代码在你进行下一个功能开发之前拿到反馈，反馈什么，反馈你刚才的重构有没有破坏原有的功能。**

为什么这么讲？

有没有测试保护，测试要不要自动化等等，这都是操作层面的事情，操作层面的事情通常都是Case by Case的。那么回到问题的本质，重构是一项极限编程实践，极限编程的价值观和原则也是符合敏捷的价值观和原则的，重构也要能体现出敏捷的核心思想和原则 -- 快速有效的反馈。否则如果，你更改了代码，只有在跟别人的代码进行集成之后的时间才发现问题，那这就肯定不属于极限编程中的重构了。

所以，只要你能保证在你重构代码后能够快速获得系统没有被破坏的反馈，我觉得都算是重构。至于如何保证，那就有各种手段了，通常最为推荐的方式就是编写自动化测试。

我也听到过有人说 -- 没有测试（自动化）保护，所有代码都修改不叫重构，对于这种过于绝对化的观点，我一般笑而不语！回到重构本身的定义，回到敏捷的本质，你所做的是不是叫重构就一目了然了，跟你有没有测试没有直接关系，只是说自动化测试是业界推荐的一种方式。

留一个问题思考：

当你的自动化测试很慢，而且不稳定的时候，没法帮助你快速获得有效的反馈，而人工测试会更快，这种情况你会怎么做呢？
