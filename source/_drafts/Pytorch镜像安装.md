---
title: Pytorch 镜像安装教程
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
cover: /asserts/pytorch/PyTorch.webp
banner: /asserts/pytorch/PyTorch.webp
poster:
   headline: 'Pytorch 镜像安装教程'
   caption: '使用清华源镜像安装最新版 pytorch，含 CPU 版和 GPU 版的详细安装步骤'
---

{% quot 使用清华源镜像安装最新版 pytorch，含 CPU 版和 GPU 版的详细安装步骤 %}

## 一键入库

如果你只想一键运行命令、通过清华源镜像安装最新版 Pytorch，进入命令行运行如下命令：

{% tabs active:1 %}

<!-- tab 最新 CPU 版 Pytorch 安装 -->
{% copy pip install torch torchvision torchaudio -i https://pypi.tuna.tsinghua.edu.cn/simple | sh prefix:$ %}

<!-- tab GPU 版 Pytorch 2.3.1 安装 -->

{% note 警告！ 对于 GPU 版的 Pytorch，这里的命令仅支持文章发布时的最新版（2.3.1）安装，否则你需要阅读文章学习如何查找各个包的最新版 build 编号。 color:red %}

请你先确认已安装了适配电脑显卡型号和 Pytorch 版本的 CUDA Toolkit，然后再进行操作；否则请通读本教程学习相关内容。

{% tabs active:1 %}

<!-- tab Pytorch 2.3.1 - CUDA 11.8 -->
{% copy conda install pytorch=2.3.1=py3.12_cuda11.8_cudnn8_0 torchvision=0.18.1=py312_cu118 torchaudio=2.3.1=py312_cu118 pytorch-cuda=11.8 -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64 -c nvidia | sh prefix:$ %}

<!-- tab Pytorch 2.3.1 - CUDA 12.1 -->
{% copy conda install pytorch=2.3.1=py3.12_cuda12.1_cudnn8_0 torchvision=0.18.1=py312_cu121 torchaudio=2.3.1=py312_cu121 pytorch-cuda=12.1 -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64 -c nvidia | sh prefix:$ %}

{% endtabs %}

{% endtabs %}

## 前言

这篇文章本不应该出现在网络上，但考虑到本人在查阅镜像安装 Pytorch 时的种种痛苦经历：

- 官方 Pytorch 安装速度慢如蜗牛，几十 KB 的速度实在扎心
- CSDN 上的国内镜像安装教程多而无用：
  - 要么不适用于新版本；
  - 要么安装后是一看是 CPU 版本；
  - 要么根本就是换汤不换药，剖开一看还是下载了官方版的 Pytorch

这些 Pytorch 镜像安装教程往往使读者感到一头雾水、“有力使不出”，容易挫败初学者的学习动力。其中，[CSDN](https://blog.csdn.net/) 和[腾讯云开发社区](https://cloud.tencent.com/developer)更是垃圾教程的重灾区，这两个网站提供了搜索结果中近乎 99% 的水文，但却对读者毫无帮助。

本人更是深受其扰，最终痛定思痛，决定写成一篇真正实用的 Pytorch 清华源镜像安装教程，以供广大 Pytorch 学习者查阅使用。

---

{% quot 在此声明，我撰写本文，目的有三： %}

其一，打击 CSDN 上的所谓的“快速镜像安装教程”！

这类劣质文章在搜索页面中排在前列，表面上打着“清华源镜像加速”的旗号，实则却是挂羊头卖狗肉。一旦读者按其中步骤做下去，就会发觉：这根本还是安装了官方版本的 Pytorch，文章吹嘘的“快速下载”更是无稽之谈。

> 点名批评《[GPU 版本 pytorch（Cuda12.1）清华源快速安装一步一步教！小白教学~](https://blog.csdn.net/cyy0789/article/details/131137525)》，浪费网友时间！

其二，遏制“无 CUDA 版本教程”的不良影响！

相比第一类“教程”，这一类的教程并非完全一无是处。但这些文章也仅有那么一个优点，那就是：做到了让我们使用清华源镜像“成功安装”Pytorch。

问题在于，这些版本的 Pytorch 无法使用 GPU 加速。如果你看到的文章连“CUDA”的相关字样都没有出现，那么恭喜你，你十有八九是中奖了。当然，对于电脑中没有 N 卡（Nvidia 显卡）的选手而言，这个版本的 Pytorch 就很合适了。

其三，取缔“老旧废”教程！

这类教程在某些年代当然还是有用的，遗憾的是，只有顽固派还愿意坚持使用相关版本的 Pytorch。随着 AI 技术日新月异，老版本的 Pytorch 已注定被淘汰。如果不是太多的垃圾教程无法帮助读者快速安装 Pytorch，谁还会选择委屈自己用旧货呢？

## 读者自查

{% folding open:false 读懂这篇文章，我需要哪些预备知识？ %}

{% checkbox checked:true 了解自己的电脑显卡生产商（Nvidia / AMD / Intel） %}
{% checkbox checked:true 了解 pytorch %}
{% checkbox checked:true 了解镜像下载 %}
{% checkbox checked:true 能简单使用 pip 包管理工具 %}
{% checkbox checked:true 能简单使用 conda 管理自己 python 环境 %}
{% endfolding %}

Pytorch 共有 CPU 版和 GPU 版两个发行版本。GPU 版本一般运算速度更快，但仅限电脑有较新年份 Nvidia 显卡的用户才能安装使用。


## 使用清华源镜像安装Pytorch

{% tabs active:1 %}

<!-- tab 无 Nvidia 显卡者，入此门 -->

本文提供了 pip 和 conda 两种工具的对应镜像安装方式。

{% tabs active:1 %}

<!-- tab pip 安装 -->

{% copy pip install torch torchvision torchaudio -i https://pypi.tuna.tsinghua.edu.cn/simple prefix:$ %}
 
<!-- tab conda 安装 -->

{% copy conda install pytorch torchvision torchaudio -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64 prefix:$ %}

其他系统的用户可以进入命令中的网页，并返回上一级网址，再查看自己的系统对应的链接格式。

不同于pip，在conda中指定包的安装版本，需要使用`=`符号而非`==`。

> 尽管conda也支持`==`格式，但使用`=`格式的conda支持扩展写法，建议你使用[conda官方文档](https://docs.conda.io/en/latest/)的书写规范。

{% endtabs %}

{% note 提示 `torchaudio`是专用于音频深度学习的包，多数用户都不会用到。如果你用不到它，记得在命令中去掉该条目，为电脑节省存储空间。 color:green %}


<!-- tab 有 Nvidia 显卡者，入此门 -->

当前版本的 Pytorch（2.3.1）对显卡使用的 CUDA 版本要求更加严格。

> 旧版 Pytorch 尽管只基于两个 CUDA 版本开发，但对众多 CUDA 版本的支持是向上兼容的。不过，最新版 Pytorch 缩小了兼容的版本范围。

| CUDA Toolkit版本 | 旧版Pytorch兼容 | 新版Pytorch（2.3.1）兼容 |
| --- | --- | --- |
| 11.8 ~ 12.0 | `pytorch-cu118` 支持从11.8到12.0所有版本的CUDA | `pytorch-cu118` 仅支持11.8和11.9两个版本的CUDA |
| 12.1 + | `pytorch-cu121` 支持CUDA 12.1+ | `pytorch-cu121` 仅支持CUDA 12.1，高版本不再兼容 |

{% folding open:false 忽略CUDA版本限制强行安装，可能会导致报错! %}

对于安装 `pytorch-cu121`

```shell
- nothing provides cuda-cudart >=12.1,<12.2 needed by pytorch-cuda-12.1-hde6ce7c_5
# 对于pytorch-cu118
- nothing provides cuda-cudart >=11.8,<12.0 needed by pytorch-cuda-11.8-h24eeafa_3
```

```shell
# 其它可能的报错输出
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

### 安装CUDA Toolkit

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

{% endtabs %}