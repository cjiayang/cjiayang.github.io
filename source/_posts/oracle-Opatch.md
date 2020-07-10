---
title: Oracle11.2.0.4打补丁实践
date: 2020-07-10 14:45:51
categories:
  - Oracle
tags:
  - Oracle
  - OPatch
---

本文介绍如何给Oracle数据库打补丁，数据库版本为Oracle11.2.0.4，单实例，操作系统为redhat6.5。

<!--more-->

## 下载升级包

1. **下载升级包**

   因为没有MOS帐号，从网上下载了升级包

   > p30670774_112040_Linux-x86-64.zip：数据库补丁
   > p6880880_112000_Linux-x86-64.zip：opatch升级包
   >百度云链接：https://pan.baidu.com/s/1ZvTQr-gI889mylgzUXkdPw 
   > 提取码：plhp
   > 微云链接：https://share.weiyun.com/k8CbxL3K
   
   opatch是安装补丁的程序，数据库软件安装完成后，就自带了opatch，但是版本太旧了，所以这里下载最新的opatch，p6880880_112000_Linux-x86-64.zip包就是用来升级opatch用的。

## 升级opatch

opatch的升级只要将新版本的包解压，覆盖系统原始的文件即可。

2. **查看原始opatch信息**

   查看原始版本

   ```shell
   [oracle@node1 ~]$ cd $ORACLE_HOME/OPatch
   [oracle@node1 OPatch]$ ./opatch version	# 查看原始版本
   
   OPatch Version: 11.2.0.3.4
   OPatch succeeded.
   ```

   查看补丁情况

   ```shell
   [oracle@node1 OPatch]$ ./opatch lsinventory	# 查看补丁情况
   
   Oracle 中间补丁程序安装程序版本 11.2.0.3.4
   版权所有 (c) 2012, Oracle Corporation。保留所有权利。
   
   Oracle Home       : /home/oracle/app/oracle/product/11.2.0/dbhome_1
   Central Inventory : /home/oracle/app/oraInventory
      from           : /home/oracle/app/oracle/product/11.2.0/dbhome_1/oraInst.loc
   OPatch version    : 11.2.0.3.4
   OUI version       : 11.2.0.4.0
   Log file location : /home/oracle/app/oracle/product/11.2.0/dbhome_1/cfgtoollogs/opatch/opatch2020-07-02_16-23-35下午_1.log
   
   Lsinventory Output file location : /home/oracle/app/oracle/product/11.2.0/dbhome_1/cfgtoollogs/opatch/lsinv/lsinventory2020-07-02_16-23-35下午.txt
   
   ----------------------------------------------------------------------------
   已安装的顶级产品 (1):
   Oracle Database 11g                                                  11.2.0.4.0
   此 Oracle 主目录中已安装 1 个产品。
   此 Oracle 主目录中未安装任何中间补丁程序。
   ----------------------------------------------------------------------------
   
   OPatch succeeded.
   ```

3. **备份原opatch**

   ```shell
   [oracle@node1 OPatch]$ cd $ORACLE_HOME
   [oracle@node1 dbhome_1]$ mv OPatch OPatch.bak
   ```

4. **解压新下载的opatch包**

   将下载的opatch包上传到oracle的家目录，上传完后，解压到`$ORACLE_HOME`下

   ```shell
   [oracle@node1 dbhome_1]$ cd ~
   [oracle@node1 ~]$ unzip p6880880_112000_Linux-x86-64.zip -d $ORACLE_HOME
   ```

5. **检查opatch是否升级成功**

   ```shell
   [oracle@node1 ~]$ cd $ORACLE_HOME/OPatch
   [oracle@node1 OPatch]$ ./opatch version	# 查看版本信息
   
   OPatch Version: 11.2.0.3.21
   OPatch succeeded.
   ```

   现在版本为11.2.0.3.21，说明升级成功

## 数据库打补丁

6. **关闭监听、数据库**

   ```shell
   [oracle@node1 OPatch]$ lsnrctl stop	# 关闭监听
   [oracle@node1 ~]$ sqlplus / as sysdba
   SQL> shutdown immediate 
   SQL> quit
   ```

7. **解压补丁包**

   将下载的补丁包上传到oracle家目录，并解压

   ```shell
   [oracle@node1 ~]$ cd ~
   [oracle@node1 ~]$ unzip p30670774_112040_Linux-x86-64.zip 
   ```

