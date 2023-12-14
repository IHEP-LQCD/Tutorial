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

## VSCode 更新后出现报错: Error: XHR failed 的处理办法

在 2023.11.8 VSCode 更新至版本 1.85 之后，在 `csns` 这样的无网络的集群上可能出现 VSCode remote server 更新时报错: Error: XHR failed，解决办法为手动安装，解决方法如下。

- 1. 先确定更新的 VSCode 版本的 commit。
    
    比如在 VSCode 终端的 output 一栏中：报错位置在 `.vscode-server/.cli.0ee08df0cf4527e40edc9aa28f4b5bd38bbff2b2.log`，那么 需要手动安装的 VSCode 版本 commit 号就是 `0ee08df0cf4527e40edc9aa28f4b5bd38bbff2b2`。请注意复制完整的 commit 号。

- 2. 在本地下载 VSCode 安装包并上传到远程。

    在本地下载对应 commit 的安装包，命令如下：
    ```
    wget -O ./vscode-server-linux-x64.tar.gz https://update.code.visualstudio.com/commit:0ee08df0cf4527e40edc9aa28f4b5bd38bbff2b2/server-linux-x64/stable
    ```
    > **提示：** ：请把 commit 号换成你需要的。

    上传至远程集群：
    ```
    scp ./vscode-server-linux-x64.tar.gz csns:~/
    ```

- 3. 解压安装包。

    先关闭你在该集群上所有的远程 VSCode 窗口（必要时 `kill -9 -1`）。

    `ssh` 连接上远程，然后解压上传的安装包到对应 commit 的安装位置下，使用命令：
    ```
    cd ~
    rm -rf ~/.vscode-server/bin/0ee08df0cf4527e40edc9aa28f4b5bd38bbff2b2
    tar -xzvf ./vscode-server-linux-x64.tar.gz -C ~/.vscode-server/bin/0ee08df0cf4527e40edc9aa28f4b5bd38bbff2b2
    ```

    > **提示：** ：请把 commit 号换成你需要的。

    解压完成后重新使用 VSCode 登录后可以正常使用。