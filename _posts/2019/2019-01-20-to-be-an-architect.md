---
layout: post
title: "架构师角色的养成"
categories: 阅读笔记
tags: 程序员
---

* content
{:toc}


本书是<a href="https://book.douban.com/subject/26248182/" target="_blank">《程序员必读之软件架构》</a>一书的读书笔记，说是读书笔记，其实就是把各章节认为重要的部分摘出来（当然这里忽略了一些我认为无关紧要的章节）。该书给我印象最深的的关于架构的阐述－－架构是一个角色，而不是级别。

虽然架构师基本都出身自程序员，但当他成为“架构师”时，带给他变化的不仅仅是级别职称，最本质的是角色的转换。这意味着架构师的工作重心不再是底层程序编写，而是一个技术领导。一方面，架构师的愿景需要靠技术团队来帮忙实现，所以架构师需要融入团队，有可能的话架构师还需要向项目贡献代码，架构师也可以通过各种方式让技术团队参与到架构设计里来，得到他们认同会让架构师的架构设计更容易展开；另一方面，架构师还有属于架构师自己的责任：架构驱动能力、设计软件、评估把握技术风险、架构演化、编写代码、质量保证。架构师需要对架构师的项目负责。架构师站在一个非常有影响力的位置上，架构师的一举一动都被整个开发团队看在眼里，架构师拥有改变团队的能量，让团队保持积极是架构师的责任。



## 什么是架构

- 将产品分解为一系列组件、模块和交互

- 了解你需要构建什么、设定愿景以便于构建和做出恰当的设计决策。架构是关于交流愿景以及引入技术领导力的。

- 不论何种领域的架构，其主要就是结构和愿景。




## 架构与设计

- 设计是指一个系统内命名的结构或行为，解决或有助于解决该系统的一个或多个问题，因而设计代表了潜在的决策空中的一个点。
- 所有的架构都是设计，但并非所有的设计都是架构。
- 架构反映了使一个系统成型的重要设计决策，而重要性则是通过改变的成本来衡量。
- 两者的定义有助于我们思考在我们的软件系统中那些可能是重要的。



## 软件架构的角色

软件架构师是一个角色，而不是一个级别

软件架构角色（可以是一个人，也可以是一个团队）应有的内容：

- 架构驱动能力：理解目标；抓住、提炼、挑战需求和限制
- 设计软件：建立技术战略、愿景和路线图
- 技术风险：发现、减轻和承担技术风险，保证架构的“运转”
- 架构演化：贯穿整个软件交付过程，持续的技术领导和对架构的承担
- 编写代码：参与到软件交付的实践部分
- 质量保证：引入并坚持标准、指导、原则

软件架构的角色基本上就是向软件团队引入技术领导。



## 软件架构师应该编码吗

### 编写代码

让编码成为你作为软件架构师角色的一部分，但不见得要成为团队里写代码最厉害的。

贡献代码能帮助你和团队建立起融洽的关系。

> 如果你了解如何编程，往往忍不住对开发者该如何编写代码提出建议。小心，因为你可能在浪费时间：如果你没有参与项目的编程，开发者多半会无视你的编码经验。他们还会认为你越权，影响他们的工作，所以尽量别在这方面指指点点。
>

### 构建原型、框架和基础

若没有时间参与写代码，对软件系统中的有疑问的概念构建原型和证明是一个很好的参与方式

### 进行代码审评

这不是一个长期的战略，但参与代码审评至少能让你了解新技术机器应用

### 实验并与时俱进

保持一定水平的技术知识才能称职地用它来进行方案设计。

在工作之外（工作之内你很少有机会参与写代码）贡献开源代码、尝试最新语言、框架、API，途径：书、博客、播客和聚会

### 不要把全部时间用于编码

如果你花全部时间写代码，那软件架构角色的其他部分由谁来扮演？



## 从开发者到架构师

软件设计的过程看起来相当简单，要做的就是搞清楚需求，设计一个能满足他们的系统。但现实中可不是这么简单，人们承担的软件架构角色可能千差万别，比如：

1. 架构驱动力：捕捉和挑战一套复杂的非功能需求，还是简答地假设他们的存在
2. 设计软件：从零开始设计一个软件系统，还是扩展已有的
3. 技术风险：证明你的架构能够工作，还是盲目乐观
4. 架构演化：持续参与和演化你的架构，还是把它交给“实现团队”
5. 编写代码：参与交付的实践部分，还是袖手旁观
6. 质量保证：保证质量并选择标准，还是反其道而行之或无作为

