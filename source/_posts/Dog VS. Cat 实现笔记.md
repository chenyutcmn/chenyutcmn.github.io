---
title: dog VS. cat 的实现
date: 2019-04-10 10:40:28
categories: note
tags: pytorch
---
一些在实现Dog VS. Cat项目中踩过的坑
<!--more-->
### 程序主题步骤
1. 获取数据集 需实现DogCat类，使用dataloader来获取batch，获取时进行预处理
2. 设置优化器，设置损失函数
3. 模型设置训练模式，输入以得到输出，计算Loss，回传，优化器优化

---
### 显存不够用
加钱

但是我没有。。。。。艹

---
### Cannot allocate memory
重启可解决，未发现原因

---
### argparse
argparse.ArgumentParser()可替代配置文件 add_argument()方法添加参数
```
add_argument('--num_workers',type=int,default=2)
```
---
### lr_scheduler.StepLR()
用于设置学习率按轮次降低，例
```
scheduler = StepLR(optimizer, step_size=30, gamma=0.1)
for epoch in range(100):
    scheduler.step()
    train(...)
    validate(...)

```
---
### 不支持求导
因为某些神秘原因，需要手动设置需要求导
