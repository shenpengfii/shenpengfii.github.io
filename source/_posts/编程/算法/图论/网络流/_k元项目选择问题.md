---
topic: flow
title: k 元项目选择问题
subtitle: 文章摘要
date: 2024-10-04 21:43:03
categories:
  - 网络流
tags:
  - k-ary Project Selection Problem
  - Burn-Fill Problem
  - 拆点法
references:
  - "[つる,燃やす埋める問題と劣モジュラ関数のグラフ表現可能性 その①,2020-03-13](https://theory-and-me.hatenablog.com/entry/2020/03/13/180935)"
  - "[noshi91,最小カット問題の k 値への一般化,2021-06-29](https://noshi91.hatenablog.com/entry/2021/06/29/044225)"
  - "[Satoru Fujishige,《Submodular Functions and Optimization (2nd Edition)》,2005](https://book.douban.com/subject/2869138)"
  - "[Ravindra K. Ahuja,《Network Flows: Theory, Algorithms, and Applications》,1993](https://book.douban.com/subject/35343567/)"
  - "古天龙,《离散数学》,2012"
mathjax: true
mermaid: true
---

<p id='brief'>文章摘要</p>

<!-- more -->
<script>document.getElementById('brief').remove();</script>

## 问题描述

在你学习用网络流算法解决 $k$ 元项目选择问题（$k$-ary Project Selection Problem）之前，你需要先回顾一下这个问题的前身——网络流中经典的项目选择问题（Project Selection Problem），这个问题模型也可称之为最大权闭合子图（Maximum Weight Closure of a Graph）。

### 项目选择问题

展开下面的列表，你将会看到有关项目选择问题和最大权闭合子图的详细描述：

{% folders %}

<!-- folder 最大权闭合子图 -->

有向图 $G= (N, E)$ 的**闭包**是一个不包含任何出边的节点子集 $N_1 \subseteq N$。也就是说，节点子集 $N_1$ 满足如下条件： $\forall i \in N_1$，如果 $(i,j) \in E$，则一定有 $j \in N_1$。

现在假设每个节点都有一个权重 $w_i \in \mathbb{R}$，这个数值可正可负。在最大权闭合子图问题中，你需要在图中找到一个具有最大权重的闭包 $N_1$，该闭包的权重定义为 $w(N_1) = \Sigma_{i \in N_1}{w_i}$。

这个寻找最大权重闭包的问题就叫做**最大闭合权子图**。

{% note 提示 [《Submodular Functions and Optimization (2nd Edition)》](https://book.douban.com/subject/2869138) 书中的 19.2 节证明了可以将这个问题转化为一个最小割问题，并给出了网络流的构造方法。这里只需要知道，所求的最大权闭包子图对应的就是流网络分割后的 S 端节点，而最大闭包的权重就是用所有正节点的权重之和减去最小割容量的值。 color:green %}

<!-- folder 项目选择问题 -->

你是一个项目公司经理，现在公司有 $n$ 个可开展的商业项目，项目 $i$ 对应的预期收益为 $P_i(P_i \geq 0)$。只有在采购了所需的机器之后，项目才可以实现盈利。每个项目依赖若干种不同的机器，不同的项目之间可以共享同一种机器，且每种机器只需要采购一台就能完成依赖该机器的所有项目。假设所有项目共需要 $m$ 种机器，第 $j$ 种机器的采购成本为 $M_j(M_j \geq 0)$。你需要为公司挑选一批项目，并采购所需机器，保证这些项目的净利润最大化。

这个问题可以转化为一个最大权闭合子图问题。在有向图中，将项目和机器均看作节点，项目节点的权重配置为 $P_i$，而机器节点的权重配置为 $-M_j$，并把从项目到机器的依赖看做一条有向边。这时，保证净利润最大的项目选择方案实际上就对应了图中的最大权闭合子图，而这个净利润的最大值就是最大权闭合子图的权重。

{% endfolders %}

从上述定义中不难看出，无论是项目选择问题还是最大权闭合子图问题，都可以最终转化为一类特殊的最小割问题。对这类具有共同特征的最小割问题，你可能会思考如何用数学工具统一其形式。恰好，一些论文已经完成了这部分工作。

{% note 提示 在学习后续内容之前，你需要掌握网络流的基本概念和相关算法。 color:green %}

从上文中可以得知，项目选择问题本质上就是一个最大权闭包子图问题。因此，你只需要定义出最大权闭包子图的数学表述即可：

---

对于一张节点带权的有向图 $G = <V, E>$，节点 $i$ 的权重为 $w_i$，定义 $V_+$ 为所有正权值节点的集合，$N_1$ 代表最大流。则其最大权闭包子图问题的数学表述为：

$$
\begin{align}
\text{Maximize} \quad & \sum_{i \in N_1}{w_i} \cr
\end{align}
$$

---

对于一张流网络图 $G=<V,E>$，定义 $i<j$ 代表节点 $i$ 和 $j$ 之间的边存在且属于网络流的某个割，则其最优化问题可以定义为：

$$
\begin{align}
\text{Minimize} \quad & \kappa + \sum_{i < j}{c(i, j)} \cr
\text{Subject to} \quad & i, j \in V
\end{align}
$$

### $k$ 元项目选择问题

## 子模函数与子模系统

子模函数（Submodular Function），也称作“劣模函数”，是指满足以下条件的函数：

$$
f(x) \leq \sum_{y \in x} f(y)
$$
