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
cmake .. -DCMAKE_INSTALL_PREFIX=~/.local -DCMAKE_BUILD_TYPE=Release -DQMP_MPI=ON
cmake --build . -j8
cmake --install .
```

通常我们并不直接使用 QMP，而是将其作为多进程版本的 QDPXX 和 QUDA 的依赖。你也可以在 `build` 文件夹中使用 `ccmake .` 命令通过命令行界面直观地看到并修改项目支持的所有配置项。

## QDPXX

[QDPXX](https://github.com/usqcd-software/qdpxx) 是一个由 USQCD 合作组开发的，用于各种格点 QCD 中关于场的线性代数库。通过算符重载，QDPXX 针对规范场，旋量场等实现了方便直观的数据结构。

通过 CMake 构建并安装 QDPXX。

```zsh
mkdir build
cd build
cmake .. -DCMAKE_INSTALL_PREFIX=~/.local -DCMAKE_BUILD_TYPE=Release -DQDP_USE_OPENMP=ON
make --build . -j8
cmake --install .
```

上述命令将自动搜索之前安装的 QMP 并利用它编译默认的多进程版本的 QDPXX。你也可以在 `build` 文件夹中使用 `ccmake .` 命令通过命令行界面直观地看到并修改项目支持的所有配置项。

当我们为个人电脑安装 QDPXX 时，通常并不需要使用多进程。这时可以通过修改选项去掉 QMP 依赖。

```zsh
mkdir build
cd build
cmake .. -DCMAKE_INSTALL_PREFIX=~/.local -DCMAKE_BUILD_TYPE=Release -DQDP_USE_OPENMP=ON -DQDP_PARALLEL_ARCH=scalar
make --build . -j8
cmake --install .
```

在这种情况下，QDPXX 将不会搜索 QMP。

## C++ 调用 QDPXX

我们现在可以通过 C++ 来调用 QDPXX 中定义的各种类和基础线性代数操作。我们将使用 CMake 配置项目。直接通过 C++ 调用 QDPXX 能完成的任务有限，通常用于处理我们编写 Chroma 扩展的时候需要的一些简单操作。

首先，需要通过 `CMakeLists.txt` 文件找到之前编译完成的 QDPXX，通常将 `CMAKE_INSTALL_PREFIX` 设置为之前使用的值即可。

```CMake
cmake_minimum_required(VERSION 3.0)
set(CMAKE_INSTALL_PREFIX "~/.local")
project(test_qdpxx LANGUAGES C CXX)

find_package(QDPXX REQUIRED)
add_executable(main main.cc)
target_link_libraries(main QPDXX::qdp)
```

这样，你就可以在 `main.cc` 文件中通过 `#include <qdp.h>` 来使用 QDPXX 中的各种功能。例如我们可以在其中定义随机的格点规范场 $U_\mu(x)$ 和费米子场 $F(x)$，并计算 $\sum_\mu U_\mu(x)F(x+\hat\mu)$。

```C++
#include <qdp.h>

using namespace QDP;

int main(int argc, char *argv[]) {
    // 初始化 QDPXX
    QDP_initialize(&argc, &argv);

    // 设置格点规模，并创建 Layout
    const int latdims[Nd] = {4, 4, 4, 8};
    multi1d<int> nrow(Nd);
    nrow = latdims;
    Layout::setLattSize(nrow);
    Layout::create();

    // 创建规范场 U 并随机初始化
    multi1d<LatticeColorMatrix> U(Nd);
    for (int dir = 0; dir < Nd; ++dir) {
        random(U[dir]);
    }

    // 创建费米子场 F 并随机初始化
    LatticeFermion F;
    random(F);

    // 创建费米子场 UF 并置 0
    LatticeFermion UF = zero;

    // UF = \sum_\mu U_\mu(x) F(x+\hat\mu)
    for (int dir = 0; dir < Nd; ++dir) {
        UF += U[dir] * shift(F, FORWARD, dir);
    }

    // 结束 QDPXX
    QDP_finalize();

    return 0;
}
```

这样，再额外添加一些与 Gamma 矩阵相关的操作就可以简单地实现 Dslash。

## Chroma

[Chroma](https://github.com/JeffersonLab/chroma) 是一个由 USQCD 合作组开发的，基于 QDPXX API 的格点 QCD 算法工具集，目前托管在 Jefferson 国家实验室。这是目前使用较为广泛的格点 QCD 软件，它通过 XML 文件描述计算流程，方便了不熟悉编程的初学者进行格点 QCD 计算。另外我们也可以直接使用 C++ 调用 Chroma 中定义的各种类和函数，定制我们需要的功能。

```zsh
mkdir build
cd build
cmake .. -DCMAKE_INSTALL_PREFIX=~/.local -DCMAKE_BUILD_TYPE=Release -DChroma_ENABLE_OPENMP=ON
make --build . -j8
cmake --install .
```

这样，我们就完成了 Chroma 的安装。能否通过多进程并行取决于 QMP 的编译选项。
