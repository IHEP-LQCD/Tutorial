# macOS

macOS 是一种 UNIX 操作系统，其环境与 Linux 略有不同，但大体上相近。相比 Linux 它有原生且优美的图形界面，因此经常有人说 macOS 是最适合程序员的操作系统。根据 [Homebrew 官网介绍](https://docs.brew.sh/Installation#2)，我们建议将 macOS 升级到 11 (Big Sur) 及以上。

## Xcode CLI

与 Windows 相同，macOS 并没有直接随附我们需要的各种命令行工具。启动 `终端.app`，输入下列命令并回车，

```zsh
xcode-select --install
```

根据提示操作就可以安装一些 Apple 提供的命令行工具，包括 `git` 和 `python3`等等。

## Homebrew

Homebrew 是 macOS 上最受欢迎的包管理工具。它通过预编译二进制包的形式提供了 macOS 开发需要的各种命令行软件和库。在终端中输入下列命令并回车，

```zsh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

现在，你可以使用 `brew search <package>` 或 `brew install <package>` 搜索和安装你需要的软件和库了。
