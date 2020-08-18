---
title: Oracle导入导出实践
date: 2020-08-18 14:01:54
categories:
  - Oracle
tags:
  - Oracle
  - expdp
  - impdp
---

本文介绍Oracle导出导入工具expdp和impdp的使用，并使用scott用户演示其用法。

<!-- more -->

### 数据导出 ###

Oracle提供的expdp工具可以将数据库对象的元数据或数据导出到转储文件中，expdp可以导出表、用户模式、表空间和全数据库4种数据。
expdp工具只能将导出的转储文件存放在directory对象对应的磁盘目录中，而不能直接指定转储文件所在的磁盘目录。因此，使用expdp工具时，必须首先建立directory对象，并且需要为数据库用户授予使用directory对象的权限。

#### EXPDP选项 ####

可以在命令行输入`expdp help=y;`获取帮助
expdp常用选项：

- HELP: 帮助信息
- DIRECTORY：用于转储文件和日志文件的目录对象
- DUMPFILE：指定目标转储文件名的列表
- SCHEMAS: 要导出的方案的列表
- TABLES: 标识要导出的表的列表
  例如, TABLES=HR.EMPLOYEES,SH.SALES:SALES_1995
- TABLESPACES: 标识要导出的表空间的列表
- EXCLUDE：排除特定对象类型
  例如, EXCLUDE=SCHEMA:"='HR'"
- INCLUDE: 包括特定对象类型
- FULL：导出整个数据库
- LOGFILE: 指定日志文件名
- QUERY: 用于导出表的子集的谓词子句
  例如, QUERY=employees:"WHERE department_id > 10"

#### 创建用户

为方便演示创建一个具有dba权限的管理员用户，并激活oracle自带的scott用户。

```sql
SQL> create user manager identified by code666 default tablespace system temporary tablespace temp;
SQL> grant connect,resource,dba to manager;
SQL> alter user scott account unlock;
SQL> alter user scott identified by tiger;
```

#### 创建导出目录 ####

首先在oracle用户下创建导出目录

```
$ mkdir -p /home/oracle/data/dump
```

然后创建oracle目录对象，并授权

```sql
SQL> create or replace directory dump_dir as '/home/oracle/data/dump';
SQL> grant read,write on directory dump_dir to scott;
SQL> grant read,write on directory dump_dir to manager;
```

#### 导出表 ####

```shell
$ expdp scott/tiger directory=dump_dir dumpfile=table.dmp tables=emp,dept
```

#### 导出模式 ####

导出模式时，当前用户只能导出当前用户的模式对象，要导出其他用户的对象要求用户必须具有DBA角色或EXP_FULL_DATABASE角色。下面导出scott模式

```shell
$ expdp scott/tiger directory=dump_dir dumpfile=schema.dmp schemas=scott
```

#### 导出表空间 ####

导出表空间是指将一个或多个表空间中的所有对象及数据存储到转储文件中。导出表空间要求用户必须具有DBA角色或EXP_FULL_DATABASE角色，下面用manager用户导出

```shell
$ expdp manager/code666 directory=dump_dir dumpfile=tablespace.dmp tablespaces=system
```

#### 全库导出 ####

导出数据库要求用户必须具有DBA角色或EXP_FULL_DATABASE角色。
注意，导出数据库时，不会导出SYS、ORDSYS、ORDPLUGINS、CTXSYS、MDSYS、LBACSYS以及XDB等模式中的对象

```shell
$ expdp manager/code666@orcl directory=dump_dir dumpfile=full.dmp full=y logfile=full.log;
```


### 数据导入 ###

Oracle提供impdp工具可以将转储文件中的数据库对象的元数据导入到Oracle数据库中，IMPDP也可以进行4种类型的导入操作：导入表、导入模式、导入表空间和导入全数据库。
与expdp相似，数据泵导入时，其转储文件被存储在DIRECTORY对象所对应的磁盘目录中，而不能直接指定转储文件所在的磁盘目录。

#### IMPDP命令 ####

同样可以在命令行输入`impdp help=y;`查看impdp获取命令帮助。

下面操作假设拿到dump文件，将dump文件导入到另一台机子中去，所以也需要创建用户和创建目录的操作。导入前需要将dump文件放到导入目录中，并改文件所属用户为oracle。

#### 创建用户

创建一个具有dba权限的管理员用户，并激活oracle自带的scott用户。

```sql
SQL> create user manager identified by code666 default tablespace system temporary tablespace temp;
SQL> grant connect,resource,dba to manager;
SQL> alter user scott account unlock;
SQL> alter user scott identified by tiger;
```

#### 创建导入目录 ####

首先在oracle用户下创建导入目录

```shell
$ mkdir -p /home/oracle/data/imp
```

然后创建oracle目录对象，并授权

```sql
SQL> create or replace directory imp_dir as '/home/oracle/data/imp';
SQL> grant read,write on directory imp_dir to manager;
```

#### 导入表 ####

普通用户只可以将表导入到自己的模式中，但如果以其他用户身份导入表，则要求该用户必须具有IMP_FULL_DATABASE角色和DBA角色。
导入表时，既可以将表导入到源模式中，也可以将表导入到其他模式中。以下示例将emp,dept表导入到manager模式中

```shell
$ impdp manager/code666 directory=imp_dir dumpfile=table.dmp tables=scott.emp,scott.dept remap_schema=scott:manager  
```

#### 导入模式 ####

普通用户可以将对象导入到自身模式中，但如果以其他用户身份导入模式时，则要求该用户必须具有IMP_FULL_DATABASE角色或DBA角色。
导入模式时，既可以将模式的所有对象导入到源模式中，也可以将模式的所有对象导入到其他模式中

```shell
$ impdp manager/code666 directory=imp_dir dumpfile=schema.dmp remap_schema=scott:manager
```

#### 导入表空间 ####

导出表空间要求用户必须具有DBA角色或EXP_FULL_DATABASE角色。导入时，可以将数据导入到其他表空间中。

```shell
$ impdp manager/code666 directory=imp_dir dumpfile=tablespace.dmp remap_tablespace=users:system
```

#### 全库导入 ####

全库导入要求用户必须具有DBA角色或EXP_FULL_DATABASE角色

```shell
$ impdp manager/code666 directory=imp_dir dumpfile=ful.dmp full=y
```

### 总结

Oracle提供的expdp工具和impdp工具都是工作在服务端，可以方便的多粒度的进行数据的导出导入操作，在开发中可以灵活使用。