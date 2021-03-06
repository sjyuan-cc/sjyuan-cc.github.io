---
layout: post

title: "服务员，能帮忙来双公筷吗？"
date: 2020-06-17
categories: [eXtreme Programming]
tags: [REFACTORING-QUESTION-100]
column: REFACTORING-QUESTION-100
sub-tag: "Code Design"

author: "袁慎建"

brief: "
百问重构系列问答。
"

---

* content
{:toc}

---

前天跟朋友吃火锅，为了图省事，我们叫了个套餐再外点了几个菜。一开始我的目光就瞄上眼前的那盘鲜嫩的牛肉。刚准备用自己的筷子夹到锅中。突然想起来，自己的筷子不要用来夹生肉（粗人一个，完全不是出于礼仪考虑），于是我就很麻利呼叫服务员：“服务员，能-帮忙来双公筷吗？” 服务员：“什么，公筷？” 只见她条件反射搬了问了一句，并迟疑了有3秒钟，然后说：“哦，好的，稍等～”

完成了跟服务员的短暂对话，转过头来，朋友就开始哈哈大笑。因为我刚才对服务员说了个公筷。突然间我好像明白他的笑点了，于是我俩开始对刚才的公筷时间召开讨论。

我跟服务员要公筷，服务员可能经历如下思绪：

1. 公筷是什么？
2. 跟筷子又有区别是什么？
3. 是我们大厨用的那种大筷子吗？
4. 不对，肯定不是这个，他们要这个作什么？
5. 我自己平时吃火锅的时候，会不会用公筷？
6. 哦对，就是桌上顾客吃饭的筷子，搞定，欧耶

按照上面这个过程，服务员经历了3次角色转换，连续问了自己5个问题，然后确定了顾客的确切需求，虽然只用了5秒钟搞定，但也把一个1秒钟能搞定的问题给复杂化了5倍。

到底是什么在作祟呢？

我在跟服务员说说公筷的时候，公筷并不是一个能够一致被彼此理解的词，这个词是在特定场合根据用途来定义的一个词，比如说一桌人吃饭，需要一个下菜和筷子，在该上下文中，我们一桌人很容易理解。而对于服务员，她也有一个跟大厨的上下文中的公筷，所以他第一反应会想到那个。这个就导致在不同上下文中，同一个术语会有不一样的理解。

我其实是需要一双筷子。当我表达需要一双筷子的时候，服务员大概率能够快速理解我的需求 -- 吃饭的筷子，这个时候能够短时间构建一个相同上下文，彼此很快达成一致理解。至于我要怎么使用这双筷子，这是餐桌内部的事情，将这个细节隐藏起来，会一定程度降低服务员的认知负担。

**回到软件设计中来，统一语言，隐藏内部实现细节，简化接口调用，这些都是我们应该多去思考的设计要素。**

我如果直接表达需要一双筷子，服务员思考过程中的5个问题和3次角色转换就可以省掉了，事情是不是就简化很多了呢？
