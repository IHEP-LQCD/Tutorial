# 格点 QCD 软件简介

## 0. Markdown 中英文混排标准

本项目采用 Markdown 作为书写语言，这是一种轻量级标记语言。这种语言容易学习，便于读写，但是它在中英文混排并不会像专业的排版软件（LaTeX 或者 Word）那样在中文和英文之间插入额外的空白，需要手动添加一个空格，例如

- C语言于1969年至1973年间，为了移植与开发UNIX操作系统，由Dennis Ritchie与Ken Thompson，以B语言为基础，在贝尔实验室设计、开发出来。
- C 语言于 1969 年至 1973 年间，为了移植与开发 UNIX 操作系统，由 Dennis Ritchie 与 Ken Thompson，以 B 语言为基础，在贝尔实验室设计、开发出来。

我们提议你在为此项目做出贡献时，可以适当地在中文和英文/数字之间添加空格，更详细的规则见[中文文案排版指北](https://github.com/sparanoid/chinese-copywriting-guidelines/blob/master/README.zh-Hans.md#%E7%A9%BA%E6%A0%BC)“空格”部分。

## 1. 操作系统和软件准备

我们将简单介绍一些操作系统和软件上的准备，以使你的个人电脑能够运行格点 QCD 所需的软件。

下面的内容大致介绍了各个操作系统下你需要准备的工具和软件，包括包管理工具，终端，Git，Python，C/C++ 编译器和构建系统，文本编辑器，以及一些重要软件简单使用方法。

- [**Linux**](./Linux.md)
- [Windows](./Windows.md)
- [macOS](./macOS.md)
- [Git](./Git.md)
- [VSCode](./VSCode.md)
- [C/C++](./C++.md)

## 2. Linux 集群

Linux 集群是我们实际上进行计算时使用的环境，它们搭载了大量的计算资源。一般而言，超级计算机会使用略微不同的环境，但大体上还是可以作为参考。

下面的内容大致介绍了在 Linux 集群中常用的环境管理软件，MPI 和任务调度系统。关于 Linux 的普通操作说明，请参阅上一节相应内容。

- [Module](./Module.md)
- [Automake & CMake](./CMake.md)
- [MPI](./MPI.md)
- [Slurm](./Slurm.md)

## 3. 格点 QCD 软件

对于格点 QCD 而言，大部分的算力都用于组态生成和传播子求解，从其中获得可观测量相比之下显得没那么困难。因此目前大部分的格点 QCD 软件都专注于前述的两个方面，这里介绍的也主要是这一类的软件。其他生成可观测量的软件或者库将会在数据分析软件部分进行介绍。

我们不太建议在你的个人电脑上直接运行本节介绍的程序，因为它们往往需要数量众多的 CPU 核心或者 GPU 进行运算，很难在个人电脑上获得一个有实际意义的计算结果。但考虑到简单的尝试可能可以从本地安装中受益，我们仍然建议你阅读下面的部分，来大致了解格点 QCD 软件编译和运行的步骤，以及一些基本的使用方法。

下面的内容大致介绍了编译系统，QDP，Chroma 和 QUDA，以及以它们为基础编写扩展的方法。

- [QMP & QDPXX & QDP-JIT](./QDP.md)
- [Chroma](./Chroma.md)
- [QUDA](./QUDA.md)

## 4. 数据分析软件

目前我们的数据分析主要使用 Python 完成。这得益于著名的数值计算库 NumPy 和科学计算库 SciPy 在世界范围内的广泛使用。在使用蒸馏方法时，NumPy 以及相应的 GPU 实现也是我们抽取可观测量最重要的工具。

下面的内容大致介绍了目前主要使用的误差分析和拟合工具，NumPy 以及它的一些 GPU 实现。

- [NumPy & SciPy](./NumPy.md)
- [NumBa & CuPy & PyTorch](./CuPy.md)
- [gvar & lsqfit](./lsqfit.md)
