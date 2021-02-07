---
title: CenterNet训练与阅读笔记
date: 2021-01-16 11:25:26
tags: CenterNet
categories: note
---
CenterNet训练与阅读笔记
<!--more-->
## 训练
1. 在src/lib/datasets/dataset下新建example.py，文件名同数据集名
2. 复制coco.py文件，修改类名，num_classes，数据集均值方差
3. 25行修改为split == val
4. 将数据集加入src/lib/datasets/dataset_factory
5. 修改/src/lib/opts.py，包括修改'--dataset'参数的默认值，修改ctdet任务的默认数据集
6. 修改src/lib/utils/debugger.py文件，45行左右添加自己数据集，460行左右添加自己的数据类别
## 源码阅读

## tips
1. 新建_init_import.py文件，在需要使用import的文件中 import _init_import 避免了每个文件都写sys.path.insert(0, path)

```
import os.path as osp
import sys

def add_path(path):
    if path not in sys.path:
        sys.path.insert(0, path)

this_dir = osp.dirname(__file__)

# Add lib to PYTHONPATH
lib_path = osp.join(this_dir, 'lib')
add_path(lib_path)

```