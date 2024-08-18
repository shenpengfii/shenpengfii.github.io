---
title: 偷本非礼 | Github 加速教程
subtitle: 手动配置 Github 的最新纯净 DNS 映射，实现 Github 访问加速
date: 2024-06-21 17:05:36
categories:
  - 环境配置
tags:
  - Github
  - 加速
cover: https://tvax3.sinaimg.cn/large/008kS6srly1hss61cps8jj30nm0ergm9.jpg
banner: https://tvax3.sinaimg.cn/large/008kS6srly1hss61cps8jj30nm0ergm9.jpg
poster:
  headline: Github 加速教程
  caption: 手动配置 Github 的最新纯净 DNS 映射，实现 Github 访问加速
references:
  - "[ClimbSnail,多种GitHub加速方式,2020-08-07](https://climbsnail.github.io/2020/GithubSpeed/)"
  - "[天乐404,GitHub访问不了或者很慢的解决办法,2023-11-16](https://cloud.tencent.com/developer/article/2359332)"
---
{% folding open:false 为什么国内 Github 访问这么慢？ %}

国内网络访问 Github 速度过慢的原因有许多，但其中最直接的原因就是 CND 域名遭到 DNS 污染。

DNS 污染是指一些刻意或无意制造出来的数据包，把域名指向不正确的 IP 地址，阻碍了网络访问。我们默认从目标网址的最近 CDN 节点获取内容，但当节点过远或 DNS 指向错误时，就会操成访问速度过慢或无法访问的问题。

{% folding open:false 什么是 CDN？ %}

Content Delivery Network (CDN)，即内容分发网络，依靠部署在各地的边缘服务器，平衡中心服务器的负荷，就近提供用户所需内容，提高响应速度和命中率。

{% endfolding %}

{% endfolding %}

使用浏览器访问[最新 Github DNS 配置下载链接](https://raw.hellogithub.com/hosts)，下载得到一份 hosts 文本文件，该文件中包含最新版本的 Github DNS 配置。

{% link https://raw.hellogithub.com/hosts 最新 Github DNS 配置下载链接 %}

复制下载文件中的全部内容，通过管理员权限粘贴到系统 hosts 文件底部。

{% folding color:blue 不同操作系统的 hosts 文件地址如表所示 %}

| 操作系统 | HOST 文件地址 |
| --- | --- |
| Windows | `C:\Windows\System32\drivers\etc\hosts` |
| Linux | `/etc/hosts` |
| MAC | `/etc/hosts` |
| Android | `/system/etc/hosts` |
| iOS | `/etc/hosts` |

{% endfolding %}

这样，可高速访问的 Github 网站的 DNS 映射就配置好了。该 DNS 映射的 IP 可能离你较远，初次访问 Github 需要不断刷新，等待几分钟后才能生效。

最后，Github 的高速 DNS 映射每隔一段时间会失效，因此我们需要周期性的下载替换最新版的 hosts 文件。
