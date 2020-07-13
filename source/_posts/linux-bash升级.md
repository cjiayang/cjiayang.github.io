---
title: Linux升级bash版本
date: 2020-07-13 16:36:33
categories: 
  - Linux
tags:
	- Linux
	- bash
---

因为bash低版本存在漏洞，需要升级版本。本文介绍在操作系统redhat6.5下将系统自带的bash-4.1.2版本升级到bash-5.0。

<!--more-->

## bash漏洞简介

bash4.3及以下版本的bash存在远程执行代码漏洞，包括但不限于Redhat、CentOS、Ubuntu、Debian、Fedora 等平台。

- CVE漏洞名称：CVE-2014-6271

- 中文漏洞名称：破壳

  **验证：**

  > 通过普通用户登录主机，在bash shell下执行：
  >
  > ```shell
  > env x='() { :;}; echo vulnerable' bash -c "echo this is a test"
  > ```
  >
  > > 如果输出 ：this is a test 
  > >
  > > 则不存bash在远程执行代码漏洞;
  >
  > > 如果输出：
  > > vulnerable
  > > this is a test
  > > 则存在bash远程执行代码漏洞

针对以上漏洞可将bash升级到4.3以上版本，下面说明升级步骤。

## 升级bash版本

1. **下载最新版bash**
   bash下载地址：http://ftp.gnu.org/gnu/bash/
   本文下载的是bash5.0版本： http://ftp.gnu.org/gnu/bash/bash-5.0.tar.gz

2. **查看当前系统bash版本**

   ```shell
   [root@node1 ~]# /bin/bash -version
   GNU bash, version 4.1.2(1)-release (x86_64-redhat-linux-gnu)
   Copyright (C) 2009 Free Software Foundation, Inc.
   License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
   
   This is free software; you are free to change and redistribute it.
   There is NO WARRANTY, to the extent permitted by law.
   ```

   可以知道当前版本为4.1.2

3. **升级bash**
   上传bash版本包到服务器，解压升级

   ```shell
   [root@node1 ~]# tar -zxf bash-5.0.tar.gz 
   [root@node1 ~]# cd bash-5.0
   [root@node1 bash-5.0]# ./configure && make && make install
   ```

   因为bash默认是安装在/usr/local/bin/目录下，所以需要创建一个链接到 /bin/目录下！

   ```shell
   [root@node1 bash-5.0]# mv /bin/bash /bin/bash.bak
   [root@node1 bash-5.0]# ln -s /usr/local/bin/bash /bin/bash
   ```

4. **验证**

   ```shell
   [root@node1 ~]# /bin/bash -version
   GNU bash，版本 5.0.0(1)-release (x86_64-pc-linux-gnu)
   Copyright (C) 2019 Free Software Foundation, Inc.
   许可证 GPLv3+: GNU GPL 许可证第三版或者更新版本 <http://gnu.org/licenses/gpl.html>
   
   本软件是自由软件，您可以自由地更改和重新发布。
   在法律许可的情况下特此明示，本软件不提供任何担保。
   [root@node1 bash-5.0]# env x='() { :;}; echo vulnerable' bash -c "echo this is a test"
   this is a test
   ```

   现在bash版本已经是 5.0.0，bash漏洞也已修复