---
title: hexo博客同步方法
date: 2021-12-13 02:02:18
tags: [教程]
---

# hexo博客同步方法

今天终于实现了随时写博客的想法, 对 git 的使用也有了更深的了解. 于是写一篇博客来记录, 以免以后忘记如何同步.

## 前置
- git
- node.js
- 一个 hexo 博客

## 开整

### 上传

首先需要在已经有 hexo 博客的电脑 ( 记为 A 端 ) 的博客目录下使用一下命令
```vim
git init #git 命令初始化
git remote add origin https://github.com/用户名/你的GitHub用户名.github.io.git #添加仓库地址
git checkout -b 分支名 #新建分支并切换到新建的分支
git add . #添加所有本地文件到git
git commit -m "这里填写你本次提交的备注，内容随意" #git提交
git push origin 分支名 #文件推送到hexo分支
```

需要注意的是, 由于 GitHub 政策变更现在已经不能通过地址来 push, 需要 ssh 完成. 因此上文地址应改为 GitHub 仓库的 ssh , 可以从仓库页面 -> code -> ssh 复制. 分支名可以自己定, 一般为 hexo.

### 下载

在想要写博客的电脑 ( B 端 ) 使用如下命令

```vim
git clone -b 分支名 https://github.com/用户名/你的GitHub用户

sudo npm install -g hexo-cli #安装hexo,注意这里不需要hexo初始化,否则之前的hexo配置参数会重置
sudo npm install #安装依赖库
sudo npm install hexo-deployer-git #安装git部署相关配置
```
博客便从 GitHub 上同步至了本地, 然后重新配置 hexo 写新的博客. 写完后再通过以下命令上传至 GitHub.

```vim
git add .
git commit -m "这里填写你本次提交的备注，内容随意"
git push origin 分支名
```
在A端使用

```vim
git pull
```

这样，你的数据就全部同步到 A 端了.