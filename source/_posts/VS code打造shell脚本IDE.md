---
title: vscode打造shell脚本IDE【转】
date: 2022-11-22 15:22:19
categories:
  - 开发工具
tags:
  - vscode
---
本文介绍如何简单配置vscode，使其成为好用的shell脚本编辑器。同时分享一些shell相关的学习资料。

<!--more-->

## VS code打造shell脚本IDE【转】

近期多了些开发shell脚本的需求，便做了些研究，于是发现：

1. shell没有专用的IDE
2. 老手们习惯了vim的开发方式，干起活来非常黑客。但对新人，不太友好

既然没有现成的，那就用插件组一套，软件依然是vscode。

#### 1. shellman

说起IDE，第一时间想到的必然是智能提示和自动补全，shellman全部搞定

![img](https://s2.loli.net/2022/11/22/gy6PcdSmQkULB7Z.png)

下载后，新建test.bash文件，输入case，可见如下结果：

![img](https://s2.loli.net/2022/11/22/fDmVr9TxEhlkOni.jpg)

选中提示中的第一个，然后就获得了if全家桶：

![img](https://s2.loli.net/2022/11/22/eKMrgNvDEGuftRX.jpg)

由上面两张图可见shellman的提示是比较系统的。

#### 2. shellcheck

有了自动补全，然后就是语法错误检查了

![img](https://s2.loli.net/2022/11/22/JPH5Nb1TQGSRsgI.jpg)

安装成功后，再写代码就会出现如下的错误提示：

![img](https://s2.loli.net/2022/11/22/t3o67U1Sbr9Ggzi.png)

#### 3. shell-format

脚本写好了，当然要格式化一下

![img](https://s2.loli.net/2022/11/22/FU9BTz8aRpr4vuW.jpg)

快捷键：Alt+Shift+F

#### 4. Code Runner

在vs code里开发，在vs code里纠错，又在vs code里格式化，到了调试不会要去命令行吧！

![img](https://s2.loli.net/2022/11/22/ravESXh56Gm9n1Y.jpg)

安装完后，如果出现require reload的字样，请重启vs code。然后对刚才创建的test.bash右键，（或者在文件里右键）：

![img](https://s2.loli.net/2022/11/22/KYPw9SzsIXUbrDJ.jpg)

#### 5. 配置VSCode运行shell

[VSCode: Windows 下配置 VSCode运行shell](http://link.zhihu.com/?target=https%3A//www.cnblogs.com/yongdaimi/p/15247771.html)

### shell编程资源

shell是与Linux系统对话的门户，每个要与Linux对话的人都避免不了学习shell。市面上充斥着各种培训教材和培训课程，引导了shell的初学者入门，但又往往对学习shell的初学者产生误导，入门而无法深入。信息的社会里最不缺的就是信息，缺的是高质量的信息。如果有质量上乘权威的学习资料和手册，将会使想要学习shell的人少走很多弯路。我也是在学习shell的路上碰到了这些好的资料，发现其质量上乘，还是开放阅读的，相见恨晚，感于其被杂乱的信息所淹没，分享于此。

| 资源                            | 地址                                                                                                                                            |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| 高级Bash脚本编程指南-英文原版   | [https://tldp.org/LDP/abs/html/](https://tldp.org/LDP/abs/html/)                                                                                   |
| 高级Bash脚本编程指南-中译本     | [http://home.ustc.edu.cn/~stephen1/book/abs-guide-3.7-cn/HTML/index.html](http://home.ustc.edu.cn/~stephen1/book/abs-guide-3.7-cn/HTML/index.html) |
| 高级Bash脚本编程指南-中译本最新 | [https://linuxstory.gitbook.io/advanced-bash-scripting-guide-in-chinese/](https://linuxstory.gitbook.io/advanced-bash-scripting-guide-in-chinese/) |
| Bash Reference Manual           | [https://www.gnu.org/software/bash/manual/bash.html](https://www.gnu.org/software/bash/manual/bash.html)                                           |
| Bash中文手册                    | [https://chegva.com/2777.html](https://chegva.com/2777.html)                                                                                       |
| 鸟哥的Linux私房菜               | [http://cn.linux.vbird.org/linux_basic/linux_basic.php](http://cn.linux.vbird.org/linux_basic/linux_basic.php)                                     |

### **参考**

- [VS code打造shell脚本IDE](https://zhuanlan.zhihu.com/p/199187317)
- [Shell调试技巧](https://chegva.com/3776.html)
