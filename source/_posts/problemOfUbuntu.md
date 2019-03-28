---
title: ubuntu下设置win10引导
date: 2018-7-5
categories: coding
tags: 
    - ubuntu 
    - boot
---
装了双系统后win10不能正常启动，需设置引导项。
<!--more-->
## 问题描述
装了双系统后win10不能正常启动

## 解决方案
在ubuntu环境下使用grub加载win10引导

### 查看系统盘情况
```
sudo fdisk -l (小写的L)
```
输出
```
 设备       启动      起点          终点        块数      Id  系统  
/dev/sda1            2048       205593844   102795898+  83  Linux  
/dev/sda2   *   205594624       304197631    49301504    7  HPFS/NTFS/exFAT  
/dev/sda3       304199678       312580095     4190209    f  W95 扩展 (LBA)  
/dev/sda5       304199680       312580095     4190208   82  Linux 交换 / Solaris  
```
自行辨认哪个是win启动盘  
### 挂载
```
mkdir /media/tempdir （用来挂载sda1的，就是创建一个tempdir，名字什么的自己定，操作前获取root权限）  
mount /dev/sda1 /media/tempdir （将sda1挂载在tempdir下）  
grub-install --root-directory=/media/tempdir /dev/sda （重新安装grub2到硬盘的主引导记录（mbr））  
```
操作成功出现：Installation finished.No Error Reported.  重启电脑
### 更新
``` 
sudo update-grub2  
``` 
输出
```
hellowd93@hellowd93-PC:~$ sudo update-grub2  
[sudo] password for hellowd93:   
Generating grub configuration file ...  
Warning: Setting GRUB_TIMEOUT to a non-zero value when GRUB_HIDDEN_TIMEOUT is set is no longer supported.  
Found linux image: /boot/vmlinuz-3.13.0-37-generic  
Found initrd image: /boot/initrd.img-3.13.0-37-generic  
Found memtest86+ image: /boot/memtest86+.elf  
Found memtest86+ image: /boot/memtest86+.bin  
Found Windows 10 (loader) on /dev/sda2  
done  
hellowd93@hellowd93-PC:~$   
```
done表示操作成功 重启选择win10系统解决问题。
### 学如逆水行舟,且行且记
