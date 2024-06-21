---
title: 比同行教程更好用，最新版Pytorch国内清华源镜像加速安装！
date: 2024-06-17 16:36:32
tags:
   - pytorch
   - 安装
   - 镜像
   - 加速
   - 清华源
   - tsinghua
   - install
   - mirror
---

# 0. 摘要

最新版的`Pytorch 2.3.1`缩减了对CUDA Toolkit的版本支持，读者需要检查系统CUDA Toolkit版本是否仍在支持范围之内，否则在安装时会出现报错情况。

其次，本文给出了conda环境中使用清华源镜像、快速安装最新版pytorch的方法介绍。

## 0.1 导读

> 读懂这篇文章，我需要哪些预备知识？
>
> 1. 能简单使用pip包管理工具
> 2. 能简单使用conda管理自己python环境
> 3. 基本了解什么是pytorch
> 4. 知道什么是镜像下载

1. 对于曾经安装过CUDA和镜像版Pytorch、想要重装却遇到问题的高级读者，请先速览[问题解答](#21-最新版pytorch缩小了对cuda的版本支持范围)，再通读全文。
2. 对于想要尝鲜、使用镜像安装pytorch的中级读者，以及初次安装pytorch、查阅大量资料但找不到镜像下载方法的初级读者，请耐心浏览全文，所有细节文中均已详述。

## 0.2 一键入库

如果你只想一键运行命令、通过清华源镜像安装最新版Pytorch系列包，可以输入如下命令运行：

1. 镜像安装CPU版本的Pytorch

```shell
# 镜像安装最新版pytorch
pip install torch torchvision torchaudio -i https://pypi.tuna.tsinghua.edu.cn/simple
```

2. 镜像安装GPU版本的Pytorch

> 注意，对于GPU版的Pytorch，这里的命令仅支持文章发布时的最新版（2.3.1）安装，否则你需要阅读文章学习如何查找各个包的最新版build编号

这里默认你已经安装了适配显卡和Pytorch的CUDA Toolkit。

- win10/win11 安装Pytorch 2.3.1 - CUDA 11.8：
```shell
conda install pytorch=2.3.1=py3.12_cuda11.8_cudnn8_0 torchvision=0.18.1=py312_cu118 torchaudio=2.3.1=py312_cu118 pytorch-cuda=11.8 -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64 -c nvidia
```

- win10/win11 安装Pytorch 2.3.1 - CUDA 12.1：
```shell
conda install pytorch=2.3.1=py3.12_cuda12.1_cudnn8_0 torchvision=0.18.1=py312_cu121 torchaudio=2.3.1=py312_cu121 pytorch-cuda=12.1 -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64 -c nvidia
```

# 1. 前情提要

这篇文章本不应该出现在网络上，但考虑到本人在查阅镜像安装Pytorch时的种种痛苦经历：

- 官方Pytorch安装速度慢如蜗牛，几十KB的速度实在扎心
- CSDN上的国内镜像安装教程多而无用：
  - 要么不适用于新版本；
  - 要么安装后是一看是CPU版本；
  - 要么根本就是换汤不换药，剖开一看还是下载了官方版的Pytorch

这些Pytorch镜像安装教程往往使读者感到一头雾水、“有力使不出”，容易挫败初学者的学习动力。其中，更以[CSDN](https://blog.csdn.net/)和[腾讯云开发社区](https://cloud.tencent.com/developer)为首，二者几乎提供了搜索结果中99%的灌水“教程”，但却对读者毫无帮助。

本人更是深受其扰，最终痛定思痛，决定写成一篇真正实用的Pytorch清华源镜像安装教程，以供广大Pytorch用户查阅使用。

---

本文旨在：

1. 打击CSDN上的所谓的“快速镜像安装教程”

这类劣质文章在搜索页面中排在前列，表面上打着“清华源镜像加速”的旗号，实则却是挂羊头卖狗肉。

一旦读者按其中步骤做下去，就会发觉：这根本还是安装了官方版本的Pytorch，文章吹嘘的“快速下载”更是无稽之谈。

> 点名批评《[GPU版本pytorch（Cuda12.1）清华源快速安装一步一步教！小白教学~](https://blog.csdn.net/cyy0789/article/details/131137525)》，浪费网友时间！

2. 遏制“无CUDA版本教程”的不良影响

相比第一类“教程”，这一类的教程并非完全一无是处。

它们有且仅有一个优点，那就是：做到了让我们使用清华源镜像“成功安装”Pytorch。

问题在于，这些版本的Pytorch无法使用GPU加速。如果你看到的文章连“CUDA”的相关字样都没有出现，那么恭喜你，你十有八九是中奖了。

当然，对于电脑中没有N卡（Nvidia显卡）的选手而言，这个版本的Pytorch就很合适了。

3. “总把新桃换旧符”，取缔老旧废教程

这类教程在某些年代当然还是有用的，遗憾的是，只有顽固派还愿意坚持使用相关版本的Pytorch。

随着AI技术日新月异，老版本的Pytorch已注定被淘汰。

如果不是太多的垃圾教程无法帮助读者快速安装Pytorch，谁还会选择委屈自己用旧货呢？

# 2. 安装CUDA Toolkit

本节是对电脑中拥有Nvidia显卡的用户而言的，如果你的电脑使用的是Intel集成显卡或者AMD显卡，请跳过这个步骤，直接查看[无nvidia显卡者请入此门](#31-无nvidia显卡者请入此门)。

---

- CUDA，英文全程是Compute Unified Device Architecture，中文翻译则是“通用并行计算架构”。

受益于CUDA架构，Pytorch等深度学习框架得以实现加速计算，极大地推动了深度学习革命的到来。

- CUDA Toolkit，也叫CUDA工具包，是由Nvidia显卡生产商提供的基于CUDA架构的驱动程序。

| CUDA | CUDA Toolkit |
| --- | --- |
| 针对显卡提出的计算架构 | Nvidia厂商为自家显卡开发的计算驱动程序 |

Pytorch为了提高推理计算速度，基于CUDA Toolkit的两个版本（11.8版和12.1版）分别实现了专供Nvidia显卡的Pytorch版本。下称为`pytorch-cu118`和`pytorch-cu121`。

未加速的Pytorch包只能使用CPU计算，无法利用显卡的并行计算能力，因此也叫做CPU版本的Pytorch，下称为`pytorch-cpu`。

| `pytorch-cu118` | `pytorch-cu121` | `pytorch-cpu` |
| --- | --- | --- |
| 针对CUDA Toolkit 11.8版本开发 | 针对CUDA Toolkit 12.1版本开发 | 通用版本，只使用CPU进行推理计算 |

> 为了和广大Pytorch用户的普遍称呼相统一，下文将不再区分CUDA和CUDA Toolkit两个术语，请读者自行甄别。

## 2.1 最新版Pytorch缩小了对CUDA的版本支持范围

当前版本的Pytorch（2.3.1）对显卡使用的CUDA版本要求更加严格。

> 之前版本的Pytorch尽管只基于两个CUDA版本开发，但对众多CUDA版本的支持是向上兼容的。不过，最新版Pytorch缩小了兼容的版本范围。

| CUDA Toolkit版本 | 旧版Pytorch兼容 | 新版Pytorch（2.3.1）兼容 |
| --- | --- | --- |
| 11.8 ~ 12.0 | `pytorch-cu118` 支持从11.8到12.0所有版本的CUDA | `pytorch-cu118` 仅支持11.8和11.9两个版本的CUDA |
| 12.1 + | `pytorch-cu121` 支持CUDA 12.1+ | `pytorch-cu121` 仅支持CUDA 12.1，高版本不再兼容 |

忽略CUDA版本限制，强行安装，可能会导致如下报错：
```shell
# 对于pytorch-cu121
- nothing provides cuda-cudart >=12.1,<12.2 needed by pytorch-cuda-12.1-hde6ce7c_5
# 对于pytorch-cu118
- nothing provides cuda-cudart >=11.8,<12.0 needed by pytorch-cuda-11.8-h24eeafa_3
```

```shell
# 相关报错切片
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

## 2.2 手把手带你安装CUDA

1. 查看N卡（Nvidia显卡）内置的CUDA版本

> 通常情况下，Nvidia显卡出厂会内置某个版本的CUDA驱动。
> 
> 如果你重装了系统，或者安装了双系统，则有可能导致显卡的驱动程序丢失，从而也同时丢失了CUDA驱动程序。这种情况下，请先安装Nvidia Panel恢复显卡驱动。
>
> 如果你想用虚拟机安装Pytorch，请直接安装`pytorch-cpu`，因为虚拟机无法直接使用宿主机的显卡。

进入命令行环境，运行如下命令：

```shell
nvidia-smi
```

输出结果是一张表格，在表头中找到`CUDA Version`项，并确认显卡内置的CUDA版本。

如果版本号是`11.8`, `11.9`或`12.1`三者其一，则可跳过本节，安装适配版本的Pytorch即可；否则就留在这一节继续。

| CUDA版本 | 动作 |
| --- | --- |
| < 11.8 | 跳过本节，直接安装`pytorch-cpu` |
| 11.8或11.9 | 跳过本节，直接安装`pytorch-cu118` |
| 12.0 | 继续，安装CUDA 11.8后，再安装`pytorch-cu118` |
| 12.1 | 跳过本节，直接安装`pytorch-cu121` |
| 12.2, 12.3, 12.4... | 继续，安装CUDA 12.1后，再安装`pytorch-cu121` |

2. 访问[CUDA Toolkit Archive](https://developer.nvidia.com/cuda-toolkit-archive)页面，下载你的系统所需的CUDA版本安装包

> 注意：对于诸多CUDA小版本，只需CUDA大版本号满足要求即可，各小版本号任意选择。
>
> 另外，这个网站下载速度挺快的，不需要另找第三方下载了。

3. 运行CUDA安装程序，完成安装

# 3. 在conda环境下，使用清华源镜像加速Pytorch安装

## 3.1 无Nvidia显卡者，请入此门

许多“教程”撰写者涉猎较浅，只知道如何用清华源安装`pytorch-cpu`，因此只提供了安装`pytorch-cpu`的方法。

为了满足更多读者的需求，本文也分别提供了通过pip和conda两种方式使用清华源镜像加速。

### 3.1.1 pip安装

```shell
# 安装最新版pytorch
pip install torch torchvision torchaudio -i https://pypi.tuna.tsinghua.edu.cn/simple
```

> `torchaudio`是专用于音频深度学习的包，多数用户都不会用到。如果你用不到它，记得在命令中去掉该条目，为电脑节省存储空间。

如果要安装指定版本，如安装0.1.0版本的torch，将`torch`改写为`torch==0.1.0`即可。关于更多pip书写格式，请参阅[PIP官方文档](https://pip.pypa.io/en/stable/)。

> 怎么知道我要用的pytorch版本对应哪个版本的torch、torchvision、torchaudio？
>
> 最新版的pytorch版本号访问官网即可查到，再根据官方版本号确认各个包的版本。
>
> 访问[清华源Anaconda镜像中的Pytorch包列表](https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64/)，在结果栏目的表头上点击Date旁边的箭头，使结果按日期排列。
> 
> 对于同一日期发布的各个包，以pytorch打头的文件名包含的版本号就是torch包对应的版本，而同一天发布的以torchvision打头的文件名包含的版本号就是对应的torchvision版本。torchaudio类似。
 
### 3.1.2 conda安装

对于windows用户，请直接运行下行命令：

```shell
# 安装最新版pytorch
conda install pytorch torchvision torchaudio -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64
```

其他系统的用户可以进入命令中的网页，并返回上一级网址，再查看自己的系统对应的链接格式。

不同于pip，在conda中指定包的安装版本，需要使用`=`符号而非`==`。

> 尽管conda也支持`==`格式，但使用`=`格式的conda支持扩展写法，建议你使用[conda官方文档](https://docs.conda.io/en/latest/)的书写规范。

## 3.2 有Nvidia显卡者，请入此门

下面介绍使用conda安装GPU版pytorch包的方法。

> 为什么要用conda命令环境，pip不行吗？
> 
> 还真不太行，因为国内的pytorch镜像下载只此清华源一家，而清华源网站不提供基于pip的GPU版pytorch包。因此，只能通过conda安装。

### 3.2.1 使用Pytorch官网查看需要安装的包名

进入[官网安装页面](https://pytorch.org/get-started/locally/)，在快速安装配置表中，选择你的相关环境设置，记得Package一栏选择Conda。

如果你需要安装`pytorch-cu118`，Compute Platform一栏选择CUDA 11.8即可；`pytorch-cu121`类似。

以安装最新版(2.3.1)的`pytorch-cu121`为例，选择完成后将出现如下命令

```shell
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
```

可以看出，该命令总共安装了pytorch、torchvision、torchaudio、pytorch-cuda四个包，并且指定了pytorch-cuda为12.1版本。

后面的`-c pytorch`，`-c nvidia`分别指定了从pytorch官方仓库和nvidia官方仓库查找各个包。

### 3.2.2 在清华源镜像中逐个搜索各个包

这一步，我们要在镜像仓库逐个查找pytorch、torchvision、torchaudio、pytorch-cuda四个包，并找到我们需要的GPU版本包对应的build编号。

下以pytorch包为例，先确认我们需要的pytorch包的版本（四个包的版本一般不相同）

> 怎么知道我要用的pytorch版本对应哪个版本的pytorch、torchvision、torchaudio？
>
> 最新版的pytorch版本号访问官网即可查到，再根据官方版本号确认四个包的版本。
>
> 访问[清华源Anaconda镜像中的Pytorch包列表](https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64/)，在结果栏目的表头上点击Date旁边的箭头，使结果按日期排列。
> 
> 对于同一日期发布的各个包，以pytorch打头的文件名包含的版本号就是pytorch包对应的版本，而同一天发布的以torchvision打头的文件名包含的版本号就是对应的torchvision版本。torchaudio类似。
>
> pytorch-cuda的版本号只能是11.8或者12.1，根据你的CUDA进行选择。

这里查到pytorch包的版本就是2.3.1，随后使用该命令在镜像仓库中搜索：

> 注意：这里使用的是windows平台的镜像链接，如果你正在使用其他操作系统，请访问命令中的网址，并返回上一级，在查询你的操作系统对应的链接格式

```shell
conda search pytorch=2.3.1 -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64
```

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

这里我需要python 3.12、cuda 12.1版本的pytorch包，在结果中，找到了一行满足我的要求：
```shell
pytorch                        2.3.1 py3.12_cuda12.1_cudnn8_0  anaconda/cloud/pytorch
```

可以看到，对应的build编号是`py3.12_cuda12.1_cudnn8_0`，为此，将该build编号按如下格式记录在一个临时记事本里：

```
pytorch=2.3.1=py3.12_cuda12.1_cudnn8_0
```

到这里，我们已经找到了所需pytorch包的build编号。

对其他包，也使用同样的方法查询，最终得到如下结果：

```
pytorch=2.3.1=py3.12_cuda12.1_cudnn8_0
torchvision=0.18.1=py312_cu121
torchaudio=2.3.1=py312_cu121
pytorch-cuda=12.1
```

### 3.2.3  合并临时记录，得到最终命令

这些临时记录实际上是按照conda命令格式书写的，将这些临时记录合并起来得到了最终命令：

```shell
conda install pytorch=2.3.1=py3.12_cuda12.1_cudnn8_0 torchvision=0.18.1=py312_cu121 torchaudio=2.3.1=py312_cu121 pytorch-cuda=12.1 -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64 -c nvidia
```

> `torchaudio`是专用于音频深度学习的包，多数用户都不会用到。如果你用不到它，记得在命令中去掉该条目，为电脑节省存储空间。