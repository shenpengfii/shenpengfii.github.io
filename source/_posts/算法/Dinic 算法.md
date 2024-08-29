---
mathjax: true
topic: algo_topic
title: Dinic 算法
subtitle: 介绍用于最大流问题的 Dinic C++ 实现
date: 2024-08-29 13:27:23
categories:
  - 算法
tags:
  - Dinic
  - Dinitz
  - C++
  - 算法
references:
  - "[wikipedia,Dinic's algorithm](https://en.wikipedia.org/wiki/Dinic%27s_algorithm)"
  - "[Nishant Singh,Dinic’s algorithm for Maximum Flow,2023-03-13](https://www.geeksforgeeks.org/dinics-algorithm-maximum-flow/)"
  - "[ShusenWang,13-4: Dinic's Algorithm 寻找网络最大流,2021-06-10](https://www.bilibili.com/video/BV1j64y1R7yK/?spm_id_from=333.337.search-card.all.click&vd_source=52571ceef051ee05017e03e129308c71)"
---

<p id='brief'>介绍用于最大流问题的 Dinic C++ 实现</p>

<!-- more -->
<script>document.getElementById('brief').remove();</script>

## 简介

{% mark Dinic 算法 %}（或称 Dinitz 算法）是解决最大流问题的常用算法之一，其设计思想与 Edmonds-Karp 算法非常接近。

Dinic 算法首先尝试构造一个网络流的分层图，随后在分层图中不断搜索从源点到汇点的阻塞流，直到无法再找到为止。每次搜索成功阻塞流，就立即调整原先的网络流量分配，并更新最大流的值。

Dinic 算法的时间复杂度为 $O(V^2E)$，其中 $|V|$ 表示顶点数，$|E|$ 表示边数。

## 实现方法

1. 定义 $F$ 为残存流矩阵，初始化 $F$ 为流网络的容量限制；定义 $maxf$ 为最大流值，初始化为 $0$。
2. 接下来不断地尝试使用 BFS 构造分层图，直到分层图不再包含汇点时停止。
{% note 注释 使用 $level$ 数组存储分层图，$level$ 数组也可以同时充当 $used$ 数组，以免重复访问节点。 %}
3. 对每次构造得到的分层图，重复通过 DFS 访问图中阻塞流上的每条路径。每条路径访问完毕后更新原先的残存流 $F$ 和最大流值 $maxf$。
{% note 注释 这里阻塞流上的路径等同于 Edmonds-Karp 算法中的增广路径。由于这些路径是通过同一次 BFS 找到的，因此这些路径共享相同的分层图，一条路径对流的增广不会影响其他路径。在 DFS 过程中，同步递归地计算一条阻塞流路径的流值，并在返回时更新残存流 $F$。 %}

## 实现细节

Dinic 算法的主要实现细节可以参照 Edmonds-Karp 算法。

在此说明唯一需要注意的点：DFS 递归过程中，要检查子递归返回的阻塞流路径的流值。如果下层返回值为 $0$ 则不再向上层返回，转而探索其同级分支，以减少 DFS 调用次数。

## 代码实现

[P2740 【[USACO4.2]草地排水Drainage Ditches】 C++ 代码](https://www.luogu.com.cn/record/175223153)
