# 实验1 初识实验环境

## 实验目的

1. 安装好实验环境，熟悉基本操作
2. 掌握包括 objdump、makefile、QEMU、Bochs 等实验工具的基本用法

## 实验内容

| 步骤 | 类型 | 内容 |
| :--- | :--- | :--- |
| Step1 | 操作 | 使用 objdump 反汇编工具 |
| Step2 | 调试 | 修正 Makefile 中的错误并尝试使用 make 构建 |
| Step3 | 操作 | 使用 QEMU/Bochs 运行 NEUOS，掌握 Bochs 断点操作 |
| 拓展 | \(选做\)操作 | 尝试虚拟机与物理机中互传文件 |

### Step 1 反汇编获取密码

        exp1/binary 程序中隐藏了一串字符，尝试运行该程序发现需要输入密码。

        查询相关资料，使用 objdump 程序查看二进制文件 exp1/binary 的 section 信息。其中有一个 section 为 “The password is ...”，请找到该密码，并运行 binary 输入密码，获取字符串。

![&#x56FE; 1-1 objdump &#x67E5;&#x770B; section &#x793A;&#x4F8B;](https://lh3.googleusercontent.com/aztmuakk_sYAngY-nXOuFIrT3InS6tgQiKri5JtyhAIdkDLSWikO166chSjAz5jeZUWN5tFv_gWrXv72jx5Chcac_f9T3HwhDcxHCr9l2Loo-L_mm4_CQ_TGXDifblY394R_a118)

### Step 2 认识 Makefile

        改写 Makefile 文件 exp1/Makefile，其中至少有 3 处_参数错误_或_语法错误_。通过查阅资料，尝试修正错误。

{% hint style="info" %}
其中一个错误与 gcc 优化参数有关。
{% endhint %}

        ****任意写一段 C 程序，命名为 hello.c，作为被编译的源代码。在 exp1 目录下使用终端，键入

```bash
make
```

        测试 Makefile 能否正常地生成可执行文件，生成的可执行文件能否正常运行。

### Step 3 首次运行 NEUOS

        根据前一节实验介绍或本节实验提示中的内容，尝试在 QEMU 与 Bochs 中启动 [NEUOS](https://github.com/VOID001/neu-os)。NEUOS 源码位于 NEUOS Lab Environment 的 Documents 目录下。

        在 Bochs 中启动 NEUOS 时，在线性地址 0x000f7b14 处设置断点，查看调试信息。

![&#x56FE; 1-2 Bochs &#x8FD0;&#x884C;&#x81F3;&#x65AD;&#x70B9;](.gitbook/assets/image%20%285%29.png)

{% hint style="info" %}
运行到断点后，Bochs 如何继续执行指令？
{% endhint %}

## 报告要点

1. 解释如何通过 objdump 查看二进制文件的 section 信息，展示其中的密码与 binary 中隐含的字符串。
2. 找到 Makefile 中的错误并改正，解释其原因。
3. 解释如何为 Bochs 设置与删除断点，并展示 Bochs 运行至断点时的截图。

## 实验提示

### NEUOS 实验环境

      ****   本实验环境为安装了内核开发工具的 [Manjaro](https://manjaro.org/) GNOME Edition \(17.1.11\) 虚拟机镜像。在 Windows 或 Linux 上安装 [VirtualBox](https://www.virtualbox.org/wiki/Downloads) ，然后下载该虚拟机镜像（2.54 GB）。

        百度网盘链接: [https://pan.baidu.com/s/1agqKofK4zD-vMlw4uykplg](https://pan.baidu.com/s/1agqKofK4zD-vMlw4uykplg) 密码: 7375

        在 VirtualBox 菜单栏中点击“管理 - 导入虚拟电脑”以导入该镜像。

  ****      推荐使用该镜像以避免繁琐的安装以及可能存在的依赖问题。另外也可自行在 Linux 环境中安装下列工具作为实验环境。

* git
* gcc
* gdb
* make
* qemu
* bochs

       除此之外，该镜像还预装了：

* vim
* zsh（使用 oh-my-zsh）
* ibus-googlepinyin

{% hint style="info" %}
实验环境中的 oshacker 账户的默认密码是 “oshacker” 。

若在 Ubuntu 下使用 VirtualBox 启动虚拟机镜像时，出现 “kernel driver not installed” 的错误提示，[点击这里](https://askubuntu.com/questions/760671/could-not-load-vboxdrv-after-upgrade-to-ubuntu-16-04-and-i-want-to-keep-secur)。

若虚拟机运行不流畅，可在 VirtualBox 设置中调整内存大小、显存大小及处理器数量。

进入实验环境后，虚拟机的显示比例或许不正常，尝试在 VirtualBox 的“显示”设置中修改缩放率，或在 Manjaro 中修改显示设置。在桌面上右击，选择 Display Settings 即可。
{% endhint %}

{% hint style="warning" %}
不建议将系统语言设置为中文，某些软件可能出现乱码，并且调试程序时使用英文调试信息在搜索引擎中检索是必要的。

启动虚拟机需要开启 VT-x，可在 [BIOS 中设置](https://jingyan.baidu.com/article/8ebacdf0df465b49f65cd5d5.html)。如果不支持 VT-x，无法使用虚拟机。
{% endhint %}

![&#x56FE; 1-3 Manjaro Display Settings](.gitbook/assets/image%20%281%29.png)

        NEUOS Lab Environment 的桌面是空白的，但它已包含实验所需的必要工具 GCC, GNU Toolchain, dd, Makefile, Bochs, QEMU 等。左侧 Dock 栏是常用软件，打开 Files，实验材料 “neuos-material” 位于 Documents 目录中。点击左下角按钮可查看应用列表。

       **** GCC\(GNU Compiler Collection\) 是由 GNU 开发的编程语言编译器，支持的语言包括 C 语言。它是以 [GPL](https://baike.baidu.com/item/GPL) 许可证所发行的自由软件，也是 GNU 计划的关键部分。GCC 原本作为 GNU 操作系统的官方编译器，现已被大多数包括 Linux 的类 Unix 操作系统采纳为标准的编译器。

        Objdump 是显示二进制文件信息的工具。

    ****    dd 的用途为转换和复制文件。

{% hint style="info" %}
了解 dd：[IBM Knowledge Center dd命令](https://www.ibm.com/support/knowledgecenter/zh/ssw_aix_71/com.ibm.aix.cmds2/dd.htm)
{% endhint %}

       **** make 是一个工具程序，用于简化重复编译和重复链接进程。Makefile 是由 make 程序引用的文本文件，它描述了目标的构建方式，并包含诸如源文件级依赖关系以及构建顺序依赖关系之类的信息。

{% hint style="info" %}
学习 Makefile：[跟我一起写Makefile 1.0 文档](https://seisman.github.io/how-to-write-makefile/overview.html)
{% endhint %}

### Bochs

        Bochs 是一种 x86 PC 仿真器和调试器。它提供对整个 PC 平台的仿真，包括一个或多个处理器和各种不同的 PC 外围设备。Bochs 并不是现代意义上的虚拟化，而是模拟。

![&#x56FE; 1-4 Bochs &#x8C03;&#x8BD5;&#x754C;&#x9762;](.gitbook/assets/image%20%282%29.png)

{% hint style="info" %}
了解 Bochs：[使用 Bochs 进行平台仿真](https://www.ibm.com/developerworks/cn/linux/l-bochs/index.html)
{% endhint %}

        ****Bochs 模拟器的启动需要配置文件，如 neu-os 目录下已写好的 bochsrc。要在 Bochs 中启动 NEUOS，启动命令行，打开 neu-os 目录，键入

```bash
make
bochs -q
```

或者直接键入

```bash
make run_bochs
```

        run\_bochs 命令定义在 Makefile 中。Bochs 会读取 bochsrc 的配置信息，启动模拟器。

        如图 1-4 中的 Bochs Enhanced Debugger 窗口是 Bochs 的调试器，左侧是寄存器信息，中间是将执行的汇编代码，右侧用于显示地址信息。顶部的 Continue 按钮会使模拟器一直执行代码，直到发生错误、遇到断点或等待用户操作；Step 按钮会向下执行一步，Step N 按钮会向下执行指定步数。调试器的底部是 Bochs 的命令行。点击顶部菜单栏中的 View，可查看 GDT、IDT 等信息。图 1-4 中的 Bochs x86-64 emulator 是模拟器的运行窗口。

### **QEMU**

       **** 类似地，QEMU 是一个面向完整 PC 系统的开源仿真器，它允许实现高级概念上的仿真，如对称多处理系统和其他处理器架构。

![&#x56FE; 1-5 QEMU &#x8FD0;&#x884C;&#x754C;&#x9762;](.gitbook/assets/image%20%283%29.png)

{% hint style="info" %}
了解 QEMU：[使用 QEMU 进行系统仿真](https://www.ibm.com/developerworks/cn/linux/l-qemu/)
{% endhint %}

        与 Bochs 不同，启动 QEMU 模拟器需要以命令指定参数。

        要在QEMU中启动 NEUOS，打开命令行，进入 neu-os 目录，键入

```bash
make
qemu-system-i386 -m 16M -boot a -fda bootimg -serial stdio
```

        Makefile 会生成 NEUOS 中的 bootimg 镜像，然后运行 QEMU。或者也可直接键入：

```bash
make run
```

  ****      run 命令定义在 Makefile 中。

        运行 QEMU 的各参数的意义是：

* i386：指定模拟的处理器
* -m 16M：指定内存大小
* -boot a：从软盘驱动器 A: 启动
* -fda bootimg：指定系统镜像
* -serial studio：将虚拟串口重定向到stdio；按下 Ctrl + c，QEMU 将立即终止。

### 文本编辑器

       **** 实验环境中包含系统预装的文本编辑器与 Vim. 

        Dock 栏中的 Add/Remove Software 是包管理器，可使用它直接安装以下工具。

* [vim](https://www.vim.org/)
* [emacs](https://www.gnu.org/s/emacs/)
* [atom](https://atom.io/)
* [codeblocks](www.codeblocks.org)

        NEUOS 使用 make 构建，但也可使用 IDE 编辑代码。如 [CLion](https://www.jetbrains.com/clion/download/#section=windows) 集成了全局搜索、终端、图形化版本控制、查看代码结构等功能，能提升开发效率。

## 拓展学习

1. 观看视频教程：[\[从零开始的操作系统编写 Lesson 0x00 开发环境\]](https://www.bilibili.com/video/av12169693/)
2. 尝试在 Virtual Box 虚拟机与物理机之间互传文件。

