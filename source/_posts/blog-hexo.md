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

第一阶段，搭建博客，并将文章上传到github上。这个过程比较简单，基本安装完node.js，git客户端后按Hexo官方教程即可实现。这个阶段遇到的问题是执行`hexo d`命令后只将Hexo解析后的代码发布到github上，并没有将博客的配置代码和文章源码上传到github上，通常写博客都会在多台电脑上写，这些配置和博客源码需要在多台电脑同步，按上述操作并不能满足需求，所以到网上继续翻。

第二阶段，删除第一阶段创建的github仓库，重新创建，创建完成后继续创建一个source源码分支，用于保存博客的源代码。这个阶段遇到些困难，细节后面会讲到。

第三阶段，选择NexT主题，并对主题做个性化配置。

所以，利用Hexo在github上搭建博客，如果有保存源码需求的，首先要考虑如何保存源码（可考虑使用github分支或另建一个仓库），然后选择主题，如果想博客漂亮一点可以美化美化，然后就可以开始写博客了。

### 环境

搭建博客过程需安装的环境：

- Windows10
- Node.js
- Git
- hexo: 4.2.0
- NexT

## GitHub Pages 上搭建博客

因为考虑到要保存源码，所以在github上创建名为`用户名.github.io`的仓库之后，立即创建一个source分支用来保存源码。

在 Hexo 中有两份主要的配置文件，其名称都是 `_config.yml`。 其中，一份位于站点根目录下，主要包含 Hexo 本身的配置；另一份位于主题目录下，这份配置由主题作者提供，主要用于配置主题相关的选项。

为了描述方便，在以下说明中，将前者称为 **站点配置文件**， 后者称为 **主题配置文件**。

### github上创建仓库

假定github的用户名为james，在github上新建一个仓库名为`james.github.io`的仓库，创建完成后默认会有一个master分支，我们再创建一个source分支，并将source分支设置为默认分支，用于保存源码，此时master跟source分支都仅只有一个README.md文件。source分支怎么用，后面会解释。

### 本地电脑搭建Hexo 

1. 安装hexo-cli，在Git Bash窗口输入命令：

   ```shell
   $ npm install -g hexo-cli
   ```

