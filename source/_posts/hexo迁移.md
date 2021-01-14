---
title: hexo迁移
date: 2021-01-14 11:18:56
tags: hexo
categories: note
---
换设备后hexo的分支迁移
<!--more-->
## Tips
1. nodejs使用12.20版本，太高报错
2. `Deployer not found: git` 的解决办法 `npm install --save hexo-deployer-git`
3. 需配置全局git账户信息
## 迁移过程
1. 安装nodejs
2. 安装hexo `npm install -g hexo-cli`
3. 新建文件夹temp`cd temp ; hexo init` 初始化hexo文件目录
4. `git clone -b branch_name http://example` 下载备份
5. 复制所有文件(包含.git文件夹)至temp目录
6. `git clone https://github.com/levblanc/hexo-theme-aero-dual.git` 下载主题，更名为aero-dual，更换主题图片，config文件修改对应设置
7. `hexo new article_title` 编辑新文章
8. 将更新推送至git hexo分支

```
git add .
git commit -m "修改配置/发表文章"
git push origin hexo
```
9. 静态文件推送master分支

```
hexo clean 
hexo g
hexo d
```