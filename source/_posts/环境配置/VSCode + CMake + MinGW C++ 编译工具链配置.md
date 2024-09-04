---
title: VSCode + CMake + MinGW C++ 工具链配置
subtitle: Windows 环境下使用 VSCode 组织 C++ 工程，并通过 CMake 调用 MinGW 编译（不使用 MSVC 编译器）
date: 2024-08-18 16:47:12
categories:
  - 环境配置
tags:
  - VSCode
  - CMake
  - MinGW
references:
  - "[Upppping,用VSCode和CMake编写调试C/C++,2019-07-22](https://www.jianshu.com/p/c3806d2ad1f8)"
  - "[svchao,windows下使用Mingw执行make编译,2021-08-26](https://www.cnblogs.com/svchao/p/15189660.html)"
  - "[VSCode CMake Tools 文档](https://github.com/microsoft/vscode-cmake-tools/blob/main/docs)"
---

<p id='brief'>Windows 环境下使用 VSCode 组织 C++ 工程，并通过 CMake 调用 MinGW 编译（不使用 MSVC 编译器）</p>

<!-- more -->
<script>document.getElementById('brief').remove();</script>

## 前言

在用 VSCode 官方提供的 C/C++ 插件编译代码时，经常碰到以下两个常见的问题：

{% timeline color:white %}
<!-- node 其一 -->
该插件的编译任务执行时间过长，效率低下。
<!-- node 其二 -->
有时我只希望单独运行程序，但无论如何配置，VSCode 每次运行都会强制打开调试功能。
{% endtimeline %}

当然，用 VSCode 写 C/C++ 代码也不是完全没有好处。

对于日常刷题而言，相较于 Visual Studio 或 Clion 这样的重型 IDE，我更偏向于使用 Dev-Cpp 一类的小型 IDE。但是，这些小型的 IDE 通常智能提示功能都非常有限，无法满足我的使用要求。

VSCode 兼具了小巧快捷和智能提示丰富的特性，对于运行单文件 C/C++ 项目来说再好不过了。

