# QMP & QDPXX & QDP-JIT

## QMP

[QMP](https://github.com/usqcd-software/qmp) 是一个由 USQCD 合作组开发的，用于各种格点软件（QDPXX 和 QUDA）的消息传递库。基本上它包装了 MPI，并根据格点 QCD 计算中的一些特定需求添加了便于使用的函数，并隐藏了使用/不使用 MPI 时的调用区别。

首先从 GitHub 下载 QMP 的源代码，

```zsh
git pull https://github.com/usqcd-software/qmp
```

通过 CMake 构建并安装 QMP。你需要自定义软件安装的位置，例如你想安装在 `~/.local`（需要预先创建这个文件夹），

```zsh
mkdir build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=~/.local
cmake --build . -j8
cmake --install .
```

当你需要构建支持 MPI 的 QMP 时，需要启用 `QMP_MPI` 选项，

```zsh
mkdir build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release -DQMP_MPI=ON -DCMAKE_INSTALL_PREFIX=~/.local
cmake --build . -j8
cmake --install .
```

通常我们并不直接使用 QMP，而是将其作为多进程版本的 QDPXX 和 QUDA 的依赖。
