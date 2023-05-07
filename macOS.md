# macOS

macOS 是一种 UNIX 操作系统，其环境与 Linux 略有不同，但大体上相近。相比 Linux 它有原生且优美的图形界面，因此经常有人说 macOS 是最适合程序员的操作系统。根据 [Homebrew 官网介绍](https://docs.brew.sh/Installation#2)，我们建议将 macOS 升级到 11 (Big Sur) 及以上。

## Xcode CLI

与 Windows 相同，macOS 并没有直接随附我们需要的各种命令行工具。启动 `终端.app`，输入下列命令并回车，

```zsh
xcode-select --install
```

根据提示操作就可以安装一些 Apple 提供的命令行工具，包括 `git` 和 `python3`等等。在终端中输入 `git`，你应该看到帮助信息。在终端中输入 `python3`，你应该进入 Python 命令行交互界面。

```zsh
git
usage: git [-v | --version] [-h | --help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           [--super-prefix=<path>] [--config-env=<name>=<envvar>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone     Clone a repository into a new directory
   init      Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add       Add file contents to the index
   mv        Move or rename a file, a directory, or a symlink
   restore   Restore working tree files
   rm        Remove files from the working tree and from the index

examine the history and state (see also: git help revisions)
   bisect    Use binary search to find the commit that introduced a bug
   diff      Show changes between commits, commit and working tree, etc
   grep      Print lines matching a pattern
   log       Show commit logs
   show      Show various types of objects
   status    Show the working tree status

grow, mark and tweak your common history
   branch    List, create, or delete branches
   commit    Record changes to the repository
   merge     Join two or more development histories together
   rebase    Reapply commits on top of another base tip
   reset     Reset current HEAD to the specified state
   switch    Switch branches
   tag       Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch     Download objects and refs from another repository
   pull      Fetch from and integrate with another repository or a local branch
   push      Update remote refs along with associated objects

'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
See 'git help git' for an overview of the system.
```

```zsh
python3
Python 3.9.6 (default, Mar 10 2023, 20:16:38)
[Clang 14.0.3 (clang-1403.0.22.14.1)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

## Homebrew

Homebrew 是 macOS 上最受欢迎的包管理工具。它通过预编译二进制包的形式提供了 macOS 开发需要的各种命令行软件和库。在终端中输入下列命令并回车，

```zsh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

现在，你可以使用 `brew search <package>` 或 `brew install <package>` 搜索和安装你需要的软件和库了。例如 `brew search cmake`，

```zsh
brew search cmake
==> Formulae
cmake ✔                    make                       omake
cmake-docs                 cmark                      imake
extra-cmake-modules        bmake                      xmake
cake                       smake

==> Casks
cmake
```

我们想安装的是 `Formulae` 中的第一个结果，将这个名称填入 `brew install` 之后，即

```zsh
brew install cmake
```

就可以安装 macOS 命令行版本的 CMake 软件。若你想安装基于 Qt 构建的 GUI 版本的 CMake（通常不建议这么干），这对应于 `Casks` 中的第一个结果，那么需要额外添加参数 `--cask`，即

```zsh
brew install --cask cmake
```

## Python

若你想要安装其他版本的 Python，例如 3.11，那么使用 Homebrew 搜索 Python，

```zsh
brew search python3
==> Formulae
boost-python3   python@3.11 ✔   python@3.8      ipython         cython
python@3.10 ✔   python@3.7      python@3.9      bpython         jython

If you meant "python3" specifically:
It was migrated from homebrew/cask to homebrew/core.
```

选择你需要的版本，输入以下命令并回撤以安装对应版本的 Python 解释器。

```zsh
brew install python@3.11
```

安装完成后，在终端输入 `python3`，你应该看到命令行交互界面显示的版本出现了改变。

```zsh
python3
Python 3.11.3 (main, Apr  7 2023, 20:13:31) [Clang 14.0.0 (clang-1400.0.29.202)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

## Visual Studio Code

Visual Studio Code (VSCode) 是 Microsoft 开发的一个开源的编辑器，可以通过繁多的插件支持多种编程语言。

使用 Homebrew 搜索 Visual Studio Code，

```zsh
brew search visualstudiocode
==> Casks
visual-studio-code ✔                     visual-studio-code-insiders
```

通常不选择内测版本，因此输入以下命令并回车，

```zsh
brew install --cask visual-studio-code
```

关于 VSCode 更多的使用方法，参见 [VSCode](./VSCode.md)。
