---
title: 虚拟机安装RedHat6.5实践
date: 2020-08-10 18:53:38
categories:
  - Linux
tags:
  - RedHat
  - VMware
---

本文介绍在虚拟机下安装RedHat6.5步骤

<!--more-->

## 准备

虚拟机创建已在另一篇文章介绍过，安装前需准备redhat6.5的iso镜像文件，可以到开源镜像站下载相应版本。

1. VMware虚拟机15.1.0
2. rhel-server-6.5-x86_64-dvd.iso镜像文件

## 安装

### 虚拟机配置镜像文件

编辑虚拟机设置  → CD/DVD → 使用ISO镜像文件，点击浏览，选择rhel-server-6.5-x86_64-dvd.iso镜像文件所在位置

![vmware-config](vmware-config.png)

### 安装步骤

1. 选择Install or upgrade an existing system
   ![step1](step1.png)

2. 点击Skip
   ![step2](step2.png)

3. 语言默认选择English
   ![step3](step3.png)

4. 键盘默认选U.S. English
   ![step4](step4.png)

5. 选择Basic Storage Device
   ![step5](step5.png)

6. 选择 Yes, discard any data
   ![step6](step6.png)

7. 主机名自定义，点击配置网络，选择eth0网卡，点击Edit
   ![step7](step7.png)

8. 勾选Connect automatically，Ipv4 Settings选项卡中，Method选择Manual手动配置
   Address填写局域网内未使用的IP，点击应用
   ![step8](step8.png)

9. 时区选择上海，去掉UTC勾选
   ![step9](step9.png)

10. 填写系统root用户密码
    ![step10](step10.png)

11. 自定义安装
    ![step11](step11.png)

12. 创建boot分区

    点击Free → 点击Create → 选择 Standard Partition
    ![step12](step12.png)

13. 创建swap分区
    点击Free → 点击Create → 选择 Standard Partition
    ![step13](step13.png)

14. 创建root分区
    点击Free → 点击Create → 选择 Standard Partition → 选择 Fill to maximum allowable size
    ![step14](step14.png)

15. 格式化系统
    点击Format，在弹窗点击Write changes to disk
    ![step15](step15.png)
    
16. 默认，点击next
    ![step16](step16.png)
    
17. 选择Desktop桌面版安装
    ![step17](step17.png)
    
18. 安装完成点击Reboot
    ![step18](step18.png)

### 首次启动配置

1. 欢迎页直接点击Forward
   ![step19](step19.png)
2. 许可证选择Yes, I agree to the License Agreement，点击Forward
   ![step20](step20.png)
3. 选择稍后注册，点击Forward，弹窗点击Register Later，继续点击Forward
   ![step21](step21.png)
4. 如果不建用户直接点击Forward，在弹窗中点击Yes确认
   ![step22](step22.png)
5. 日期和时间，点击Forward
   ![step23](step23.png)
6. 点击Finish，在弹窗中点击Yes和OK，系统重启。
   ![step24](step24.png)

### 验证

1. root用户登陆
   ![step25](step25.png)
2. ping www.baidu.com 如果网络能通则成功
   ![step26](step26.png)

