---
title: Pytorch 镜像安装教程（Win10/11）
subtitle: 使用清华源镜像安装最新版 pytorch，含 CPU 版和 GPU 版的详细安装步骤
date: 2024-07-15 16:36:32
tags:
   - pytorch
   - 安装
   - 镜像
   - 加速
   - 清华源
   - tsinghua
   - install
   - mirror
cover: /asserts/pytorch/PyTorch.webp
banner: /asserts/pytorch/PyTorch.webp
poster:
   headline: 'Pytorch 镜像安装教程（Win10/11）'
   caption: '使用清华源镜像安装最新版 pytorch，含 CPU 版和 GPU 版的详细安装步骤'
---

{% quot 一键安装 %}

如果你已经对 Pytorch 安装有所了解，只想立刻安装最新版镜像包，那么在这里{% mark 选择你需要的 Pytorch 版本 %}，进入命令行环境运行我提供的命令即可：

{% tabs active:1 %}

<!-- tab 最新 CPU 版 Pytorch 安装 -->

{% copy pip install torch torchvision torchaudio -i https://pypi.tuna.tsinghua.edu.cn/simple | sh prefix:$ %}

<!-- tab GPU 版 Pytorch 2.3.1 安装 -->

{% note 警告！ 对于 GPU 版的 Pytorch，这里提供的命令仅支持“文章发布时的最新版”（2.3.1）。如若不能满足你的需求，文章的后续内容还提供了 GPU 版 Pytorch 的镜像安装命令的详细定制步骤，阅读后你可以自行安装任意版本的 Pytorch 镜像。 %}

{% tabs active:1 %}

<!-- tab Pytorch 2.3.1 - CUDA 11.8 -->
{% copy conda install pytorch=2.3.1=py3.12_cuda11.8_cudnn8_0 torchvision=0.18.1=py312_cu118 torchaudio=2.3.1=py312_cu118 pytorch-cuda=11.8 -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64 -c nvidia | sh prefix:$ %}

<!-- tab Pytorch 2.3.1 - CUDA 12.1 -->
{% copy conda install pytorch=2.3.1=py3.12_cuda12.1_cudnn8_0 torchvision=0.18.1=py312_cu121 torchaudio=2.3.1=py312_cu121 pytorch-cuda=12.1 -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64 -c nvidia | sh prefix:$ %}

{% endtabs %}

{% endtabs %}

{% quot 前言 %}

{% folding open:false 不得不吐槽一下网上的灌水教程！ color:blue %}

这篇文章本不应该出现在网络上，但考虑到本人在查阅镜像安装 Pytorch 时的种种痛苦经历：

- 官方 Pytorch 安装速度慢如蜗牛，几十 KB 的速度实在扎心
- CSDN 上的国内镜像安装教程多而无用：
  - 要么不适用于新版本；
  - 要么安装后是一看是 CPU 版本；
  - 要么根本就是换汤不换药，剖开一看还是下载了官方版的 Pytorch

这些 Pytorch 镜像安装教程往往使读者感到一头雾水、“有力使不出”，容易挫败初学者的学习动力。其中，[CSDN](https://blog.csdn.net/) 和[腾讯云开发社区](https://cloud.tencent.com/developer)更是垃圾教程的重灾区，这两个网站提供了搜索结果中近乎 99% 的水文，但却对读者毫无帮助。

本人更是深受其扰，最终痛定思痛，决定写成一篇真正实用的 Pytorch 清华源镜像安装教程，以供广大 Pytorch 学习者查阅使用。

{% endfolding %}

{% quot 在此声明，我撰写本文，目的有三： icon:default %}

{% folders %}

<!-- folder 其一，打击 CSDN 上的所谓的“快速镜像安装教程”！ -->

这类劣质文章在搜索页面中排在前列，表面上打着“清华源镜像加速”的旗号，实则却是挂羊头卖狗肉。一旦读者按其中步骤做下去，就会发觉：这根本还是安装了官方版本的 Pytorch，文章吹嘘的“快速下载”更是无稽之谈。

> 点名批评《[GPU 版本 pytorch（Cuda12.1）清华源快速安装一步一步教！小白教学~](https://blog.csdn.net/cyy0789/article/details/131137525)》，浪费网友时间！

<!-- folder 其二，遏制“无 CUDA 版本教程”的不良影响！ -->

相比第一类“教程”，这一类的教程并非完全一无是处。但这些文章也仅有那么一个优点，那就是：做到了让我们使用清华源镜像“成功安装”Pytorch。

问题在于，这些版本的 Pytorch 无法使用 GPU 加速。如果你看到的文章连“CUDA”的相关字样都没有出现，那么恭喜你，你十有八九是中奖了。当然，对于电脑中没有 N 卡（Nvidia 显卡）的选手而言，这个版本的 Pytorch 就很合适了。

<!-- folder 其三，取缔“老旧废”教程！ -->

这类教程在某些年代当然还是有用的，遗憾的是，只有顽固派还愿意坚持使用相关版本的 Pytorch。随着 AI 技术日新月异，老版本的 Pytorch 已注定被淘汰。如果不是太多的垃圾教程无法帮助读者快速安装 Pytorch，谁还会选择委屈自己用旧货呢？

{% endfolders %}

{% quot 读者自查 %}

{% folding open:false 读懂这篇文章，我需要哪些预备知识？ color:green %}

{% checkbox checked:true 了解自己的电脑显卡生产商（Nvidia / AMD / Intel） %}
{% checkbox checked:true 了解 pytorch %}
{% checkbox checked:true 了解镜像下载 %}
{% checkbox checked:true 能简单使用 pip 包管理工具 %}
{% checkbox checked:true 能简单使用 conda 管理自己 python 环境 %}
{% endfolding %}

Pytorch 共有 CPU 版和 GPU 版两个发行版本。GPU 版本一般运算速度更快，但仅限电脑有较新年份 Nvidia 显卡的用户才能安装使用。

在开始安装前，你需要先了解自己的电脑适合安装 CPU 还是 GPU 版的 Pytorch。

{% quot 使用清华源镜像安装Pytorch（Win10/11） %}

{% tabs active:1 %}

<!-- tab 无 Nvidia 显卡者，入此门 -->

本文提供了 pip 和 conda 两种工具的 CPU 版 Pytorch 镜像安装方式。

{% tabs active:1 %}

<!-- tab pip 安装 -->

{% copy pip install torch torchvision torchaudio -i https://pypi.tuna.tsinghua.edu.cn/simple prefix:$ %}
 
<!-- tab conda 安装 -->

{% copy conda install pytorch torchvision torchaudio -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64 prefix:$ %}

{% endtabs %}

<!-- tab 有 Nvidia 显卡者，入此门 -->

{% quot 安装 CUDA Toolkit %}

{% note 提示 接下来的内容是对电脑中拥有 Nvidia 显卡的用户而言的，如果你的电脑使用的是 Intel 集成显卡或者 AMD 显卡，安装 CPU 版的 Pytorch 即可。 color:green %}

在安装之前，我建议你了解一些概念名词，以更快地理解本文。

{% folders %}

<!-- folder 什么是 CUDA？ -->

CUDA，英文全程是 Compute Unified Device Architecture，中文翻译则是“通用并行计算架构”。

受益于 CUDA 架构，Pytorch 等深度学习框架得以实现加速计算，极大地推动了深度学习革命的到来。

<!-- folder 什么是 CUDA Toolkit？ -->

CUDA Toolkit，也叫 CUDA 工具包，是由 Nvidia 显卡生产商提供的基于 CUDA 架构的驱动程序。

| CUDA | CUDA Toolkit |
| --- | --- |
| 针对显卡提出的计算架构 | Nvidia 厂商为自家显卡开发的计算驱动程序 |

为了和广大 Pytorch 用户的普遍称呼相统一，下文将不再区分 CUDA 和 CUDA Toolkit 两个术语，请读者自行甄别。

<!-- folder GPU 版的 Pytorch 为什么还有两个细分版本？ -->

Pytorch为了提高推理计算速度，基于 CUDA Toolkit 的两个版本（11.8 版和 12.1 版）分别实现了专供 Nvidia 显卡的 Pytorch 版本。下称为 `pytorch-cu118` 和 `pytorch-cu121`。

未加速的 Pytorch 包只能使用 CPU 计算，无法利用显卡的并行计算能力，因此也叫做 CPU 版本的 Pytorch，下称为 `pytorch-cpu`。

| `pytorch-cu118` | `pytorch-cu121` | `pytorch-cpu` |
| --- | --- | --- |
| 针对 CUDA Toolkit 11.8 版本开发 | 针对 CUDA Toolkit 12.1 版本开发 | 通用版本，只使用 CPU 进行推理计算 |

{% endfolders %}

{% folding open:false 当前版本的 Pytorch（2.3.1）对显卡使用的 CUDA 版本要求更加严格。 %}

先前版本的 Pytorch 尽管只基于两个版本的 CUDA 开发，但对众多 CUDA 版本的支持是向上兼容的。不过，最新版 Pytorch 缩小了兼容的版本范围。

| CUDA Toolkit版本 | 旧版 Pytorch 兼容 | 新版 Pytorch（2.3.1）兼容 |
| --- | --- | --- |
| 11.8 ~ 12.0 | `pytorch-cu118` 支持从 11.8 到 12.0 所有版本的 CUDA | `pytorch-cu118` 仅支持 11.8 和 11.9 两个版本的 CUDA |
| 12.1 + | `pytorch-cu121` 支持 CUDA 12.1+ | `pytorch-cu121` 仅支持 CUDA 12.1，高版本不再兼容 |

{% folding open:false 忽略 CUDA 版本限制强行安装，可能会导致报错! %}

```shell
# pytorch-cu121 可能报错：
- nothing provides cuda-cudart >=12.1,<12.2 needed by pytorch-cuda-12.1-hde6ce7c_5
# pytorch-cu118 可能报错：
- nothing provides cuda-cudart >=11.8,<12.0 needed by pytorch-cuda-11.8-h24eeafa_3
```

```shell
# 其它可能的报错输出：
Could not solve for environment specs
The following packages are incompatible
├─ pytorch-cuda 12.1**  is not installable because it requires
│  └─ cuda-cudart >=12.1,<12.2 , which does not exist (perhaps a missing channel);
└─ pytorch ==2.3.1 py3.12_cuda11.8_cudnn8_0 is not installable because it requires
   └─ pytorch-cuda >=11.8,<11.9  but there are no viable options
      ├─ pytorch-cuda 11.8 would require
      │  └─ cuda-cudart >=11.8,<12.0 , which does not exist (perhaps a missing channel);
      └─ pytorch-cuda 11.8 would require
         └─ cuda 11.8.* , which does not exist (perhaps a missing channel).
```

{% endfolding %}

{% endfolding %}

一言以蔽之：你需要在确认电脑显卡支持的前提下，安装 11.8 或 12.1 两个版本之一的 CUDA。11.9 版本的 CUDA 其实也是可以用的，但是为了预防以后也被淘汰，我还是建议你安装 11.8 或 12.1 两个版本的 CUDA。

{% quot 手把手带你安装CUDA icon:hashtag %}

1. 查看 N 卡（Nvidia 显卡）支持的最高 CUDA 版本

进入命令行环境，运行如下命令：

```shell
nvidia-smi
```

输出结果是一张表格，在表头中找到 `CUDA Version` 项，确认显卡支持的最高 CUDA 版本，根据下表查询你接下来会用到的几个重要信息。

| CUDA Version | 动作 |
| --- | --- |
| < 11.8 | 回到上文安装 `pytorch-cpu`，你的显卡版本过低 |
| >= 11.8, < 12.1 | 先安装 CUDA 11.8，再安装 `pytorch-cu118` |
| >= 12.1 | 先安装 CUDA 12.1，再安装 `pytorch-cu121` |

这里记住了你需要的 CUDA Toolkit 版本和 Pytorch 版本之后，再进行下一步。

2. 访问 [CUDA Toolkit Archive](https://developer.nvidia.com/cuda-toolkit-archive) 页面，下载你所需的 CUDA Toolkit 版本

{% note 注意 对于诸多 CUDA 小版本，只需 CUDA 大版本号满足要求即可，各小版本号任意选择。另外这个网站下载速度挺快的，等几分钟就完事了。 color:green %}

3. 运行 CUDA 安装程序，完成安装

{% quot 镜像安装 GPU 版 Pytorch %}

下面介绍在 conda 环境下安装 GPU 版 pytorch 镜像包的方法。

{% folding 为什么要用 conda 命令环境，pip 不行吗？ %}

还真不太行，因为国内的 pytorch 镜像下载只此清华源一家，而清华源网站不提供基于 pip 的 GPU 版 pytorch 包。因此，只能通过 conda 安装。

{% endfolding %}

{% quot 使用 Pytorch 官网查看需要安装的包名 icon:hashtag %}

进入[官网安装页面](https://pytorch.org/get-started/locally/)，在快速安装配置表中，选择你的相关环境设置，记得 Package 一栏选择 Conda。

如果你需要安装 `pytorch-cu118`，Compute Platform 一栏选择 CUDA 11.8 即可；`pytorch-cu121`类似。

以安装最新版（2.3.1）的 `pytorch-cu121` 为例，选择完成后将出现如下命令

```shell
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
```

可以看出，该命令总共安装了 pytorch、torchvision、torchaudio、pytorch-cuda 四个包，并且指定了 pytorch-cuda 为 12.1 版本。

后面的 `-c pytorch`，`-c nvidia` 分别指定了从 pytorch 官方仓库和 nvidia 官方仓库查找各个包的下载链接。

{% quot 在清华源镜像中搜索各个包的国内下载链接 icon:hashtag %}

接下来，我们要在清华源的镜像仓库中查找 pytorch、torchvision、torchaudio、pytorch-cuda 四个包的镜像 build 编号。知道 build 编号后，就能实现快速镜像下载了。

而除去 pytorch-cuda 之外，其余三个包都需要明确版本号才能开始查找对应的 build 编号。

下以 pytorch 包为例演示该包的 build 编号查找过程。

{% folding 到底要怎么知道我需要的 pytorch 版本对应哪个版本的 pytorch、torchvision、torchaudio 包呢？ %}

最新版的 pytorch 发布版本号官网主页会显示出来，接下来再根据官方的发布版本号确认四个包的版本。

访问[清华源 Anaconda 镜像中的 Pytorch 包列表](https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64/)，在结果栏目的表头上点击 Date 旁边的箭头，使结果按日期排列。
 
对于同一日期发布的各个包，以 pytorch 打头的文件名包含的版本号就是 pytorch 包对应的版本，而同一天发布的以 torchvision 打头的文件名包含的版本号就是对应的 torchvision 版本。torchaudio 类似。

pytorch-cuda的版本号只能是 11.8 或者 12.1，这个在先前的步骤中应该已经明确了。

{% endfolding %}

本文中，查到 pytorch 包的版本就是 2.3.1，随后运行该命令在镜像仓库中搜索 2.3.1 版本的 pytorch 包：

{% copy conda search pytorch=2.3.1 -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64 prefix:$ %}

得到如下结果：

```shell         
# Name                       Version           Build  Channel
pytorch                        2.3.1    py3.10_cpu_0  anaconda/cloud/pytorch
pytorch                        2.3.1 py3.10_cuda11.8_cudnn8_0  anaconda/cloud/pytorch
pytorch                        2.3.1 py3.10_cuda12.1_cudnn8_0  anaconda/cloud/pytorch
pytorch                        2.3.1    py3.11_cpu_0  anaconda/cloud/pytorch
pytorch                        2.3.1 py3.11_cuda11.8_cudnn8_0  anaconda/cloud/pytorch
pytorch                        2.3.1 py3.11_cuda12.1_cudnn8_0  anaconda/cloud/pytorch
pytorch                        2.3.1    py3.12_cpu_0  anaconda/cloud/pytorch
pytorch                        2.3.1 py3.12_cuda11.8_cudnn8_0  anaconda/cloud/pytorch
pytorch                        2.3.1 py3.12_cuda12.1_cudnn8_0  anaconda/cloud/pytorch
pytorch                        2.3.1     py3.8_cpu_0  anaconda/cloud/pytorch
pytorch                        2.3.1 py3.8_cuda11.8_cudnn8_0  anaconda/cloud/pytorch
pytorch                        2.3.1 py3.8_cuda12.1_cudnn8_0  anaconda/cloud/pytorch
pytorch                        2.3.1     py3.9_cpu_0  anaconda/cloud/pytorch
pytorch                        2.3.1 py3.9_cuda11.8_cudnn8_0  anaconda/cloud/pytorch
pytorch                        2.3.1 py3.9_cuda12.1_cudnn8_0  anaconda/cloud/pytorch
```

这里我需要 python 3.12、cuda 12.1 版本的 pytorch 包，在结果中，找到了一行满足我的要求：
```shell
pytorch                        2.3.1 py3.12_cuda12.1_cudnn8_0  anaconda/cloud/pytorch
```

可以看到，对应的 build 编号是 `py3.12_cuda12.1_cudnn8_0`，为此，将该 build 编号按如下格式记录在一个临时记事本里：

```
pytorch=2.3.1=py3.12_cuda12.1_cudnn8_0
```

到这里，我们已经找到了所需 pytorch 包的 build 编号。

对其他包，也使用同样的方法查询，最终得到如下结果：

```
pytorch=2.3.1=py3.12_cuda12.1_cudnn8_0
torchvision=0.18.1=py312_cu121
torchaudio=2.3.1=py312_cu121
pytorch-cuda=12.1
```
这里 pytorch-cuda 的格式稍微不同，因为我们不需要查找它的 build 编号。

{% quot 合并临时记录，得到最终命令 icon:hashtag %}

这些临时记录实际上是按照 conda 命令格式书写的，将这些临时记录合并起来得到了最终命令：

{% copy conda install pytorch=2.3.1=py3.12_cuda12.1_cudnn8_0 torchvision=0.18.1=py312_cu121 torchaudio=2.3.1=py312_cu121 pytorch-cuda=12.1 -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64 -c nvidia prefix:$ %}

在命令行中运行该命令，即可自动开始镜像安装。完成后，简单写点代码验证一下是否安装成功。

{% endtabs %}

{% note 提示 `torchaudio` 是专用于音频深度学习的包，多数用户都不会用到。如果你用不到它，记得在命令中去掉该条目，以免占用电脑存储空间。 color:green %}