给一个软件系统的架构出力和为之负责之间有一个很大的差异，那就是构成软件架构角色所需的，跨越不同领域融会贯通的技能、知识和经验.



## 拓展Ｔ（技术）

软件架构不是一个“后技术”或者“非技术”的工种。

### 进一步的技术能力

也许会受困于单一的技术栈，但你不得不小心地保持开放思维，不要被经验束缚

### 知识面宽

软件架构角色可能需要回答下面的问题：

- 和其他可选技术相比，我们所选的是否最合适
- 对该系统的设计和构建，还有哪些选择
- 是否应该采用一种通用的框架模式
- 我们是否明白所做的决策的利弊
- 我们照顾到了品质属性的需求吗
- 如何证明这种架构行之有效

### 软件架构师是通才型专家

能够在底层细节和大局之间切换



## 软技能

- 领导力：创造共有愿景，并带领人们向着共同目标前进的能力
- 沟通
- 影响力：这是最重要的技能，从毫不掩饰的劝说到神经语言编程或绝地控心术，它能够以多种途径实现。通过妥协和谈判也可以达到这样的目的。每个人都有自己的想法和计划，你在处理时还得让他们都不反感，并主动去追求你需要的结果。好的影响力也要求好的倾听和探索能力。
- 信心：信心很重要，是有效的领导力、影响力和沟通的基础。但信心不代表傲慢。
- 合作：软件架构角色不应该被孤立，（与其他人）合作想出更好的方案是一项值得实践的技能。这意味着倾听、谦逊和响应反馈
- 指导
- 辅导
- 动力：保持团队愉快、开朗和积极。团队要有积极性才会跟随你这个软件架构师所创建的任何愿景。你还要面对团队中一些人不买账的局面
- 润滑剂：你经常需要退后一步，促进讨论，特别是团队内有不同意见是，这需要探索、客观，帮助团队达成共识
- 政治：每个组织都少不了政治，我的咒语是，离得越远越好，但你至少应该明白周围发生了什么，这样才能做出更可靠的决策。
- 责任感：你不能因为失败就责备软件开发团队中的其他人，有责任感对你而言很重要。如果软件架构不能满足业务目标，无法交付飞功能性需求或技术品质很差，那都是你的问题。
- 授权：学会在适当的时候授权，但请记住，你授权的可不是责任


不管你是否意识到，你都在一个非常有影响力的位置上，你的一举一动都被整个开发团队看在眼里。不管愿不愿意，但是这个原因，你就有改变整个团队的能量。如果你很主动，开发团队也可能变得很主动。如果你对工作充满热情，团队其他人也会很可能充满热情。如果你对每件事很乐观，开发团队也会这样。

作为领导，让团队保持积极是你的责任，你的角色在整个团队中不应该被低估。

## 软件架构需要引入控制吗

很多软件架构的实践都会向软件项目引入指导和一致性。只有引入一定程度的控制和约束，比如阻止团队成员偏离正轨，指导和一致性才可能成为现实。

问题是需要引入的控制量。一种方法是独裁，另一种方法是放手。

把控制看做操纵杆，而不是某些非黑即白，只有两种状态的东西。一端是由你独裁的方法，另一端则宽松得多，两者之间你可以调整。建议先从部分控制开始，倾听反馈，以便随着项目的推进再微调。如果团队老是问“为什么”和“怎么办”，那可能就需要更多的指导，如果团队好像总是在和你对着干，可能你就是把操纵杆推得太多了。

## 小心鸿沟

软件架构角色需要负责“大局”，很多团队会找来一个有着受人尊敬的“架构师”头衔的人，却只把他们硬塞在凌驾于团队的位置上。发生任何事，都会在架构师和原本要一起工作的团队之间造成巨大的鸿沟，让架构师被立刻孤立起来，特别是当架构师被看做是只会下命令的独裁者。这导致了几个问题：

- 不管架构师的决策是否正确，开发团队都不尊重他；

- 开发团队变得缺乏积极性

- 重要决策因为职责不明而无人负责

- 因为没有人负责大局，项目最终变得苦不堪言


拉近距离：

- 包容与合作：让开发团队参与软件架构的过程，帮助他们了解大局，认同你所做的决策。确保每个人都明白决策背后的原理和目的，会对此有所帮助
- 动手：如果可能，参与一些项目的日常开发工作来提高你对架构交付的理解，或者协助设计和代码评审