8. **校验冲突**

   ```shell
   [oracle@node1 ~]$ cd 30670774/
   [oracle@node1 30670774]$ $ORACLE_HOME/OPatch/opatch prereq CheckConflictAgainstOHWithDetail -ph ./
   
   Oracle 临时补丁程序安装程序版本 11.2.0.3.21
   版权所有 (c) 2020, Oracle Corporation。保留所有权利。
   
   PREREQ session
   
   Oracle 主目录       ：/home/oracle/app/oracle/product/11.2.0/dbhome_1
   主产品清单：/home/oracle/app/oraInventory
      来自           ：/home/oracle/app/oracle/product/11.2.0/dbhome_1/oraInst.loc
   OPatch 版本    ：11.2.0.3.21
   OUI 版本       ：11.2.0.4.0
   日志文件位置：/home/oracle/app/oracle/product/11.2.0/dbhome_1/cfgtoollogs/opatch/opatch2020-07-02_16-54-06下午_1.log
   
   Invoking prereq "checkconflictagainstohwithdetail"
   
   Prereq "checkConflictAgainstOHWithDetail" passed.
   
   OPatch succeeded.
   ```

   由于这个测试库之前并没有打补丁，所以这里就没有补丁冲突的问题，如果这里显示有冲突，再去网MOS上查找相关解决方案。

9. **执行打补丁**

   ```shell
   [oracle@node1 30670774]$ $ORACLE_HOME/OPatch/opatch apply
   ```

   ![确认并继续](confirmAndContinue.png)

   这里要输入3次y和一次回车，打补丁的过程耗是比较久的，耐心等待

10. **检查打补丁情况**

    ```shell
    [oracle@node1 30670774]$ $ORACLE_HOME/OPatch/opatch lsinventory # 检查打补丁情况
    ```

    这里会列出系统已打的补丁情况

11. **启动数据库，并运行sql文件**

    ```shell
    [oracle@node1 30670774]$ cd $ORACLE_HOME/rdbms/admin
    [oracle@node1 30670774]$ sqlplus / as sysdba
    SQL> startup
    SQL> @catbundle.sql psu apply
    SQL> quit
    ```

12. **启动监听**

    ```shell
    [oracle@node1 admin]$ lsnrctl start
    ```

至此数据库打补丁已经全部完成

## 回退数据库补丁

**数据库在做变更时，需要考虑回退方案**，接下来介绍如何回退数据库补丁

13. **关闭监听、数据库**

    ```shell
    [oracle@node1 ~]$ lsnrctl stop	# 关闭监听
    [oracle@node1 ~]$ sqlplus / as sysdba
    SQL> shutdown immediate 
    SQL> quit
    ```

14. **回退补丁**

    ```shell
    [oracle@node1 ~]$ $ORACLE_HOME/OPatch/opatch rollback -id 30670774
    ```

    ![回退补丁](rollback.png)

    这里要回复一次y，回退过程比较耗时，需耐心等待

    

15. **启动数据库，并运行sql文件**

    ```shell
    [oracle@node1 ~]$ cd $ORACLE_HOME/rdbms/admin
    [oracle@node1 admin]$ sqlplus / as sysdba
    SQL> startup
    SQL> @catbundle_PSU_ORCL_ROLLBACK.sql	# 这里的ORCL为实例名称
    SQL> quit
    ```

16. **检查回退情况**

    ```shell
    [oracle@node1 admin]$ $ORACLE_HOME/OPatch/opatch lsinventory # 检查打补丁情况
    
    Oracle 临时补丁程序安装程序版本 11.2.0.3.21
    版权所有 (c) 2020, Oracle Corporation。保留所有权利。
    
    
    Oracle 主目录       ：/home/oracle/app/oracle/product/11.2.0/dbhome_1
    主产品清单：/home/oracle/app/oraInventory
       来自           ：/home/oracle/app/oracle/product/11.2.0/dbhome_1/oraInst.loc
    OPatch 版本    ：11.2.0.3.21
    OUI 版本       ：11.2.0.4.0
    日志文件位置：/home/oracle/app/oracle/product/11.2.0/dbhome_1/cfgtoollogs/opatch/opatch2020-07-02_18-24-48下午_1.log
    
    Lsinventory Output file location : /home/oracle/app/oracle/product/11.2.0/dbhome_1/cfgtoollogs/opatch/lsinv/lsinventory2020-07-02_18-24-48下午.txt
    
    --------------------------------------------------------------------------------
    Local Machine Information::
    Hostname: localhost
    ARU platform id: 226
    ARU platform description:: Linux x86-64
    
    已安装的顶级产品 (1):
    
    Oracle Database 11g                                                  11.2.0.4.0
    此 Oracle 主目录中已安装 1 个产品。
    
    
    此 Oracle 主目录中未安装任何临时补丁程序。
    
    
    --------------------------------------------------------------------------------
    
    OPatch succeeded.
    ```

    可以看到，补丁已经被卸载了

17. **启动监听**

    ```shell
    [oracle@node1 admin]$ lsnrctl start
    ```

这样补丁回退就结束了

## 总结

本文介绍了单实例数据库打补丁的步骤，仅做参考，实际应以补丁包中的readme为准。

## 参考

- [Oracle - 数据库打补丁最佳实践](https://www.cnblogs.com/ddzj01/p/12097467.html)
- readme.html