2. 初始化博客，新建一个文件夹hexo ，并切换到hexo文件夹，执行命令：

   ```shell
   $ hexo init
   $ ls
   _config.yml    package.json       scaffolds/  themes/
   node_modules/  package-lock.json  source/
   ```

   初始化过程中会将[https://github.com/hexojs/hexo-starter.git](https://github.com/hexojs/hexo-starter.git)代码克隆到hexo文件夹内。此处的_config.yml即为Hexo的 **站点配置文件**。

3. 下载依赖包，执行命令：

   ```shell
   $ npm install
   ```

4. 依赖包下载完成后，就可以打包并执行hexo内置Web服务器：

   ```shell
   $ hexo g # 生成静态文件
   $ hexo s # 启动服务器
   ```

浏览器中输入 [http://localhost:4000/](http://localhost:4000/) 就可以本地预览博客了。

### 更换NexT主题

Hexo主题文件放在themes文件夹下，默认主题为landscape，要更换主题，只需将主题文件放到themes文件内，并修改**站点配置文件**的配置项即可。

1. 克隆NexT主题，在站点根目录下，执行命令：

   ```shell
   $ git clone https://github.com/theme-next/hexo-theme-next.git themes/next
   ```

   主题文件克隆完成后，进入./themes/next目录，将**.git**目录删除，这个目录是NexT主题的git仓库，需要删除，不然会冲突。

2. 修改站点配置文件，将theme字段配置为next。

   ```yaml
   # Extensions
   ## Plugins: https://hexo.io/plugins/
   ## Themes: https://hexo.io/themes/
   theme: next
   ```

3. 重启Hexo服务器

   ```shell
   $ hexo g
   $ hexo s
   ```

浏览器刷新，即可看到效果。

### 部署到github

接下来要做的就是把通过命令`hexo g`生成的静态文件保存到github仓库的master分支下，然后把站点的配置及博客源码保存到source分支下。

第一步，我们先将通过命令`hexo g`生成的静态文件部署到github上，看下效果。

1. 修改**站点配置文件**，配置Hexo生成的静态文件上传的目的地址。

   ```
   deploy:
     type: 'git'
     repo:
       github: https://github.com/james/james.github.io.git //这里即github仓库地址
     branch: master //这里必须是master分支，否则看不到效果
   ```

2. 然后在站点根目录下执行：

   ```shell
   $ hexo g
   $ hexo d
   ```

执行`hexo d`命令完成后，在浏览器输入`https://james.github.io`即可看到部署后的效果。到这里我们已经将本地博客发布到github上了。

第二步，我们还需把源码部署到github的source分支上，用source分支保存源码，方便再各台电脑上同步源码及配置。

在本地电脑再创建一个空文件夹，名为source（随便取），进入source文件夹，将第一步创建的github仓库的source分支克隆到该目录下。因为已经将source分支设置为默认分支，直接克隆得到的就是source分支。

```shell
git clone https://github.com/james/james.github.io.git
```

克隆下来会有一个james.github.io目录，这个目录里面的.git目录就是博客的源码仓库了。接下来的操作都在james.github.io目录下进行，hexo目录的使命已经完成。

我们将刚刚hexo目录里的除**.git**目录（该目录是隐藏的，需要把隐藏目录显示出来）外所有文件都拷贝到james.github.io目录里，然后在该目录创建一个.gitignore文件，.gitignore文件内容如下:

```
.DS_Store
Thumbs.db
db.json
*.log6
node_modules/
public/
.deploy*/
```

即文件里匹配的内容不需要上传到source分支下。创建好.gitignore文件后，我们就可以将源代码上传到source分支了，就是普通的git提交代码的操作，在james.github.io目录下执行：

```shell
$ git add .
$ git commit -m "hexo博客源码分支首次上传"
$ git push
```

执行以上命令后，在github仓库的source分支下，就可以看到提交的代码了。至此，博客源代码已经上传到到github仓库的source分支下了。

那么以后就在james.github.io目录下进行博客编写`hexo new`，生成`hexo g`，预览`hexo s`和发布`hexo d`了，执行`hexo d`后就会将Hexo生成的静态文件部署到github仓库的master分支下。部署完成后，在james.github.io目录下执行git命令：`git add .`，`git commit -m "xxx"`，`git push` ，就可以将当前电脑写的代码部署到source分支下了。

当在另外一台电脑需要写作时，如果是第一次写，将仓库克隆下来，再执行`npm install`安装好依赖库，就可以得到跟原电脑一样的配置了。如果不是第一次写，那直接拉取代码即可完成同步操作了。

## 站点配置

### 配置

Hexo 官网：[https://hexo.io/zh-cn/docs/configuration](https://hexo.io/zh-cn/docs/configuration)

站点配置在**站点配置文件**中配置，即根目录下的_config.yml配置文件，大部分的配置先保持默认即可，有需要修改的时候再改。进行下一步之前先改下网站信息：

```yaml
# Site
title: Monkey Young
subtitle: '路漫漫其修远兮'
description: [阅读, 记录, 前行]
keywords:
author: jychen
language: zh-CN
timezone: Asia/Shanghai
```

### Hexo常用命令

```shell
$ hexo init # 初始化网站
$ hexo new <title> # 新建文章
$ hexo g # hexo g是hexo generate命令的缩写，就是将我们用markdown写的博客文件解析成静态资源文件，解析后的文件在public文件夹下。
$ hexo s # hexo s是hexo server命令的缩写。启动服务器，默认访问网址为： http://localhost:4000/。
$ hexo d # hexo d是hexo deploy命令的缩写，这个命令会将public的文件上传到第一步配置的github仓库的master分支下。
$ hexo clean #清除缓存文件 (db.json) 和已生成的静态文件 (public)。
```

## 主题配置

NexT 官方文档：[http://theme-next.iissnan.com/getting-started ](http://theme-next.iissnan.com/getting-started )

最新版本文档：https://theme-next.js.org/docs/getting-started/

最新版本文档还不支持中文，旧版本的文档已经很全，可直接看旧版本文档。主题配置文件为./themes/next目录下的_config.yml，下面列出一些本博客用到的配置。

```yaml
# 主题
#scheme: Muse
#scheme: Mist
scheme: Pisces
#scheme: Gemini

# 自定义配置文件
custom_file_path:
  #head: source/_data/head.swig
  #header: source/_data/header.swig
  #sidebar: source/_data/sidebar.swig
  #postMeta: source/_data/post-meta.swig
  #postBodyEnd: source/_data/post-body-end.swig
  #footer: source/_data/footer.swig
  #bodyEnd: source/_data/body-end.swig
  #variable: source/_data/variables.styl
  #mixin: source/_data/mixins.styl
  style: source/_data/styles.styl

# 目录设置
menu:
  home: / || fa fa-home 
  about: /about/ || fa fa-user
  tags: /tags/ || fa fa-tags
  categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  movies: /movies/ || fa fa-film
  books: /books/ || fa fa-book
  #schedule: /schedule/ || fa fa-calendar
  #sitemap: /sitemap.xml || fa fa-sitemap
  #commonweal: /404/ || fa fa-heartbeat

# 侧边栏社交账号
social:
  GitHub: https://github.com/cjiayang || fab fa-github
  豆瓣: https://www.douban.com/people/99588562/ || fa fa-film
  知乎: https://www.zhihu.com/people/yang-zi-43-81-44 || fa fa-question-circle
  简书: https://www.jianshu.com/u/129784edb0b6 || fa fa-book

# 文末打赏功能
reward:
  wechatpay: /images/wechatpay.png
  #alipay: /images/alipay.png
  #paypal: /images/paypal.png
  #bitcoin: /images/bitcoin.png

# github角
github_banner:
  enable: false
  permalink: https://github.com/yourname
  title: Follow me on GitHub

# valine评论
valine:
  enable: true
  appid: g2JcDOgE11SRuaMu7LMFSMAA-gzGzoHsz # Your leancloud application appid
  appkey: tE9EIqQRVvHB9X3uQyKkOCdI # Your leancloud application appkey
  notify: false # Mail notifier
  verify: false # Verification code
  placeholder: Just go go # Comment box placeholder
  avatar: mm # Gravatar style
  guest_info: nick,mail,link # Custom comment header
  pageSize: 10 # Pagination size
  language: zh-cn # Language, available values: en, zh-cn
  visitor: false # Article reading statistic
  comment_count: true # If false, comment count will only be displayed in post page, not in home page
  recordIP: false # Whether to record the commenter IP
  serverURLs: # When the custom domain name is enabled, fill it in here (it will be detected automatically by default, no need to fill in)
  #post_meta_order: 0
  
# 百度统计
baidu_analytics: xxxxxxxxxxxxxxxxxxx

# leancloud访问统计
leancloud_visitors:
  enable: true
  app_id: xxxxx-xxxxxxx
  app_key: xxxxxxxxxxxx
  # Required for apps from CN region
  server_url: # <your server url>
  # Dependencies: https://github.com/theme-next/hexo-leancloud-counter-security
  # If you don't care about security in leancloud counter and just want to use it directly
  # (without hexo-leancloud-counter-security plugin), set `security` to `false`.
  security: true

# 不算子统计
busuanzi_count:
  enable: true
  total_visitors: true
  total_visitors_icon: fa || fa-user
  total_views: true
  total_views_icon: fa || fa-eye
  post_views: true
  post_views_icon: fa || fa-eye

# 本地搜索功能
local_search:
  enable: true
  # If auto, trigger search by changing input.
  # If manual, trigger search by pressing enter key or search button.
  trigger: auto
  # Show top n results per article, show all results by setting to -1
  top_n_per_article: 1
  # Unescape html strings to the readable one.
  unescape: false
  # Preload the search data when the page loads.
  preload: false
  
# 文章末尾添加“本文结束”标记
passage_end_tag:
  enabled: true
```





## 参考

1. [Hexo 官方文档](https://hexo.io/zh-cn/docs/)
2. [NexT 官方文档 ](http://theme-next.iissnan.com/getting-started )
3. [Hexo 搭建个人博客文章汇总](https://tding.top/archives/aad98408.html)
4. [尝试折腾了下用 Hexo-Next-Theme 搭建的博客](https://leay.net/2020/03/23/hexo-next/)
5. [最全Hexo博客搭建](https://www.simon96.online/2018/10/12/hexo-tutorial/)