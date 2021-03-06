---
layout: post

title: "重构有哪些参考准则？"
date: 2020-06-07
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

重构本身没有什么固定的准则，要说有，就回到重构的定义来看，重构之后不能改变软件的可观察的行为，也就是说你不能破坏软件的功能，只要能有效保证这一点，你可以随意重构。

如何保证这一点，你可以每次重构之后进行人肉测试，当然这种模式是不可持续的，人工成本大，不可重复，而且还易出错。为了有效构建测试安全网，在实际重构之前，非常建议给你要重构的功能添加上自动化测试。

有了自动化测试，你的重构就可以大刀阔斧了吗？我见过有些人进行大规模的重构（重写），最终现实原因（测试覆盖不全），引发了灾难。重构，就跟你做其他复杂的事情一样，最好是小步有节奏的前进，每重构一个小点，运行一下你的测试，要随时能够获取你当前的健康状态的反馈。一旦发生错误了，如果不能很快修复，你还可以选择回滚到之前健康的状态。

总结一下，记住重构的三条准则，为了方便记忆，我用了一个口诀：

- 时刻都要好（测试保护）
- 小步伐快跑（小步前进，随时回退）
- 性能以后搞

当然这些准则单纯看的话都能看懂，要想体会里面的滋味，还离不开大量的练习，在我的敏捷工程实践训练营中，我会让学员时刻参照这些准则，去重构代码坏味道。

留一个思考题给你，性能以后搞，不考虑性能吗？你是怎么理解的呢？

