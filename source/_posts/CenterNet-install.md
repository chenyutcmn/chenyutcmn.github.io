---
title: CenterNet_install
date: 2021-01-14 10:44:03
tags: CenterNet
categories: note
---
CenterNet 部署环境搭建
<!--more-->

## CenterNet 环境搭建
#### make命令error
以python setup.py install命令代替
#### ImportError: torch.utils.ffi is deprecated. Please use cpp extensions instead
将pytorch版本退回到0.4.1 ps:老老实实按readme来，别作妖
#### DCNv2 编译问题
在make.sh文件最上方添加如下语句
```
export CUDA_PATH=/usr/local/cuda/
export CXXFLAGS="-std=c++11"
export CFLAGS="-std=c99"

export PATH=/usr/local/cuda-9.0/bin${PATH:+:${PATH}}
export CPATH=/usr/local/cuda-9.0/include${CPATH:+:${CPATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

cd src/cuda
```
#### ModuleNotFoundError: No module named 'skbuild'
pip install --upgrade pip
