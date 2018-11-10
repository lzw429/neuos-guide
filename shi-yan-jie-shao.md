# 实验介绍

{% hint style="info" %}
本文档通过 GitBook 书写，建议访问 [https://neuos.gitbook.io/guide](https://neuos.gitbook.io/guide) 阅读。
{% endhint %}

## 实验目录

1. 初识实验环境
2. 内核引导启动
3. VGA 与串行端口
4. 内存管理与分页
5. 缺页异常
6. 中断与系统调用
7. 进程控制
8. 文件系统

## NEUOS 简介

        本实验的材料由 NEUOS 与 Linux 0.11 组成。NEUOS 是一个基于 Linux 0.11 内核的教学用操作系统。通过早期 Linux 内核进行学习，排除了目前内核中复杂而庞大的实现细节，而能较为完整地了解内核实现原理。

        因为 Linux 下的 C 语言开发通常使用 AT&T 内联汇编，NEUOS 中的汇编代码为 AT&T 格式。本实验需要一些简单的汇编语言知识。

### 源码

{% hint style="info" %}
运行 NEUOS 需要在 Linux 环境下。我们提供了一个虚拟机镜像 “NEUOS Lab Environment” 用作实验环境，详见“实验1-实验提示-NEUOS 实验环境”一节。实验环境的 Documents 目录中内置了下列三份源码。
{% endhint %}

* NEUOS ：[https://github.com/VOID001/neu-os](https://github.com/VOID001/neu-os)
* Linux 0.11 ：[https://github.com/lzw429/Linux-0.11](https://github.com/lzw429/Linux-0.11)
* 实验材料：[https://github.com/lzw429/neuos-material](https://github.com/lzw429/neuos-material)

### 运行（README）

        NEUOS 源码使用 make 构建。用命令行打开 neu-os 目录，键入

```bash
make
```

make 将生成软盘镜像作为启动盘，这是启动 NEUOS 所必需的。

       生成镜像并使用有调试功能的 Bochs 运行 NEUOS，在命令行键入：

```bash
make run_bochs
```

或者

```bash
make
bochs -q
```

        生成镜像并使用 QEMU 运行 NEUOS，在命令行键入

```bash
make run
```

        每次修改代码后，命令行键入

```bash
make clean
make
```

可重新构建。

        QEMU 与 Bochs 的运行参数请查看 Makefile.

## 预备

必备知识：

* 计算机组成原理、数据结构与算法
* 一定的 C 语言编程能力

如果了解以下知识，本实验会相对轻松：

* AT&T 汇编语言程序设计
* GNU 工具链

## 学习建议

      ****  所有实验文档中的“实验资料”，并不是只为了有限的实验内容而编写的。实验资料对读者理解 Linux 的早期内核大有裨益。在开始每一项实验前，我们建议先浏览写在“实验内容”后的“实验资料”。

        在内核引导启动部分，关注操作系统如何与 BIOS 协调将计算机启动；在进程调度与文件系统部分，着重关注数据结构与算法的设计。

## 参考资料

* 《Linux 内核完全注释》赵炯（对于理解 Linux 0.11 十分必要）
* 《Linux 内核设计与实现》Robert Love
* 《深入理解 Linux 内核》Daniel Bovet, Marco Cesati
* [**英特尔® 64 位和 IA-32 架构开发人员手册**](https://www.intel.cn/content/www/cn/zh/architecture-and-technology/64-ia-32-architectures-software-developer-manual-325462.html)

## 实验反馈

        请将您在本实验中遇到的疑惑或任何意见与建议反馈给我们。

        [https://github.com/lzw429/neuos-material/issues](https://github.com/lzw429/neuos-material/issues)

