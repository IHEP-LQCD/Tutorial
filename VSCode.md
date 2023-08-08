# Visual Studio Code

## 简介

Visual Studio Code 是微软主导开发的一款开源文本编辑器。其最大的特点是通过丰富且便利的插件市场来支持各种编程语言的开发工作。对于格点 QCD 而言，我们主要使用 C/C++ 插件和 Python 插件来对相应的工作流做支持。

## VSCode 在远程崩溃 / 无限重连 处理办法

在使用 ihep / csns 这些集群时，偶尔会出现 远程server崩溃 / 无限重连的问题。

可能原因及处理：

- `~/.vscode-server` 没有写入权限。比如 ihep 集群的用户文件夹就需要手动二次验证开启。或者写入文件达到上限。这种情况建议把 `.vscode-server` 翻到：

    ```bash
        mv ~/.vscode-server {workdir}/.vscode-server
        ln -s {workdir}/.vscode-server ~/.vscode-server
    ```

- 系统中已经存在另外一个 VSCode 进程。处理：可以停止用户进程 `kill -9 -1`，后重新登录。

> **提示：** 如果以上解决不了，你急了的话，可以 `rm -rf ~/.vscode-server`, 后重新登录安装 vscode-server.
