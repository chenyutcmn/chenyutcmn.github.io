---
title: BERT_Flow
date: 2021-02-06 22:57:31
tags: BERT-flow
categories: paper summary
---
<!--more-->
从NICE, Real NVP, Glow到BETR_Flow
## BERT-Flow 总结
### motivation
BERT已在各大任务中有良好表现，然而在句子表示学习相关的问题中(如文本相似度)仍表现欠佳，甚至不如一些较为简单的模型(如Glove)。

本文认为，这是由embedding space的各项异性导致的，及句向量在空间中分布不均。

提取句向量有两种方式，一种是使用所有词向量的平均，一种是使用cls token。实验表明，使用平均的方式效果更好。这可以理解为句向量是由所有词向量融合而成的。然而，词是有词频的，有的词出现的频率高，有的频率低。
这造成word embedding space的分布是不均匀。平均之后的句向量可能落在word embedding space中undefined的区域，破坏了空间的凸集性质。

若以上假设成立，那么将句向量映射至高斯分布，即可缓解问题。
### 流式模型
流式模型是作为一个生成模型被提出的。可参考VAE，GAN网络，即通过一个已知的分布，拟合一个未知分布，公式如下

![2021020701.PNG](http://ww1.sinaimg.cn/large/ed603485ly1gnez6274e2j209o03kmxv.jpg)

这里 q(z) 一般是标准的高斯分布，而 qθ(x|z)=qθ(x|z) 可以选择任意的条件高斯分布或者狄拉克分布。这样的积分形式可以形成很多复杂的分布。理论上来讲，它能拟合任意分布。

在参数 θ 下，x满足分布q(x)，为了求出参数 θ ，可以通过极大化目标求解，这里假设真实数据分布为 p̃(x)

![2021020702.PNG](http://ww1.sinaimg.cn/large/ed603485ly1gnezh5havtj208b02waai.jpg)

这里的积分形式难以求解。但是x可以由z通过一系列可逆变换(仿射变换)得来，也就是说我们也可以有x得到z，z的分布又是已知的，那么我们可以建立z的分布与x的分布之间的联系。

z在prior分布上的概率密度好算, 那怎么计算每个仿射变换前后概率的变化量呢?

![2021020705.PNG](http://ww1.sinaimg.cn/large/ed603485ly1gnezpzs9gyj20oy0n142j.jpg)

![2021020706.PNG](http://ww1.sinaimg.cn/large/ed603485ly1gnezqvr7t7j20oq0sjq8c.jpg)

![2021020707.PNG](http://ww1.sinaimg.cn/large/ed603485ly1gnezrg6zl1j20gf09gmxo.jpg)

以上为流式模型原理，在NICE, Real NVP, Glow还有复杂的细节未看透彻，留待之后研究

## todo：NICE, Real NVP, Glow的演进