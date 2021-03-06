---
layout: post

title: "客户让你给代码加注释怎么办？"
date: 2020-06-23
categories: [eXtreme Programming]
tags: [REFACTORING-QUESTION-100]
column: REFACTORING-QUESTION-100
sub-tag: "Code Smell"

author: "袁慎建"

brief: "
百问重构系列问答。
"

---

* content
{:toc}

---

作为乙方，我以前听过一些同事说客户要求给代码加注释，一开始自己不以为然，直到有一天这件事情发生在我身上：某大有可为的项目接近尾声，准备下项目前一周，PM说接到客户的诉求，要求给代码加上注释，听到这个信息，内心蛮崩溃的，团队所有开发人员都把大大的拒绝写在脸上，PM也一脸无奈的疯狂吐槽。

怎么办，你以为我们会这么乖乖的听话，去吭哧吭哧的无脑加注释吗？并没有，毕竟这跟我们的做事的价值观发生了冲突的，不能因为客户的一个要求就去满足，虽然客户是上帝。

我们后来通过多次沟通，成功说服客户放弃这个念头，当然也了解客户提这个要求背后的担心点 -- 之前合作的一些Vendor写的代码是在是没法看懂，基本上都会要求他们把注释写齐全，有时候会作为验收标准。

所以客户要求加注释，并不是代表客户认为注释是个好东西，只是因为他没法通过其他手段来保证系统的可维护性，退而求其次，尝试通过一刀切来管理Vendor。

回到注释问题的本身。注释很早前被Martin Fowler在《重构》一书中列为一种代码坏味道，一些真正的敏捷开发者也非常赞同注释是一种代码臭味，但是他们也提到是有特例的。一些挤入软件开发的新人，可能会不明就里的走两个极端：

1. 一行代码得配一行注释，代码无注释不欢。
2. 拒绝任何注释，代码只为自己代言。

> 先下结论：凡是走极端的事情都可能有问题  -- 《中庸》一书中没提到过

讨论注释好不好之前，还是要回答为什么，你为什么需要添加注释？注释在解决你什么问题？同时又会有什么副作用？如果连这三个问题都没有思考过就去加注释，那你很可能就是在无脑添加注释，坏处多多。

- 我这代码太长了，干了5件事情，我要在每件事情前说明一下
- 我没有想到好的英文名字，我还是用中文解释一下吧
- 我这个代码用了缩写，可能别人看不懂，不如来点注释说明一下
- ......

以上理由的初衷都是好的，好像都是想让别人看懂自己的代码，但这种做法犹如茅房很臭，想让上厕所人的好受一点，于是买了一盒增香剂放到茅房中，你猜猜结果会怎样，非常奇特的“香臭味”，刺鼻难闻。这个时候你应该想办法让茅坑不要那么臭。同理，这个时候你就应该让代码本身就能表达意图，这才是治根的方法。

- 干了5件事情，拆成5个方法，把你的注释用在方法命名上
- 没想到好的英文名，去查词典，问同事，起名字哪那么容易搞定
- 缩写很酷吗？自己都觉得不好意思给别人看的代码怎么好意思写出来

当然还有一个直击灵魂的问题：后期，你的代码修改改了，注释会被改吗？通常是不会，别抬杠说你会改，好好想想你改过没有！代码改了，注释没改，就等于撒谎的注释，简直就是罪大恶极。怎么避免，**逼自己不要写注释，当你感觉需要添加注释的时候，先尝试重构，试图让注释变得不再必要。**

当然也不要走极端，毕竟编程不是1+1=2这么简单事情，什么样的场景你都可能遇到，而不是什么样的场景都能应对如流的。

1. 比如，有时候实在很无奈，写了一段颇有苦衷的代码，这时候就用注释来解释一下为什么这么做吧，好让读者知道原由。
2. 比如，代码需要标注法律信息，审计要求，或者版权要求，想做一个好公民，避免出差错，来点注释吧。
3. 比如，你要写一个公开于世界的API，造福人类（程序员），你需要提供一些详尽的API文档信息和版本变迁信息，加点注释吧，毕竟你要以最低标准要求你的用户

当然我尝试给写注释赋予了一个原则 --  **解释Why，高于广播How**


到此，你认为什么时候应该加注释、什么时候不应该加注释呢？

