---
title: QT6 镜像安装教程
subtitle: 使用中科大源安装 QT Online Creator
date: 2024-10-03 13:28:38
categories:
  - 环境配置
tags:
  - QT
references:
  - "[Qt 中科大镜像帮助页面](https://mirrors.ustc.edu.cn/help/qtproject.html)"
---

<p id='brief'>使用中科大源安装 QT Online Creator</p>

<!-- more -->
<script>document.getElementById('brief').remove();</script>

进入 [QT6 中科大镜像源](https://mirrors.ustc.edu.cn/qtproject/official_releases/online_installers/) 下载对应系统版本的安装包。

以 Windows 系统安装为例，进入命令行，执行该命令启动安装程序：

{% copy .\qt-unified-windows-x86-online.exe --mirror https://mirrors.ustc.edu.cn/qtproject %}

该命令配置了中科大源，安装过程中会自动从中科大源下载安装包。