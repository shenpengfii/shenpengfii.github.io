---
title: Github 加速
date: 2024-06-21 17:05:36
tags:
    - Github
    - 加速
---

手动配置 Github 的最新纯净 DNS 映射，实现 Github 访问加速

<!-- more -->

# 1. 为什么国内 Github 访问时间长、速度慢？

国内网络访问 Github 速度过慢的原因有许多，但其中最直接的原因就是 CND 域名遭到 DNS 污染。

- Content Delivery Network (CDN)，即内容分发网络，依靠部署在各地的边缘服务器，平衡中心服务器的负荷，就近提供用户所需内容，提高响应速度和命中率。

- DNS 污染，是指一些刻意或无意制造出来的数据包，把域名指向不正确的 IP 地址，阻碍了网络访问。我们默认从目标网址的最近 CDN 节点获取内容，但当节点过远或 DNS 指向错误时，就会操成访问速度过慢或无法访问的问题。

# 2. 修改本机 HOSTS 文件，手动配置 Github DNS 映射

## 2.1 获取最新的Github DNS配置

使用浏览器访问[最新Github DNS配置下载链接](https://raw.hellogithub.com/hosts)，下载得到一份hosts文本文件，该文件包含最新版本的Github DNS配置。

## 2.1 打开系统 HOSTS 文件

| 操作系统 | HOST文件地址 |
| --- | --- |
| Windows | `C:\Windows\System32\drivers\etc\hosts` |
| Linux | `/etc/hosts` |
| MAC | `/etc/hosts` |
| Android | `/system/etc/hosts` |
| iOS | `/etc/hosts` |

> 注：使用管理员权限或root用户权限编辑该文件

复制下载到的hosts文件内容，再添加到系统hosts文件中保存。

这时，可高速访问的 Github DNS 映射就配置好了。 

# 参考链接

[天乐404,GitHub访问不了或者很慢的解决办法,2023-11-16](https://cloud.tencent.com/developer/article/2359332)
