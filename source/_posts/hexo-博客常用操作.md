---
title: hexo 博客常用操作
mathjax: true
tags: hexo博客
categories: 笔记
abbrlink: 8
date: 2026-08-20 16:37:27
hidden:
update:
---

这里记录 hexo 博客的几个常用操作。

<!-- more -->
# 新建文章
使用 `hexo new "文章名"` 。随后会在 `/source/_posts` 文件夹下生成 `文章名.md` 文件。

# 博客部署
在对博客进行修改后，先后输入 `hexo clean` `hexo g` `hexo s` 在本地部署博客并在 `localhost:4000` 中进行预览。之后可以用 `hexo d` 命令将修改后的博客部署到远端 github page 上。

# 博客备份
部署博客后可以将文件备份到远端仓库上。
使用 `git add .` `git commit -m "修改内容"` `git push` 命令。

# 对本地文件的修改

这里给出本地文件夹结构。
```
my-blog
│  .gitignore
│  .npmignore
│  db.json
│  package-lock.json
│  package.json
│  README
│  README.md
│  _config.next.yml
│  _config.yml
│  
├─.deploy_git        
├─.github
├─node_modules      
├─public        
├─scaffolds
│      
└─source
    ├─about
    │      
    ├─categories
    │      
    ├─images
    │      
    ├─links
    │      
    ├─tags
    │      
    ├─_data
    │      
    └─_posts            

```
其中，大部分博客的修改都在 `/_config.yml` 和 `/_config.next.yml` 上进行。而 `/node_modules` 文件夹中的修改会被更新所覆盖，因此下面的修改都不会在 `/node_modules` 文件夹中进行。


## 修改黑暗模式
黑暗模式插件的配置文件位于 `/node_modules/hexo-next-darkmode` 文件夹中，但因前文所述原因，不建议在此文件夹中进行更改。建议在 `/source/_data/styles.styl` 文件上进行更改。
例如：想要更改黑暗模式下单行代码块的颜色，可以在 `styles.styl` 文件中增加 
```styl
.darkmode--activated code {
    color: ;/* 更改字体颜色 */
    background: ;/* 更改背景颜色 */
    ...
}
```
对于其他想要在黑暗模式下更改的样式，也可以用 `.darkmode--activated` 后加要更改的样式进行更改。

