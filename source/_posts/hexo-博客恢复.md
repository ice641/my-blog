---
title: hexo 博客恢复
mathjax: true
tags: hexo博客
categories: 笔记
hidden: false
abbrlink: 7
date: 2026-08-20 09:53:17
update:
---
在此记录一下这个博客在经过 4 年的长草期后重新恢复的过程。

这里省去博客的云端备份环节，从换新电脑的部分开始。

<!--more-->

# 配置环境

hexo 博客部署依赖于 git 和 node。

git : [https://git-scm.com/install/](https://git-scm.com/install/)

node : [https://nodejs.org/zh-cn/download](https://nodejs.org/zh-cn/download)

安装好后按 `Win + R` 键，输入 `cmd`。弹出终端后输入 `git -v`、`node -v`和`npm -v`，如果显示版本号，那么代表环境配置完成。

# 连接远端备份仓库
在要放置博客内容的文件夹内（下用 my-blog 表示）打开 git bash，输入 
```
git config --global user.name "用户名"
git config --global user.email "邮箱地址"
```
来连接 github。
接着输入
```
ssh-keygen -t rsa -C "邮箱地址"
```
获得一个在 C 盘用户文件夹中 `.ssh` 文件夹。里面有好几个文件，打开 `id_rsa.pub`，将里面的内容全选复制。
打开 github 中的设置界面，打开 SSH and GPG keys 一栏，点击 `new SSH key`，将刚刚复制的内容粘贴后点击添加即可完成本地与远端的 ssh 连接。

# 克隆备份仓库
输入
```
git clone https://github.com/用户名/my-blog.git
```
从远端克隆博客备份。

输入以下命令会下载所需要的依赖。
```
npm install
```

# 博客部署
输入以下命令安装 hexo。
```
npm install -g hexo -cli
```
安装好 hexo 后可以尝试 `hexo clean`、`hexo g`、`hexo s` 来部署博客并在本地预览。如果此时出错，有可能是备份的文件夹有损坏或者 hexo 更新后备份的博客版本低而缺少相关内容。此时可以先把克隆下来的部分移出文件夹，（在确保 `/my-blog` 文件夹中无任何文件的情况下） 尝试 `hexo init` 来获得最新的 hexo 初始化文件，并重新把备份文件复制到 `/my-blog` 中覆盖初始化的文件，并再次尝试  `hexo clean` `hexo g` `hexo s` 部署博客。

如果以上任一步骤出现报错，请尝试将报错信息复制给自己信任的 AI 让其帮助分析）

# 博客的备份
在 `hexo d` 将博客部署到 github 上后输入 `git add .` `git commit -m "更新内容"` `git push` 来备份自己的博客。