{% image https://code.visualstudio.com/assets/docs/cpp/cpp/cpp-extension.png C/C++ 插件 %}

现在的问题是：VSCode C/C++ 插件过于难堪的编译效率反而让“在 VSCode 上刷题”这件事变得有点鸡肋。

为了解决这个问题，让 VSCode 运行 C/C++ 的代码也能和在 Dev-Cpp 上一样快捷，是时候尝试一下 CMake 了。
## 工具介绍

{% quot CMake - 一款流行的跨平台自动编译工具 icon:default %}

CMake 本身是一款跨平台的 C/C++ 自动编译工具，只需要简单的几行代码配置，就能让庞杂的项目组织按设定生成程序。

对于前文提到的问题来说，CMake 在组织项目信息的同时，还能将一些耗时较长的编译准备工作的临时信息也保存起来，后续再次运行项目就不需要重复这部分工作。这一点极大地提高了项目的编译效率，比起 VSCode C/C++ 插件的编译速度那简直是一个天上一个地下。

下面进入 VSCode + CMake 配置环节。
## 安装 CMake 及其插件

使用 CMake 有几个前提：你的电脑需要有一个编译器（MSVC、GCC 或 CLang），一个项目生成器（Make 或 Visual Studio Generator）。

我一般是在 Windows 上刷题写 C/C++，但为了和测试平台保持一致使用 GCC 编译器，所以我的电脑上已经安装了 MinGW（Windows 版 GCC）编译器，并且 VSCode 也安装了 C/C++ 插件。{% mark 不了解这一步的可以参考 %}[官方教程](https://code.visualstudio.com/docs/cpp/config-mingw)。

MinGW 还自带了一个 mingw32-make，这实际上就是一个改了名字的 Make，可以作为 CMake 所需的生成器。

也就是说，如果你的 VSCode 和 MinGW 都安装好了，那么下面就只需要专注于 CMake 的安装配置。

这一步完成后，尝试以下两行代码，检查你的 MinGW 和 mingw32-make 是否可用。

{% copy gcc --version prefix:$ %}

{% copy mingw32-make --version prefix:$ %}

{% quot 安装 CMake icon:hashtag %}

{% note 提醒一下 这里我们只介绍 WIndows 平台 VSCode 使用 MinGW 的例子。

假如你要在 VSCode 配套使用 MSVC 编译器，直接用 Visual Studio 才是正确的操作。  

至于 Linux 和 Mac 平台的用户，我相信他们大概率不至于遇到像 Windows 用户这样“诡异”的需求。对于这些平台的大型项目开发来说，XCode 和 CLion 要比本文的方案简单一万倍。 color:blue %}

为了安装 CMake，可以选择访问 [CMake 官网下载页面](https://cmake.org/download/)，下载 .msi 安装程序。

但要是你的电脑上因为某种特殊原因已经有了 2017 或更高版本的 Visual Studio，那么就不要再去官网下载 CMake 了。因为你可以在 Visual Studio 中安装的【使用C++的桌面开发】工具包，包里附带了一个 CMake，并且这个 CMake 可以被 Visual Studio 和 VSCode 共用。

{% image /asserts/images/VSCode+CMake+C++/VS安装CMake.jpg 安装【使用C++的桌面开发】 %}

如果你是通过官网下载的 CMake，可以运行下行命令验证。

{% copy cmake --version prefix:$ %}

{% note 特别提醒 从 Visual Studio 安装 CMake 后运行这段命令，会提示无法识别 cmake 命令，但 CMake 其实已经被安装了。你可以在文件管理器里搜索 cmake.exe，如果存在那就没问题。 color:red %}

安装后的 CMake 本身只是一个工具包，直接使用不够方便。而 VSCode 插件市场中正好有两个 CMake 配套的必备插件：一款是由 twxs 开发的 CMake；另一款是微软官方提供的 [CMake Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools)。

CMake 插件能够提供 CMake 配置文件的语法高亮和自动补全。CMake Tools 能够自动识别并关联到系统中的 CMake 工具，并提供快捷的 CMake 配置文件生成。

在 VSCode 中搜索安装这两个插件。
## 创建 CMake 工程

新建一个 `demo` 文件夹，用 VSCode 打开。

{% timeline color:white %}
<!-- node 第一步 -->
打开 VSCode 命令面板（按 F1），输入 CMake: Quick Start 并选择。
<!-- node 第二步 -->
在弹出栏中输入你的工程名，这里可以输入 demo。
<!-- node 第三步 -->
在弹出栏中选择 C++ 工程。
<!-- node 第四步 -->
在弹出栏中勾选 CTest 并确认。
<!-- node 第五步 -->
在弹出栏中选择 Executable。
<!-- node 第六步 -->
在弹出栏中选择添加预设。
<!-- node 第七步 -->
在弹出栏中选择从编译器创建预设。
<!-- node 第八步 -->
在弹出栏中选择 MinGW 编译器。
<!-- node 第九步 -->
在弹出栏中为新预设创建一个名称，这里可以输入 g++。
{% endtimeline %}

接着该项目下就会多出 `main.cpp`、`CMakeLists.txt` 和 `CMakePresets.json` 三个文件。`CMakeLists.txt` 定义了该项目的代码组织结构，而 `CMakePresets.json` 定义了该项目的环境配置。

由于我的需求只是单文件运行，所以无需改动前者。如果你想要进一步学习，建议查阅其他教程或官方文档。

打开 `CMakePresets.json`，可以看到如下内容：
```json
{
    "version": 8,
    "configurePresets": [
        {
            "name": "g++",
            "displayName": "GCC 13.2.0 x86_64-w64-mingw32",
            "description": "使用编译器: C = D:\\MSY64\\ucrt64\\bin\\gcc.exe, CXX = D:\\MSY64\\ucrt64\\bin\\g++.exe",
            "binaryDir": "${sourceDir}/out/build/${presetName}",
            "cacheVariables": {
                "CMAKE_INSTALL_PREFIX": "${sourceDir}/out/install/${presetName}",
                "CMAKE_C_COMPILER": "D:/MSY64/ucrt64/bin/gcc.exe",
                "CMAKE_CXX_COMPILER": "D:/MSY64/ucrt64/bin/g++.exe",
                "CMAKE_BUILD_TYPE": "Debug"
            }
        }
    ]
}
```

这里分别指定了我的 MinGW 工具的 gcc 和 g++ 程序作为 C/C++ 编译器。

但我们还需要指定 ming32-make 作为 CMake 的生成器，因此修改该文件为如下内容：
```json
{
    "version": 8,
    "configurePresets": [
        {
            "name": "g++",
            "displayName": "GCC 13.2.0 x86_64-w64-mingw32",
            "description": "使用编译器: C = D:\\MSY64\\ucrt64\\bin\\gcc.exe, CXX = D:\\MSY64\\ucrt64\\bin\\g++.exe",
            "binaryDir": "${sourceDir}/out/build/${presetName}",
            "architecture": {
                "strategy": "external"
            },
            "toolset": {
                "strategy": "external"
            },
            "generator": "MinGW Makefiles",
            "cacheVariables": {
                "CMAKE_INSTALL_PREFIX": "${sourceDir}/out/install/${presetName}",
                "CMAKE_BUILD_TYPE": "Debug"
            }
        }
    ]
}
```

修改后，文件指定了使用 MinGW 提供的 mingw32-make 作为生成器，并利用该生成器自动查找 gcc 和 g++ 命令，所以我删去了之前生成的 gcc 和 g++ 的两行配置。

当然不删也是可以的，只是这么做更加简洁一些，并且配置里面就不存在路径了，所有路径都是自动检测的，你可以复制到自己的项目里直接用。

到这里项目就配置完毕了，要运行或者调试该项目，点击 VSCode 下方状态栏左侧多出的运行和调试图标即可运行。{% mark 注意不要找错位置了，VSCode 侧边栏以及文件右上角的运行图标都不是 CMake 提供的！ color:yellow %}

最后补充一点，这个 CMake 插件的调试功能其实也不太好用，也就运行功能很方便了。如果你在调试时遇到经常无法运行程序的问题，建议还是换成 C/C++ 插件的调试功能；或者不使用 CMake Tools 插件，参阅[其他教程](https://www.jianshu.com/p/c3806d2ad1f8)配置 CMake 调试任务。

<center>{% emoji blobcat ablobcatrainbow height:1em %}{% emoji blobcat ablobcatrainbow height:2em %}{% emoji blobcat ablobcatrainbow height:3em %}{% emoji blobcat ablobcatrainbow height:2em %}{% emoji blobcat ablobcatrainbow height:1em %}</center>

_所以说：C/C++ 和 CMake Tools 两个插件的功能实现都不太完整，两个结合着还能凑活着用一用，单独用那就有点捉襟见肘了。_
