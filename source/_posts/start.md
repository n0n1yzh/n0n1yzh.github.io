---
title: Hexo相关指令及上传中的一些问题和解决(长期更新)
date: 2020-11-02 20:03:16
updated:
tags:
categories: 总之就是很杂很乱
keywords:
description:
top_img: https://s3.ax1x.com/2020/11/22/D8rEH1.jpg
comments:
cover: false
toc:
toc_number:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax: true
katex: 
aplayer:
highlight_shrink:
aside: false
plugins: markdown-it-emoji
---

## 这是一个开篇

### Hexo 相关指令

> 安装 hexo 在新建文件夹下打开 git 命令框

```
$ npm install -g hexo
```

> 初始化 新建一个网站

```
$ hexo init [floder]
```

之后启动服务(npm install --> hexo server --> hexo clean --> hexo generate --> hexo deploy) 可以本地访问页面

> 创建文章

```
$ hexo new post "title"
```

在 \ source \ _posts 路径中找到新建的 md 文件  如果重命名会重新生成一个 md 文件

> 生成静态文件

```
$ hexo generate
```

> 部署到远程仓库

```
$ hexo deploy
```

> 生成静态文件后直接部署网站

```
$ hexo g -d
```

> 部署到服务器 在本地查看更新内容

```
$ hexo server
```

> 重设端口 端口占用时用

```
$ hexo s -p 4000
```

仅修改此次 $ hexo s 的端口

> 清楚缓存文件和已生成的静态文件

```
$ hexo clean
```

在某些情况（尤其是更换主题后），如果发现站点的更改无论如何也不生效，可能需要运行该命令。

```
$ hexo clean 
$ hexo g 
$ hexo d
```

部署更新

> :alarm_clock: 在某些情况下会出现 Spawn failed 的报错 直接删除根目录的 .deploy_git 文件重新部署可以解决。

