---
layout: post
title: 李航《统计学习方法》学习笔记(2)--感知机
categories: 阅读笔记
tags: 机器学习
---

* content
{:toc}


感知机是二分类的线性模型，其输入是实例的特征向量，输出的是事例的类别，分别是+1和-1，属于判别模型。

假设训练数据集是线性可分的，感知机学习的目标是求得一个能够将训练数据集**正实例点和负实例点完全正确分开的分离超平面**。如果是非线性可分的数据，则最后无法获得超平面。

感知机是神经网络与支持向量机的基础。



## 一、感知机模型

f(x) = sign(w·x+b)

其中：w为权值，b为偏置，sign是符号函数(即：若w·x+b ≥ 0，则取值 +1，反之取值 -1)。

感知机的几何解释：

线性方程　w·x+b = 0   对应于特征空间中的一个超平面S，其中w是超平面的法向量，b是超平面的截距。这个超平面将特征空间划分为两个部分。位于两部分的点分别被正、负两类。因此超平面S称为分离超平面。



## 二、感知机的学习策略

假设数据是线性可分的，感知机学习的目标是求得一个能够将训练数据集正实例点和负实例点完全正确分开的分离超平面。超平面。

损失函数是误分类点到超平面S的总距离(函数间隔):

L(w,b) = -∑y<sub>i</sub>(w·x<sub>i</sub> + b)

显然，损失函数是非负的，如果没有误分类点，损失函数值是0。而且，误分类点越少，误分类点离超平面越近，损失函数值就越小。

感知机学习的策略是在假设空间中选取使损失函数最小的模型参数w，b，即期望使误分类的所有样本，到超平面的距离之和最小。



## 三、感知机学习算法

感知机学习算法是误分类驱动的，具体采用随机梯度下降算法：首先，任意选取一个超平面w<sub>0</sub>，b<sub>0</sub>，然后用梯度下降法不断地极小化目标函数(损失函数)，极小化过程中不是一次使所有误分类点的梯度下降，而是一次随机选取一个误分类点使其梯度下降。

感知机算法由于采取不同的初值或选取不同的误分类点，解可以不同（支持向量机求解得到的则是唯一的超平面）。



