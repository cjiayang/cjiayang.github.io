---
title: 利用Hexo在GitHub Page搭建博客
date: 2020-05-21 19:16:07
tags:
  - hexo
  - next
---

Hexo 是一个快速、简洁且高效的博客框架。本文记录利用Hexo在GitHub Pages 上搭建博客的过程，以备忘。

<!-- more -->

## 概述

为了搭建博客，经历了一番波折，也翻了好多文章，一些值得参考的文章会在文章后面列出。总结起来博客搭建经历了以下几个过程。

第一阶段，搭建博客，并将文章上传到github上。这个过程比较简单，基本安装完node.js，git客户端后按hexo官方教程即可实现。这个阶段遇到的问题是执行`hexo d`命令后只将hexo解析后的代码发布到github上，并没有将博客的配置代码和文章源码上传到github上，通常写博客都会在多台电脑上写，这些配置和博客源码需要在多台电脑同步，按上述操作并不能满足需求，所以到网上继续翻。

第二阶段，删除第一阶段创建的github仓库，重新创建，创建完成后继续创建一个source源码分支，用于保存博客的源代码。这个阶段遇到些困难，细节后面会讲到。

第三阶段，选择next主题，并对主题做个性化配置。

所以，利用hexo在github上搭建博客，如果有保存源码需求的，首先要考虑如何保存源码（可考虑使用github分支或另建一个仓库），然后选择主题，如果想博客漂亮一点可以美化美化，然后就可以开始写博客了。

## 环境

搭建博客过程需安装的环境：

- Windows10
- Node.js
- Git

## GitHub Pages 上搭建博客

因为考虑到要保存源码，所以在github上创建仓库之后，创建一个source分支。

### github上创建仓库

在github上新建一个仓库名为`用户名.github.io`的仓库，创建完成后默认会有一个master分支，我们再创建一个source分支，用于保存源码。source分支怎么用，后面会解释。

### 本地电脑搭建hexo

安装hexo-cli，在Git Bash窗口输入命令：

```shell
$ npm install -g hexo-cli
```

初始化博客，新建一个博客文件夹hexo，并切换到hexo文件夹，执行命令：

```shell
$ hexo init
$ ls
_config.yml    package.json       scaffolds/  themes/
node_modules/  package-lock.json  source/
```

初始化过程中会将https://github.com/hexojs/hexo-starter.git代码Clone到hexo文件夹内。

下载依赖包，执行命令：

```shell
$ npm install
```

依赖包下载完成后，就可以打包并执行hexo内置Web服务器：

```shell
$ hexo g # 生成静态文件
$ hexo s # 启动服务器
```

浏览器中输入 http://localhost:4000/ 就可以本地预览博客了。

### 更换NexT主题

hexo主题文件放在themes文件夹下，默认主题为landscape，要更换主题，只需将主题文件下载到themes文件内，并修改配置文件即可。