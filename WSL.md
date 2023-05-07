# WSL

Windows Subsystem for Linux (WSL) 是微软在 Windows 10 引入的子系统，可以在 Windows 上近乎无缝地获得 Linux 体验。配合 VSCode 的 WSL 扩展，对于格点 QCD 来说的开发体验实际已经超过了 macOS。启动终端，在命令行中输入

```PowerShell
wsl --install
```

根据提示操作就可以启用 WSL 需要的系统功能，并安装默认的 Ubuntu 发行版。可以通过 `wsl --list --online` 命令列出所有可用的发行版，并通过 `wsl --install <Distro>` 替换为相应的 `NAME` 项来安装需要的发行版。这一步可能需要重启 Windows。

如果一切顺利，你将会在终端的新建下拉菜单中看到相应的发行版。按照提示完成一些初始化例如账户密码设置等步骤后，就可以通过这种方式运行 WSL。另一方面，你可以直接在命令行中输入 `wsl` 来进入已安装的发行版。

你可以在资源管理器的路径区域输入 `\\wsl.localhost`，选择安装的发行版，就可以直接通过 Windows 资源管理器浏览 WSL 中的文件。反过来，在 WSL 中，`/mnt/c` 则对应于 Windows 系统的 `C:` 盘。
