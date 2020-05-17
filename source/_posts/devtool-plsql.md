---
layout: post
title:  "PLSQL12使用"
date:   2019-11-06 19:06:18 +0800
categories: 
  - devtool
tags: 
  - 开发工具 
  - PLSQL12
---

    本文对PLSQL12.0.7版本的下载，安装，汉化，和日常使用技巧做记录。
<!--more-->



###  1 PLSQL下载安装及汉化
#### 1.1 下载地址

- PLSQL官网下载：[ PL/SQL Developer - Registered Download ](https://www.allroundautomations.com/bodyplsqldevreg.html)

- 汉化可执行文件下载：[ PL/SQL Developer - Language Packs ]( https://www.allroundautomations.com/plsqldevlang/120/index.html )

  ![plsql_pic0.png](https://i.loli.net/2019/11/13/IN8AicbCqn3Gup9.png)

#### 1.2 安装及汉化

* 下载 12.0.7x64位版本  [plsqldev1207x64.msi](https://www.allroundautomations.com/files/plsqldev1207x64.msi)  ，双击安装，安装时选择Enter licence information输入注册码如下：

```
product code： 4vkjwhfeh3ufnqnmpr9brvcuyujrx3n3le
serial Number：226959
password: xs374ca
```

  ![devtool_plsql_gif1.gif](https://i.loli.net/2019/11/14/3ThPfsB7IboXJiV.gif)



* 双击chinese.exe，将目录选在上一步PL/SQL的安装目录下，完成后会在安装目录下生成Chinese.lang文件。启动PL/SQL，免登录进入，选择Preferences -> Appearance，语言选择Chinese.lang，点击Apply，返回主页面之后即可汉化完成

  ![devtool_plsql_gif2.gif](https://i.loli.net/2019/11/14/PwhSCk5amAXeZ4l.gif)



### 2 Oracle客户端下载

#### 2.1 下载Oracle客户端

Oracle客户端下载页面：[Oracle Instant Client Downloads]( https://www.oracle.com/database/technologies/instant-client/downloads.html )，

点击选择 [Instant Client for Microsoft Windows (x64)](https://www.oracle.com/database/technologies/instant-client/winx64-64-downloads.html) ，选择与自己安装数据库相匹配的客户端版本， 下载Instant Client Package - Basic 包，如我的Oracle版本是Version 11.2.0.4.0，则下载 [instantclient-basic-windows.x64-11.2.0.4.0.zip](https://www.oracle.com/database/technologies/instant-client/winx64-64-downloads.html#license-lightbox) ，解压到plsql安装的同级目录（也可其他目录，主要是好找）

  ![devtool_plsql_gif3.gif](https://i.loli.net/2019/11/14/QRjJ7WOXFbyKSTY.gif)


#### 2.2 PL/SQL配置Oracle客户端

* 免登录进入PLSQL，点击首选项 -> 连接，Oracle注目录名和OCI库选择Oracle客户端解压目录下的oci.dll文件，点击应用，重启后即可在页面连接登录
  ![plsql_pic9.png](https://i.loli.net/2019/11/14/JFenlED5PTvYUI4.png)

### 3 PL/SQL配置

#### 3.1 视图配置

- 切换到`视图`选项卡，勾选连接和对象浏览器，勾选单文档，调整窗口布局后，点击保存布局。

  ![devtool_plsql_pic1.png](https://i.loli.net/2019/11/14/Eobm1IFcsnwgf6a.png)

#### 3.2 连接配置

* 切换到`配置`选项卡，点击`连接`按钮，添加经常使用的连接信息，不同连接可以选不同的颜色进行区分。重启PL/SQL之后，就可以在登录界面上看到已经填写好的连接，直接双击即可登录，不用每次输入连接信息。

  ![devtool_plsql_pic4.png](https://i.loli.net/2019/11/18/RphNMajLwf8rq9e.png)

  ![devtool_plsql_pic5.png](https://i.loli.net/2019/11/18/KdzcYvXGmDTJnRg.png)

#### 3.3 对象浏览器配置

* `首选项 -> 用户界面 - > 浏览器`，对象浏览器各个常用的对象如表，视图，函数等可以调整顺序和改变文件夹的颜色，方便使用。也可切换到配置选项卡，点击`对象浏览器文件夹`按钮，进行配置。

  ![devtool_plsql_gif6.gif](https://i.loli.net/2019/11/14/UAYViHs7cDEnPr8.gif)

#### 3.4 自动代码补全配置

* 可以为常用的关键字和查询语句设置代码补全，以下代码供参考：

  ```
  s = SELECT
  f = FROM
  w = WHERE
  o = ORDER BY
  d = DELETE
  sf = SELECT * FROM
  df = DELETE FROM
  sc = SELECT COUNT (*) FROM
  ```
  
  
  
  ![devtool_plsql_gif5.gif](https://i.loli.net/2019/11/14/vOwJnzimt2LVxP8.gif)

#### 3.5 美化器配置

* 配置美化格式，配置完后可以保存成文件，在其他机子也可以用

  ![devtool_plsql_pic2.png](https://i.loli.net/2019/11/14/IrNvxQJbOqyLRAY.png)

#### 3.6 快捷键配置

* `首选项 -> 用户界面 -> 键配置`设置常用的快捷键，如打开SQL窗口和命令窗口的快捷键
  ![devtool_plsql_pic3.png](https://i.loli.net/2019/11/14/x32vGRN6PeLaOKu.png)

#### 3.7 工具使用

* 在工具选项卡常用的有导出用户对象，比较用户对象，导出表，比较表数据的功能，功能都比较强大。

### 参考文章

---

* [PLSQL Developer 12.0.7连接Oracle12c数据库]( https://blog.csdn.net/sl1992/article/details/80489413 )
* [ PLSQL Developer 12 注册码 ]( https://www.cnblogs.com/shizilukou123/p/9149358.html )