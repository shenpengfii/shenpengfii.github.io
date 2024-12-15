---
title: QT6 + OpenCV 镜像安装教程
subtitle: 使用中科大源安装 QT Online Creator，并为项目配置 OpenCV 环境
date: 2024-10-03 13:28:38
categories:
  - programming
  - toolchain
tags:
  - QT
  - OpenCV
  - 镜像
references:
  - "[Qt 中科大镜像帮助页面](https://mirrors.ustc.edu.cn/help/qtproject.html)"
  - "[OpenCV Offitial Tutorial, Installation in Windows](https://docs.opencv.org/4.10.0/d3/d52/tutorial_windows_install.html)"
  - "[-_Matrix_-,Qt配置OpenCV教程，亲测已试过（详细版）,2021-01-22](https://blog.csdn.net/weixin_43763292/article/details/112975207)"
  - "[kimi,CMakeLists.txt文件修改以支持OpenCV,2024-10-04](https://kimi.moonshot.cn/chat/crvnfsatnn0p5v3euqag)"
---

<p id='brief'>使用中科大源安装 QT Online Creator，并为项目配置 OpenCV 环境</p>

<!-- more -->
<script>document.getElementById('brief').remove();</script>

## 安装 QT6

进入 [QT6 中科大镜像源](https://mirrors.ustc.edu.cn/qtproject/official_releases/online_installers/) 下载对应系统版本的安装包。

以 Windows 系统安装为例，进入命令行，执行该命令启动安装程序：

{% copy qt-unified-windows-x86-online.exe --mirror https://mirrors.ustc.edu.cn/qtproject %}

该命令配置了使用中科大源进行下载安装。

进入安装页面后，请勿选择默认安装方案，而是使用自定义组件方式安装，这样做可以最大化节省程序占用存储空间。

{% image /asserts/images/QT6+OpenCV/MaintainanceTool.png 安装页面截图 %}

进入自定义组件界面后，只需要关注 `\QT` 目录下的组件包，其余部分可以先保持默认，除非项目有其他特殊需求。

在 `\QT` 目录下，选择一个偏好的 QT 版本包，这里我使用 `QT 6.7.2`。

在这个包目录中，可以选择具体编译平台所需的预编译套件。为了支持 QT 的跨平台特性，我希望使用 MinGW 编译方案，因此我仅勾选了对应的 MinGW 预编译套件。另外，不要忘记在同级的 `\Developer and Designer Tools` 目录下，还需安装对应版本的 MinGW 编译器。

{% note 警告 这里不应该选择其他版本的 MinGW 编译器，测试证明这容易引发未知的编译器报错。 color:red %}

{% note 提示 除此之外，如果你还希望使用的是 MSVC 编译方案，那么就应该在 `\QT x.x.x` 目录下增加勾选对应的 MSVC 预编译套件。 color:blue %}

至此，QT6 就安装完成了。

## 安装 OpenCV

官方文档中描述了 OpenCV 在 Windows 平台有两种安装方式：一种为直接下载由官方提供的预编译包，另一种为使用 CMake 编译源码的方式安装。

由于前者仅适用于 MSVC 编译器，而我的需求是使用 QT 的 MinGW 编译方案，因此我选择使用 CMake 编译源码的方式安装 OpenCV。

在 [OpenCV Release 页面](https://opencv.org/releases/) 下载最新版本的 OpenCV Sources 压缩包，解压到一个合适的位置，例如 `D:\OpenCV`。

到这里我们已经得到了 OpenCV 的源码，接下来就需要用 **QT 内置的 MinGW 编译器和 CMake 项目组织工具**来编译生成适配的 OpenCV 静态库和动态链接库了。

{% note 提示 事实上，由于我在上一步的 QT 安装过程中，勾选了 MinGW 编译器，且 CMake 工具是 QT 自带的，因此我可以直接使用 QT 自带的 MinGW 编译器和 CMake 项目组织工具。 color:green %}

在你的 QT 安装目录下分别找到 MinGW 和 CMake 工具，在我的系统中为 `D:\QT\Tools\mingw1120_64` 和 `D:\QT\Tools\CMake_64`。

下面逐步演示如何使用 QT CMake 工具编译 OpenCV。

{% timeline color:white %}

<!-- node 第一步：打开 `cmake-gui` -->

首先进入 QT CMake 工具的 bin 目录，打开 `cmake-gui.exe`。

<!-- node 第二步：配置 OpenCV 的源码和编译目录 -->

在 `Where is the source code:` 对应的文本框中，点击右侧的 `Browse Source` 定位到 OpenCV 源码的解压目录中。

而在 `Where to build the binaries:` 对应的文本框中，你需要新建一个目录用于保存 OpenCV 编译后的库文件。这里我选择在 OpenCV 源码的同级目录下新建一个名为 `build-qt-mingw` 的目录。

{% image /asserts/images/QT6+OpenCV/cmake-guiOpenCV目录配置.png cmake-gui 截图 %}

<!-- node 第三步：配置使用 QT 内置的 MinGW 编译器 -->

点击 cmake-gui 软件左下角的 Configure 按钮，配置在编译过程中使用 QT 内置的 MinGW 编译器。

在接下来弹出的窗口中，选择使用 `MinGW Makefiles`，并勾选 `Specify native compilers`。

在随后弹出的窗口为，分别为 `C` 和 `C++` 指定对应的编译器软件目录，这里必须使用 QT 内置的 MinGW。在我的系统中，分别为 `D:\QT\Tools\mingw1120_64\bin\gcc.exe` 和 `D:\QT\Tools\mingw1120_64\bin\g++.exe`。

Fortran 一栏留空即可。

<!-- node 第四步：配置增加 OpenGL 和 QT 模块 -->

在软件提示 `Configure Done` 之后，从中间的键值列表中勾选 `WITH_OPENGL` 和 `WITH_QT` 两项，并重新点击左下角的 `Configure` 按钮，等待软件配置完成。

{% note 警告 这一步骤中，软件会下载来自 Github 仓库的文件，建议你启用一些 Github 加速工具，以免文件下载失败或下载时间过长。 color:yellow %}

重新配置完成后，你可能会看到一些键值表项变成了红色，这说明这部分的配置需要你手动修改为正确的值。

由于不同版本配置的标红内容有所区别，这里展示我的修改过程，你可以用于参考：

{% image /asserts/images/QT6+OpenCV/cmake-gui标红表项.png cmake-gui 标红表项 %}

我做出的修改如下：

| 表项 | 修改后的内容 |
| --- | --- |
| `QT_QMAKE_EXECUTABLE` | `D:\QT\Tools\mingw_64\bin\qmake.exe` |
| `QT5_DIR` | 这一项我直接删除了，因为我在 QT 安装时并未勾选 QT 5 组件 |
| `QT6_DIR` | `D:\QT\6.7.2\mingw_64\lib\cmake\QT6` |

修改完成后，重新点击左下角的 `Configure` 按钮，等待软件配置完成。当然，即便修改后也可能会再次出现标红项，这时可以多点几次 `Configure` 按钮，直到软件不出现标红项为止。

在没有标红项后，点击左下角的 `Generate` 按钮，生成 CMake 项目配置文件。

<!-- node 第五步：执行 OpenCV 编译命令 -->

在命令行中进入之前创建的 OpenCV 编译文件目录 `build-qt-mingw`，执行以下命令：

{% copy D:\QT\Tools\CMake_64\bin\cmake.exe install prefix:$ %}

这里，`D:\QT\Tools\CMake_64\bin\cmake.exe` 是我的 QT 内置 CMake 软件位置，你可以替换为自己的 QT CMake 地址。

接下来等待执行完成即可。

{% endtimeline %}

接下来，将编译后的 OpenCV 库的 `bin` 目录添加到系统环境变量中，以供外部程序调用 OpenCV 的动态链接库。

至此，QT 所需的 OpenCV 也就正式安装完成了。

## QT6 配置 OpenCV

新建一个 QT Widget Application 项目。

{% note 警告 如果你的项目使用 MinGW 编译方案，且你的系统中存在多个版本的 MinGW，那就应该确保项目所用的工具套件中的编译器为通过 QT 安装的 MinGW。 color:red %}

由于最新版的 QT Creator 会默认使用 CMake 而非 QMake 作为项目的生成工具，因此不同于其他文章修改 QMake 配置文件，本文会修改 CMake 配置文件来为项目引入 OpenCV。

在项目的 `CMakeLists.txt` 文件中，首先找到如下两行代码：

```cmake
find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets)
```

在以上两行代码下面添加如下代码：

```cmake
# 引入 OpenCV 头文件
set(OpenCV_DIR "D:\\OpenCV\\build-qt-mingw")
find_package(OpenCV REQUIRED)
```

这里增加的代码是为了引入 OpenCV 的头文件，`D:\\OpenCV\\build-qt-mingw` 是 OpenCV 编译后的库文件目录，你需要根据自己的实际情况修改为自己的 OpenCV 库文件目录。

随后还需要引入 OpenCV 的动态链接库，找到如下代码：

```cmake
target_link_libraries(Demo PRIVATE Qt${QT_VERSION_MAJOR}::Widgets)
```

将其修改为

```cmake
target_link_libraries(Demo PRIVATE Qt${QT_VERSION_MAJOR}::Widgets ${OpenCV_LIBS})
```

现在，就可以尝试在项目中引入 OpenCV 的头文件和代码，并尝试编译运行了。