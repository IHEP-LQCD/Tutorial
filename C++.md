# C & C++

## C/C++ 编译器

目前 C/C++ 的主流编译器有三个：

- GCC：使用最为广泛的 GNU Compiler Collection（GCC）是 GNU 基金会的项目，历史悠久，是曾经的编译器事实标准。

- Clang/LLVM：后起之秀 Clang/LLVM 工具链则来自 LLVM 项目，最早主要由苹果公司推动，Clang 作为前端有更友好的错误信息提示。LLVM 项目有着优秀的软件工程实践，LLVM 本身更是成为了众多新型编程语言首选的中间表示，并为众多解释型语言的即时编译（JIT）功能提供支持。

- MSVC：与这两个跨平台的开源编译器不同，微软公司的 MSVC 只能在 Windows 下运行，并只能得到运行在 Windows 下的软件。MSVC 的 C 语言与 GCC/Clang 支持的有些许不同，不建议使用 MSVC 开发 C 语言项目。另一方面，MSVC 对 C++ 的支持，特别是新标准的跟进上是三者中进度最快的。

本节的内容将基于 GCC 进行，理论上 Clang/LLVM 也不会有兼容性问题。通过命令

```bash
gcc -v
```

你应当可以得到你所使用的 GCC 版本信息。

## C

C 语言于 1969 年至 1973 年间，为了移植与开发 UNIX 操作系统，由 Dennis Ritchie 与 Ken Thompson，以 B 语言为基础，在贝尔实验室设计、开发出来。C 语言是一种静态类型的编译型语言，基本上是目前最接近硬件层面的高级语言。

对于 C 语言编写的可执行文件来说，则必须要有一个主入口，也就是著名的 `int main(int argc, char* argv[])` 函数，它需要有一个 `int` 类型的返回值。默认参数 `argc` 和 `argv` 是在调用可执行文件时传入的所有参数列表。通常而言，这个返回值为 `0` 意味着程序正常结束并退出。在文件 `a.c` 中输入如下内容：

```C
/* a.c */
int main(int argc, char* argv[]) {
    return 0;
}
```

通过命令 `gcc -o a a.c` 就可以将源文件 `a.c` 编译成可执行文件 `a`，再使用命令 `./a` 即可运行。显然上面的程序什么都没有做，因此执行程序不会有任何输出。现在我们通过 `printf` 在命令行环境中输出一些东西。

```C
/* a.c */
#include <stdio.h>

int main(int argc, char* argv[]) {
    printf("Hello World!");
    return 0;
}
```
