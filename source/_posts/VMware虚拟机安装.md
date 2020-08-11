---
title: VMware虚拟机安装及使用
date: 2020-07-17 11:28:19
categories:
  - 开发工具
tags:
  - VMware
---

[VMware Workstation Pro](https://www.vmware.com/products/workstation-pro.html) 是在单个Linux或Windows系统上运行多个操作系统的行业标准。VMware是一款非常好用的虚拟机软件，在软件开发，测试中经常会使用到。本文介绍VMware15.1.0版本在Windows10环境下的安装和使用。

<!--more-->

## 资源下载

VMware下载完后，直接安装，然后输入注册码即可使用。官网文档说的比较全面，不过太细了，遇到具体问题再过来翻一翻。

- 官网下载地址：[VMware Workstation Pro 15.1.0 Build 9474260](https://download3.vmware.com/software/wkst/file/VMware-workstation-full-15.1.0-13591040.exe)

- 官网文档：[使用 VMware Workstation Pro](https://docs.vmware.com/cn/VMware-Workstation-Pro/15.0/com.vmware.ws.using.doc/GUID-0EE752F8-C159-487A-9159-FE1F646EE4CA.html)

- 注册码：

  >UG5J2-0ME12-M89WY-NPWXX-WQH88
  >GA590-86Y05-4806Y-X4PEE-ZV8E0
  >YA18K-0WY8P-H85DY-L4NZG-X7RAD
  >UA5DR-2ZD4H-089FY-6YQ5T-YPRX6
  >B806Y-86Y05-GA590-X4PEE-ZV8E0
  >ZF582-0NW5N-H8D2P-0XZEE-Z22VA

## VMware工作界面

![用户界面](interface.png)

- 主页：主页显示了VMware的三大功能，也是最常使用的三个功能
- 视图：可以选择多个视图查看虚拟机
- 菜单：提供虚拟机的管理菜单

## 创建虚拟机

文件 → 新建虚拟机

1. 选择自定义 → 下一步
   ![step1](step1.png)
2. 选择WorkStation15.x → 下一步
   ![step2](step2.png)
3. 选择稍后安装操作系统 → 下一步
   ![step3](step3.png)
4. 选择操作系统，以及操作系统版本
   ![step4](step4.png)
5. 填写虚拟机名称，选择虚拟机文件存放位置
   ![step5](step5.png)
6. 处理器配置默认
   ![step6](step6.png)
7. 虚拟机内存可按推荐内存填写，视实际情况配置
   ![step7](step7.png)
8. 选择桥接网络
   ![step8](step8.png)
9. I/O控制器类型默认
   ![step9](step9.png)
10. 磁盘类型默认
11. 创建新虚拟磁盘
    ![step10](step11.png)
12. 磁盘容量20-50G，视需求而定
    ![step11](step12.png)
13. 磁盘文件默认
    ![step12](step13.png)
14. 自定义硬件，删除声卡和打印机，点击完成
    ![step13](step14.png)

## 虚拟机网络

虚拟机网络主要有三种模式，桥接模式网络连接、NAT 和仅主机模式网络连接。最常使用也最简单的就是桥接模式。

### 桥接模式网络连接

桥接模式网络连接通过使用主机系统上的网络适配器将虚拟机连接到网络。如果主机系统位于网络中，桥接模式网络连接通常是虚拟机访问该网络的最简单途径。

当您将 Workstation Pro 安装到 Windows 或 Linux 主机系统时，系统会设置一个桥接模式网络 (VMnet0)。可以参见[配置桥接模式网络连接](https://docs.vmware.com/cn/VMware-Workstation-Pro/15.0/com.vmware.ws.using.doc/GUID-BAFA66C3-81F0-4FCA-84C4-D9F7D258A60A.html#GUID-BAFA66C3-81F0-4FCA-84C4-D9F7D258A60A)。

### NAT 模式网络连接

使用 NAT 模式网络时，虚拟机在外部网络中不必具有自己的 IP 地址。主机系统上会建立单独的专用网络。在默认配置中，虚拟机会在此专用网络中通过 DHCP 服务器获取地址。虚拟机和主机系统共享一个网络标识，此标识在外部网络中不可见。

当您将 Workstation Pro 安装到 Windows 或 Linux 主机系统时，系统会设置一个 NAT 模式网络 (VMnet8)。在您使用**新建虚拟机**向导创建新的虚拟机并选择典型配置类型时，该向导会将虚拟机配置为使用默认 NAT 默认网络。

您只能有一个 NAT 模式网络。可以参见[配置网络地址转换](https://docs.vmware.com/cn/VMware-Workstation-Pro/15.0/com.vmware.ws.using.doc/GUID-89311E3D-CCA9-4ECC-AF5C-C52BE6A89A95.html#GUID-89311E3D-CCA9-4ECC-AF5C-C52BE6A89A95)。

### 仅主机模式网络连接

仅主机模式网络连接可创建完全包含在主机中的网络。仅主机模式网络连接使用对主机操作系统可见的虚拟网络适配器，在虚拟机和主机系统之间提供网络连接。

## 参考

[使用 VMware Workstation Pro](https://docs.vmware.com/cn/VMware-Workstation-Pro/15.0/com.vmware.ws.using.doc/GUID-0EE752F8-C159-487A-9159-FE1F646EE4CA.html)