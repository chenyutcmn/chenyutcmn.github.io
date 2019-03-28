---
title: 'R-CNN学习笔记'
date: 2018-10-28 14:55:01
tags: R-CNN
categories: note
---

对RCNN系列论文的阅读笔记
<!--more-->

## R-CNN学习笔记 
#### 算法流程
1. Region proposal提取约2000个目标区域
2. 对CNN网络进行有监督的预训练
3. 把第一步得到的2000个区域放入网络提取特征并fine-tuningSVM分类器，到这一步网络已训练好
4. 对检测到的目标框进行bounding box回归
