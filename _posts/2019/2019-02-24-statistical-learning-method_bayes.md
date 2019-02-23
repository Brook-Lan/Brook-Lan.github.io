---
layout: post
title: 李航《统计学习方法》学习笔记(4)--朴素贝叶斯法
categories: 读书笔记
tags: 机器学习
---

* content
{:toc}


朴素贝叶斯法是基于**贝叶斯定理**与**特征条件独立**假设的分类的方法。对于给定的训练集，首先基于特征条件独立假设学习输入/输出的联合概率分布，然后基于此模型，对给定的输入x，利用贝叶斯定理求出后验概率最大的输出y。



## 一、学习与分类

### 1.1 基本方法

朴素贝叶斯法通过训练数据集学习联合概率分布P(X,Y)，具体地，学习先验概率分布及条件概率分布。

先验概率分布：
$$P(Y=c_k)$$，　k=1,2,...,K

条件概率分布：
$$P(X=x|Y=c_k)=P(X^{(1)}=x^{(1)}, ..., X^{(n)}=x^{(n)}|Y=c_k)$$，　k=1,2,...,K

从上面的式子可以看出，条件概率分布有指数级数量的参数，其估计实际是不可行的。朴素贝叶斯法对条件概率作了**条件独立**的假设：

$$
\begin{equation}\begin{split}P(X=x|Y=c_k) = P(X^{(1)}&=x^{(1)}, ..., X^{(n)}=x^{(n)}|Y=c_{k}) \\
= \prod_{j=1}^n P(X^{(j)}=x^{(j)}|Y=c_{k}) \\
\end{split}\end{equation}
$$

条件独立假设等于是说用于分类的特征在类确定的条件下都是条件独立的，这一假设使得贝叶斯法变得简单，但有时会牺牲一定的分类准确性，**朴素**贝叶斯也因此得名。

朴素贝叶斯分类时，对给定的输入x，由学习到的模型计算后验概率分布，将后验概率最大的类作为x的输出。

后验概率由贝叶斯定理计算得到：

$$P(Y=c_k|X=x)=\frac{P(X=x|Y=c_k)P(Y=c_k)}{\sum_kP(X=x|Y=c_k)P(Y=c_k)}$$

化简后最终输出为：

$$y=\arg\max_{c_k}P(Y=c_k)\prod_jP(X^{(j)}=x^{j}|Y=c_k)$$



### 1.2 后验概率最大化的含义

朴素贝叶斯法将实例分类到后验概率最大的类中，等价于**期望风险最小化**



## 二、参数估计

### 2.1 极大似然估计

先验概率$$P(Y=c_k)$$的极大似然估计是：

$$ P(Y=c_k) = \frac{\sum_{i=1}^{N}I(y_i=c_k)}{N},　　k=1, 2, ..., K$$

条件概率的极大似然估计为：

$$P(X^{(j)}=a_{jl}|Y=c_k)=\frac{\sum_{i=1}^{N}{I(X_i^{(j)}=a_{jl}, y_i=c_k)}}{\sum_{i=1}^{N}I(y_i=c_k)}$$

$$j=1,2,...,n ;    l=1,2,...,S_j ; k=1,2,...,K$$

式中，$$x_i^{j}$$是第i个样本的第j个特征；$$a_{jl}$$是第j个特征的可能取的第$$l$$个值；$$I$$为指示函数，即：

$$
I(x=a) =
\begin{cases}
1, & \text{x = a}  \\
0, & \text{x $\neq$ a}
\end{cases}
$$

### 2.2 贝叶斯估计

用极大似然估计可能会出现所要估计的概率值为0，这时会影响后验概率的计算结果，使分类产生偏差。解决这一问题的方法是采用贝叶斯估计。

条件概率的贝叶斯估计：

$$P_{\lambda}(X^{(j)}=a_{jl}|Y=c_k)=\frac{\sum_{i=1}^NI(x_i^{(j)}=a_{jl},y_i=c_k)+{\lambda}}{\sum_{i=1}^NI(y_i=c_k)+S_j{\lambda}}$$

式中$$\lambda≥0$$，等价于在随机变量各个取值的頻数上赋予一个正数$$\lambda$$ 。当$$\lambda=0$$时就是极大似然估计，常取$$\lambda=1$$ ，这时成为拉普拉斯平滑。

先验概率的贝叶斯估计是：

$$P_{\lambda}(Y=c_k)=\frac{\sum_{i=1}^NI(y_i=c_k)+{\lambda}}{N+K{\lambda}}$$
