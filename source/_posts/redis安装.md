---
title: CentOS7安装Redis
date: 2020-10-08 19:40:26
categories:
  - 中间件
tags:
  - Redis
---

本文介绍CentOS7中Redis的安装实践

<!--more-->

## yum安装

如果能联网，yum安装最快捷，但是yum源中Redis版本不是最新的，所以如果想安装较新版本的Redis需要使用其他方式安装。

1. 安装EPEL仓库

   ```shell
   yum install epel-release
   yum update
   ```

2. 安装Redis

   ```shell
   yum -y install redis
   ```

3. 启动Redis

   ```shell
   systemctl start redis
   ```

4. 修改配置文件

   ```shell
   vim /etc/redis.conf
   ```

## 源码安装

当需要较新版本的Redis时，可以使用源码安装，下面以redis-6.0.8为例介绍Redis源码安装。

### 安装准备

Redis安装依赖gcc，CentOS系统默认的gcc版本为4.8.5，需要升级到5.3版本以上；执行`make test`
时需要依赖tcl库，需要提前安装tcl库

```shell
yum -y install gcc tcl
yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils
# 永久启用最新版本gcc
echo "source /opt/rh/devtoolset-9/enable" >> /etc/profile
```

### 下载Redis

```shell
wget http://download.redis.io/releases/redis-6.0.8.tar.gz
```

### 编译安装

`make install`命令会将redis/src下的一些脚本拷贝到/usr/local/bin/目录下，因为/usr/local/bin/目录已经在path环境变量中配置了，所以执行此命令的目的是在任何目录下都可以直接启动停止redis。

```shell
tar -zxf redis-6.0.8.tar.gz -C /usr/local/
cd /usr/local/redis-6.0.8
make MALLOC=libc
make test # 可选择执行
make install
```

### 后台运行

安装完后执行`redis-server`启动是前台运行，需要修改redis.conf配置文件。在/etc目录下创建redis目录，将源码根目录下的redis.conf文件拷贝到/etc/redis目录，并改名为6379.conf。

```shell
mkdir /etc/redis/
cp redis.conf /etc/redis/6379.conf
vim /etc/redis/6379.conf
```

找到`daemonize no`并将其改为`daemonize yes`。

```
# By default Redis does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
daemonize yes
```

server启动的时候指定配置文件，就后台启动了。

```shell
redis-server  /etc/redis/6379.conf
```

### 以服务方式启动

修改配置文件可以解决redis前台启动的问题，但是每次启动都指定配置文件，非常不方便。下面介绍以服务的方式启动关闭redis，简化redis服务的操作。

首先切换到redis源码目录

```shell
cd redis-6.0.8
```

将utils目录下的redis_init_script文件拷贝到/etc/init.d目录下，并保存为redis

```shell
cp utils/redis_init_script /etc/init.d/redis_6379
```

创建redis.service

```shell
vim /etc/systemd/system/redis.service
```

redis.service文件输入以下内容

```
[Unit]
Description=Redis on port 6379
[Service]
Type=forking
ExecStart=/etc/init.d/redis_6379 start
ExecStop=/etc/init.d/redis_6379 stop
[Install]
WantedBy=multi-user.targe
```

更新服务

```shell
systemctl enable redis
#务必要进行reload
systemctl daemon-reload
#在centos7下可用service命令启动
service redis start
#查看服务状态
service redis status
#在低于centos7版本下用systemctl
systemctl start redis
```

