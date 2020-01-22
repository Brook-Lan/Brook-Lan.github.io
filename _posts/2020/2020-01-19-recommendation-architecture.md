---
layout: post
title: 推荐系统架构
categories: Tech
tags: [架构, 推荐系统]
---
* content
{:toc}
这是给公司设计的推荐系统架构



## 一、解决的问题

1.信息过载

2.挖掘长尾

3.用户体验



## 二、逻辑架构

![逻辑架构](/posts_img/2020/recommendation/recommendation1.png)

**召回**：模型简单，速度快，从海量文章中捞出数量级在几百以内的文章（需体现多样化、个性化）

**排序**：对召回的文章进行点击率预估，并按照预估的概率排序，挑选 top K(已经过滤用户浏览过的内容) 进行推荐

**业务策略**：与业务相关的内容，比如广告、公告之类的



## 三、技术架构

![物理架构](/posts_img/2020/recommendation/recommendation2.png)

- 离线层主要是一些数据量大、耗时高的计算，如矩阵分解，通常是一天跑一次
- 近线层则动态跟进用户最新的行为数据，召回速度应要快



## 四、冷启动

推荐系统基于用户大量的历史行为数据。但实际场景中，并非所有的用户都拥有丰富的历史数据（如新注册用户）。对此，有以下几种冷启动方式

- 基于热门物品（文章）推荐
- 利用用户注册信息（注册时引导用户选择感兴趣的主题或频道）
- 利用用户上下文
- 预先准备一份官方推荐阅读列表



## 五、推荐阅读

[推荐系统召回四模型之：全能的FM模型](https://zhuanlan.zhihu.com/p/58160982)

[京东电商推荐系统实践](https://mp.weixin.qq.com/s/vpANPrl86Ou2fBVHgLXtBQ)

[今日头条算法原理（全）](https://mp.weixin.qq.com/s/DC_hJUbTnLhuCwYVOgVlVw)

[知乎推荐系统的实践及重构之路](https://zhuanlan.zhihu.com/p/53130925)
