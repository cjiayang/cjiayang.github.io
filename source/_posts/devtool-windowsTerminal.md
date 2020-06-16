---
title: 初识windows Terminal
date: 2020-06-06 10:26:01
categories: 
  - windows
  - devtool
tags: 
  - windows terminal
---

windows terminal是微软推出的简单易用的**命令行终端工具**，支持cmd命令，PowerShell命令，和WSL子系统命令。本文简单介绍windows Terminal的配置。

<!--more-->

## Windows Terminal简介

Windows 终端是一个面向命令行工具和 shell（如命令提示符、PowerShell 和适用于 Linux 的 Windows 子系统 (WSL)）用户的新式终端应用程序。 它的主要功能包括多个选项卡、窗格、Unicode 和 UTF-8 字符支持、GPU 加速文本呈现引擎，还可以用于创建你自己的主题并自定义文本、颜色、背景和快捷键绑定。

以上是官方文档的介绍，可以看出Windows Terminal首先是一个微软官方提供的一个**终端工具**，包含了许多便于用户使用的新特性，并提供自定义配置功能。我们可以方便的使用Windows Terminal中执行cmd命令，PowerShell命令，和WSL子系统命令。

### 如何安装

直接从 [Microsoft Store](https://aka.ms/terminal) 安装 Windows Terminal。

### 官方文档

https://docs.microsoft.com/zh-cn/windows/terminal/

## 自定义配置

### 在哪配置

打开 Windows 终端，在下拉菜单中选择“设置” 。 终端会使用默认文本编辑器打开 `settings.json` 文件。在`settings.json`文件中，终端支持自定义影响整个应用程序的[全局属性](https://docs.microsoft.com/zh-cn/windows/terminal/customize-settings/global-settings)、影响每个配置文件的设置的[配置文件属性](https://docs.microsoft.com/zh-cn/windows/terminal/customize-settings/profile-settings)以及允许你使用键盘与终端交互的[键绑定](https://docs.microsoft.com/zh-cn/windows/terminal/customize-settings/key-bindings)。

配置文件的具体配置官方文档已经写的足够简洁了，一般使用默认配置即可满足需求，如果需要修改快捷键或者主题样式，可以按官方文档的配置步骤配置即可。下面对配置文件进行解释。

### 配置文件注释

```
// 最外层的都是全局属性，配置的指影响全局
{
    "$schema": "https://aka.ms/terminal-profiles-schema",
    "alwaysShowTabs" : true,
    "defaultProfile" : "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}", // 配置打开新标签页时使用的是哪个profile，值对应profiles里对象的guid
    "initialCols" : 120,
    "initialRows" : 30,
    "keybindings" : // 配置快捷键的地方
    [
        {
            "command" : "closeTab",
            "keys" : 
            [
                "ctrl+w"
            ]
        },
        {
            "command" : "newTab",
            "keys" : 
            [
                "ctrl+t"
            ]
        }
    ],
    "requestedTheme" : "system",
    "showTabsInTitlebar" : true,
    "showTerminalTitleInTitlebar" : true,
    "profiles" :  // 每个终端的配置
    [
        {
            "acrylicOpacity" : 0.9,
            // "background" : "#012456",
            "closeOnExit" : true,
            "colorScheme" : "Solarized Dark", 		// 颜色主题，对应的是schemes对象的name值
            "commandline" : "powershell.exe",		// 打开时执行的命令
            "cursorColor" : "#FFFFFF",
            "cursorShape" : "bar",
            "fontFace" : "Consolas",  				// 字体
            "fontSize" : 10,  						// 字体大小
            "guid" : "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",	// 终端guid
            "historySize" : 9001,
            "icon" : "ms-appx:///ProfileIcons/{61c54bbd-c2c6-5271-96e7-009a87ff44bf}.png",
            "name" : "Windows PowerShell", 			// 终端名
            "padding" : "0, 0, 0, 0",
            "snapOnInput" : true,
            "startingDirectory" : "%USERPROFILE%",
            "useAcrylic" : false					// 透明背景
        }
    ],
    "schemes" : 	// 主题样式
    [
        {
            "background" : "#0C0C0C", 	// 背景颜色
            "black" : "#0C0C0C",
            "blue" : "#0037DA",
            "brightBlack" : "#767676",
            "brightBlue" : "#3B78FF",
            "brightCyan" : "#61D6D6",
            "brightGreen" : "#16C60C",
            "brightPurple" : "#B4009E",
            "brightRed" : "#E74856",
            "brightWhite" : "#F2F2F2",
            "brightYellow" : "#F9F1A5",
            "cyan" : "#3A96DD",
            "foreground" : "#CCCCCC",	//前景颜色
            "green" : "#13A10E",
            "name" : "Campbell",		//主题名称
            "purple" : "#881798",
            "red" : "#C50F1F",
            "white" : "#CCCCCC",
            "yellow" : "#C19C00"
        }
    ]
}

```

