---
layout: post
title: YouTube深度推荐系统模型召回serving理解
categories: Tech
tags: 推荐系统
---
* content
{:toc}


最近看了一大佬解读Youtube深度推荐系的论文解读([YouTube深度学习推荐系统的十大工程问题](https://zhuanlan.zhihu.com/p/52504407))，解读中提到了模型serving阶段的user vector和video vector的生成时，说到：

>...在原文中并没有介绍得到user embedding和video embedding的具体过程，只是在架构图中从softmax朝 model serving module那画了个箭头（如下图红圈内的部分），**到底这个user vector和video vector是怎么生成的？**有经验的同学可以在评论中介绍一下。

其下的回复里，用户名为[老昶信](https://www.zhihu.com/people/lao-chang-xin) 的回复得到的作者的肯定，可能这就是因为我成不了大佬的原因吧，看了半天还是没看太懂。后续又经过一番努力，总算理解过来，这里记录下我的的理解。

在我们直奔主题之前，建议先看上面大佬的解读（见文末链接）

废话不多说，我们先上图：

![召回模型](/posts_img/2020/youtube-recall/recall.jpg)

我们看到左上角的serving部分：最后一层(relu层)输出user vector；softmax输出video vectors。将两者计算内积取TopN即为推荐结果。

问题来了：模型的输入是用户用户特征数据，怎么最后又是输出user vector，又是输出video vector？

要理解这个问题，需要抓住以下两个两个重点：**模型结构的简化**和**word2vec**相关知识



## 一、被简化的模型结构

我们看到上图中，模型是由好几个Relu激活层堆叠起来，很明显，激活层前面的一些网络结构被省略了。可能是作者认为这些层无关宏旨，在实践过程中，每个人都可以自行设计符合业务需求的网络。其次，也是最重要的，**最后一层的softmax前面也省略了一个全联接层，video vectors 便是这个全联接层的权重**。这点稍微接触过深度学习的朋友应该不难理解(汗，笔者性子直来直去，一开始对softmax层没想太多，所以一直卡在那，怎么也没想通那两个vector是怎么生成的)

## 二、word2vec的伪装

既然是vector，自然得提到word2vec，这里对word2vec的细节不提太多，大致说下vec是怎么来的，以skip-gram为例：

![word2vec-skip-gram](/posts_img/2020/youtube-recall/skip-gram.jpg)

上图中的 $$W_{V\times N}$$便是通过训练得到的所有词向量，隐含层h是$$W_{V\times N}$$中的某一列，即某个词的词向量。从那么右边的$$W^{'}_{N\times V}$$是什么呢？其实这个也是词向量，是另一个空间的词向量。这个词向量与h做点积便是h与该空间里的词的相似度。因为右边是一个多分类(每个类是一个词)，所以对应每个分类的权重便是这个词的另一个空间里的词向量。（关于词向量是输入向量还是输出向量，参见 [《万物皆Embedding，从经典的word2vec到深度学习基本操作item2vec》](https://zhuanlan.zhihu.com/p/53194407)）

看到这，相信大家已经醒悟过来，原来之前提到的youtube里的user vector 和 video vectors，原来本质还是word2vec，只是这里softmax输出的video分类，自然输出向量表示的是video，而softmax全连接之前的向量为什么是user vector，是因为整个模型的最开始输入都是用户相关的特征。多说一句，相比word2vec，youtube的召回网络的输入用的并不是用户id，而是用户的行为，并经过一些网络层做抽象表达，但传达的信息还是用户的信息，所以最后的输出自然是user vector



## 三、附

[YouTube深度学习推荐系统的十大工程问题](https://zhuanlan.zhihu.com/p/52504407)

[重读Youtube深度学习推荐系统论文，字字珠玑，惊为神文](https://zhuanlan.zhihu.com/p/52169807)

