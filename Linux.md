# Linux

大部分的格点 QCD 软件都是基于 Linux 编写的。在使用集群甚至超级计算机时，它们也都搭载 Linux 操作系统，因此基本的 Linux 命令行操作是格点 QCD 研究人员必需的技能。

Linux 拥有众多的发行版，每个发行版的操作会有些许不同。总的来看，国内现有的计算资源大都使用 CentOS 发行版，而对于桌面系统则以 Ubuntu 为主流，因此我们主要基于这两个发行版进行说明。更进一步，可以将流行的发行版大致分为 Red Hat (CentOS, Fedora)，Debian (Ubuntu, Deepin) 和 SUSE 等几个派系，一般总能找到对应的操作方式。至于 Arch (Manjaro) 和其他更小众发行版相应的操作方式，请善用搜索引擎，通常从 ArchWiki 收录文档开始会是个不错的选择。


## RHEL/CentOS

RHEL 及其派生发行版通常使用 yum 作为包管理器，SUSE 及其派生发行版也是如此。另外 Fedora 使用 yum 作为包管理器，但是他们处理的都是 rpm 格式打包。

NVIDIA 最新的 CUDA SDK 提供了 RHEL 7 及之后的版本，CentOS 7，Fedora 35 支持；AMD 最新的 ROCm SDK 提供了 RHEL 8.6 及之后的 LTS 版本支持。

这里需要注意，CentOS 7 即将进入 EOL，即使它与大部分集群的环境相同，我也不推荐你在本地安装它，因为使用中可能会遇到许多二进制兼容性的问题，并导致不得不手动编译软件的情况。

## Debian/Ubuntu

Debian 及其派生发行版一个重要特点是使用 apt 作为包管理器，处理 deb 格式的打包。

NVIDIA 最新的 CUDA SDK 提供了 Debian 10 及之后的版本，以及 Ubuntu 2004 及之后的 LTS 版本支持；AMD 最新的 ROCm SDK 提供了 Ubuntu 20.04 及之后的 LTS 版本支持。

另外也可以通过官方的 apt 源获得较新的 CUDA 工具集。事实上，我比较推荐通过发行版的源获取 CUDA 工具集，可以更好地处理依赖。

```bash
sudo apt update
sudo apt upgrade
sudo apt install nvidia-cuda-toolkit nvidia-cuda-toolkit-gcc nvidia-cuda-samples
```

在编译 CUDA 相关的项目时，使用 `cuda-gcc` 和 `cuda-g++` 作为宿主编译器，可以解决 `nvcc` 与 `gcc` 版本不匹配的问题。
