---
title: KCF跟踪算法总结
date: 2018-7-3
categories: coding
tags: 
    - kcf 
    - track
---

跟踪算法可大致分为两类，基于判别模型，和基于生成模型。基于判别模型的跟踪算法通过训练识别目标的分类器，对下一帧的每一个区域进行判别，通过找到目标以达到跟踪的目的，即tracking by detection，例如correlation filter；基于生成模型的跟踪算法通过构建隐变量模型，预测下一帧目标可能出现的位置，即tracking by prediction，例如particle filter。前者效果和速度都显著优于后者。
更新论文的阅读笔记 at 2019.3.14
<!--more-->

## 概述
跟踪算法可大致分为两类，基于判别模型，和基于生成模型。基于判别模型的跟踪算法通过训练识别目标的分类器，对下一帧的每一个区域进行判别，通过找到目标以达到跟踪的目的，即tracking by detection，例如correlation filter；基于生成模型的跟踪算法通过构建隐变量模型，预测下一帧目标可能出现的位置，即tracking by prediction，例如particle filter。前者效果和速度都显著优于后者。
## 算法目标
训练回归模型寻找最小化样本的平方误差的函数\\(f(x)=w^Tx\\)。即
$$\underset{w}{Min}\underset{i}\sum{(f(x)-y)}^2+\lambda{||w||}^2$$
其中\\(\lambda {||w||}{^{2}}\\)为正则项，用以控制模型的复杂度以防止过拟合。  
该优化问题存在解析解，为
$$w=\left ( XX^{^{T}}+\lambda I \right )^{^{-1}}X^{^{T}}y$$
## 特点
上述计算式的时间复杂度为\\(O\left ( n^{3} \right )\\)，n为特征数量即像素点数  
我们引入循环矩阵形如
$$X=C(x)=\begin{Bmatrix}
 x_{1}  & x_{2}   & \cdots & x_{1} \\\ 
 x_{n}  & x_{1}   & \cdots & x_{2} \\\ 
 \vdots & \vdots  & \ddots & \vdots\\\ 
 x_{2}  & x_{n-1} & \cdots & x_{n}
\end{Bmatrix}$$
利用其性质将其对角化，即
$$X=Fdiag(\hat{x})F^{H}$$
将其代入可得
$$\hat{w}=\frac{\hat{x}^{H}\odot \hat{y}}{\hat{x}^{H}\odot \hat{x}+\lambda }$$
其中\\(\odot\\)为向量间元素相乘，\\(\hat{x}\\)为\\(X\\)对角化后的向量此计算式的复杂度为\\(O(nlogn)\\)  
因样本在其维度上不可分，所以对其进行非线性特征变换将其映射到更高维空间使其线性可分。推倒过程如下
$$f(x_{i})=w^{T}\varphi (x_{i})$$
$$w=\underset{w}{Min}{||\varphi (X)w-y||}^2+\lambda{||w||}^2$$
$$w=\sum_{i}{\alpha_{i}\varphi (x_{i})}$$
$$\alpha =\underset{\alpha }{Min}(||\varphi (x)\varphi (x)^{T}\alpha -y||^2+\lambda ||\varphi (X)^{T}\alpha ||^{2})$$
该问题称为\\(w\\)的对偶问题,另关于列向量\\(\alpha\\)的导数为0,得
$$\alpha =(\varphi (X)\varphi (X)^{T}+\lambda I)^{-1}y$$
最后，可得
$$w=\sum_{l=1}^{n}\alpha_{l}x_{l}$$
$$\hat{f}(\cdot)=\sum_{l=1}^{n}\alpha_{l}<\cdot ,x>$$  

### 以上基础公式推倒对理解KCF算法并没有什么卵用


### tip

1. 根据之前帧与当前帧训练一个相关滤波器，也就是求滤波器参数，是一个岭回归问题
2. 加余弦窗的作用？ 因为傅里叶变换是周期性的，变换之后在图像的上下左右边缘会出现噪声 

## 整体思路

如上文所述
![](https://ws1.sinaimg.cn/large/007xDx5ily1g19j42yjccj305v02sa9v.jpg)
到这一步时，其实滤波器已经得到了。但是注意，这里的x并不是一个二维矩阵，而是由二维图像组成的一维向量。 
从头说起，我们对原始目标框进行循环位移后得到了M\*N个样本，如图(这里是3\*3)
![](https://ws1.sinaimg.cn/large/007xDx5ily1g19j9rszbhj304n05iabd.jpg)
这副图片有误导效果，不应排列成3\*3的形式，其实它应当看成是1\*(M\*N)的一维向量，此一维向量再去组成2D循环矩阵。 
所以上面公式中的x可以看成是一个高维特征，那么，此时引入核方法。
![](https://ws1.sinaimg.cn/large/007xDx5ily1g19jgkhcxtj3041036gle.jpg)
在傅里叶域的形式
![](https://ws1.sinaimg.cn/large/007xDx5ily1g19jhy3e1sj302p01c0me.jpg)
在时域中的K是上面所说“由二维图像组成的一维向量”的核相关矩阵，变换到频域后，Kxx帽也就是K矩阵的第一行，也就是“一维向量第一个元素与各个元素的相关性”。而原始循环矩阵的第一行是由原始目标框循环位移得来，变换到频率域后，因为傅里叶变换的周期性，可以看成是原始目标框在频率域的自相关。注意此时得到的Kxx帽是一个一维M\*N(样本数量)项的向量 

历时9个月，因为上课时老师提起卷积与相关，才猛然惊觉已将KCF算法忘的差不多了，回头再看，当时学的懵懵懂懂，现在看来也是一头雾水。索性这半年多没有荒废，知乎，博客，原论文互相印证终于将原理搞懂。写到此处，才真正对KCF算法思考通透，有两点感悟，一是对文献的阅读能力有待提高，二是以前你以为很厉害的人会在你进步的途中变得没有那么厉害，不宜妄自菲薄。

### 学如逆水行舟,且行且记
