# Linux 笔记

## 常见 Linux 版本
### 红帽系
- <a href=https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux>Red Hat Enterprise Linux</a>
- <a href=https://www.centos.org/download>CentOS Stream</a>
- <a href=https://www.suse.com>SUSE Linux Enterprise Server</a>
- <a href=https://fedoraproject.org/workstation>Fedora Workstation</a>
- <a href=https://www.gentoo.org/downloads>Gentoo Linux</a>
- <a href="https://www.chinaredflag.cn/product/newproduct?tolink=redflag-web/DT11">红旗 Linux 桌面操作系统</a>
- <a href=https://www.openmandriva.org>OpenMandriva Lx</a>
### 其他常见版本
- <a href=https://ubuntu.com/download>Ubuntu</a>
- <a href=https://www.debian.org/distrib>Debian</a>
- <a href=https://www.knopper.net/knoppix-mirrors>KNOPPIX</a>

## 设备文件名
### 常见设备
- `/dev/hda` ：IDE 硬盘
- `/dev/hdb` ：IDE 硬盘
- `/dev/hdc` ：IDE 硬盘
- `/dev/hdd` ：IDE 硬盘
- `/dev/sda` ：SCSI / SATA / USB 硬盘
- `/dev/sdp` ：SCSI / SATA / USB 硬盘
- `/dev/cdrom` ：光驱
- `/dev/sr0` ：光驱
- `/dev/fd0` ：软盘
- `/dev/fd1` ：软盘
- `/dev/lp0` ：25 针打印机
- `/dev/lp1` ：25 针打印机
- `/dev/lp2` ：25 针打印机
- `/dev/usb/lp0` ：USB 打印机
- `/dev/usb/lp15` ：USB 打印机
- `/dev/mouse` ：鼠标

把 `/dev/sda` 中的最后一个字符换成 `a` ~ `p` 之间的任何一个小写字母，仍然是 SCSI / SATA / USB 硬盘的设备文件名。把 `/dev/usb/lp0` 中的最后一个字符换成 `0` ~ `15` 之间的任何一个整数，仍然是 USB 打印机的设备文件名。

### 硬盘分区
- `/dev/hda1` ：IDE 硬盘的主分区接口
- `/dev/hda2` ：IDE 硬盘的主分区 / 扩展分区接口
- `/dev/hda3` ：IDE 硬盘的主分区 / 扩展分区接口
- `/dev/hda4` ：IDE 硬盘的主分区 / 扩展分区接口
- `/dev/hda5` ：IDE 硬盘的逻辑分区接口
- `/dev/hda6` ：IDE 硬盘的逻辑分区接口
- `/dev/hda7` ：IDE 硬盘的逻辑分区接口

把 `/dev/hda` 换成任何一块硬盘的设备文件名，即表示该硬盘的分区接口。

#### 分区编号规则
- 同一块硬盘上的主分区 / 扩展分区合计最多 4 个，二者共用 `1` / `2` / `3` / `4`，且其中至多只有 1 个扩展分区。
- 主分区用于写入数据，扩展分区只用于包含**逻辑分区**。逻辑分区的编号从 `5` 开始，行使和主分区相同的功能。

例如：某一块硬盘上有 2 个主分区、1 个扩展分区、3 个逻辑分区，那么这块硬盘的分区编号 `1` / `2` 是主分区，`3` 是扩展分区，`5` / `6` / `7` 是逻辑分区，`4` 不存在。

## 命令行格式
```
cmd opt params
```
- `cmd` ：命令名称。
- `opt` ：选项。格式为 `-` 后跟选项简称，`--` 后跟选项全称。
- `params` ：参数，命令的作用对象。作为 `params` 的文件 / 目录是指它的**路径**，作为 `params` 的设备 / 硬盘分区是指它们的**设备文件名**。

`opt` 可以为空，也可以有多个。多个选项同时启用，`-` 后跟上各个选项简称即可，也可以分开写。例如：同时启用 `a` / `b` / `c` / `d` 四个选项，可以写成 `-abcd`，也可以写 `-a -b -c -d`。也可以写 `-acdb`，因为大部分选项次序与实际效果无关。`opt` 可以为 `--help`，此时显示 `cmd` 命令的常用选项列表。

`params` 一般不为空，除非 `cmd` 命令有默认参数或者要求 `params` 为空，否则不会执行命令，而是显示 `cmd` 命令的常用选项列表。若 `params` 可以有多个，则互相用**空格**隔开。

## 路径格式
- `/` ：根目录。
- `.` ：当前目录
- `..` ：上级目录
- `~` ：宿主目录（家目录）

`/` 前面没有任何字符才表示根目录，否则 `/` 为目录与其下的文件 / 下级目录之间的分隔符。用户名 `xxx` 的宿主目录为 `/home/xxx`，管理员的宿主目录为 `/root`。

**绝对**路径必须从根目录开始，**相对**路径只能从当前目录开始。相对路径可理解为以 `./` 为起点的绝对路径，只不过 `./` 可以省略。

## 挂载卸载命令
```
mount param1 param2
mount -t param0 param1 param2
umount param
```
- `mount` ：把 `param1` 设备挂载到 `param2` 目录。`param2` 称为 `param1` 设备的挂载点。
  - `-t` ：指定 `param0` 文件系统。若**不**启用，则自动识别文件系统。
- `umount` ：卸载 `param` 设备。

## 文件处理命令
```
ls param/
ls param
ls
ls -a param/
ls -a
ls -l param/
ls -l param
ls -l
ls -ld param/
ls -ld param
ls -ld
ls -lh param/
ls -lh param
ls -lh
ls -i param/
ls -i param
ls -i
ll param/
ll param
ll
ll -d param/
ll -d param
ll -d
ll -h param/
ll -h param
ll -h
mkdir param/
mkdir -p param/
cd param/
cd
pwd
rmdir param/
cp param1 param2/
cp param1 param2/new
cp -r param1/ param2/
cp -r param1/ param2/new
cp -p param1 param2/
cp -p param1 param2/new
mv param1 param2/
mv param1 param2/new
mv param1 new
rm param
rm -r param/
rm -f param
rm -rf param/
touch param
cat param
cat -n param
tac param
more param
less param
head param
head -n num param
tail param
tail -n num param
tail -f param
tail -fn num param
ln param1 param2
ln -s param1 param2
ln -s param1/ param2
```
- `ls` ：显示 `param` 目录下的所有文件。若 `param` 为空，则默认为当前目录。_Linux 的**目录**就是 Windows 的**文件夹**，但同时也是文件。_
  - `-a` ：同时显示**隐藏文件**。若不启用则不显示隐藏文件。_Linux 的**隐藏文件**是指文件名以 `.` 开头。_
  - `-l` ：显示**详细信息**（见下）；若不启用则只显示文件名。启用时，`param` 可以不是目录，此时显示 `param` 的详细信息。
    - `-d` ：若同时启用 `-ld` 且 `param` 是目录，则显示 `param` 的详细信息。
    - `-h` ：若同时启用 `-lh`，则详细信息中的**文件大小**按最大单位显示。若不启用，则默认单位是**字节**。
  - `-i` ：显示 **_i_ 节点**编码。
- `ll` ：执行 `ls` 并启用 `-l`。
- `mkdir` ：新建 `param` 目录。`param` 可以有多个，此时分别新建多个目录。不能新建相同路径下已存在的同名目录。
  - `-p` ：递归创建。若不启用，则只能创建已有目录的下级目录；若启用，如果相同路径下已存在同名目录，则会忽略该目录而非弹出警告。_若 `A` 目录是 `B` 目录的上级目录，那么 `B` 就是 `A` 的下级目录。_
- `cd` ：切换当前目录。`param` 为切换后的当前目录。若 `param` 为空，则默认为宿主目录。
- `pwd` ：显示当前目录的路径。`params` 需为**空**。
- `rmdir` ：删除 `param` 目录（需为**空**目录）。`param` 可以有多个且可以使用 `*` 作为通配符，此时分别删除多个空目录。
- `cp` ：复制 `param1` 文件到 `param2` 目录下，覆盖同名旧文件（如果存在）。`param1` 可以有多个且可以使用 `*` 作为通配符，此时分别复制多个文件到 `param2` 目录下。若 `param1` 只有 1 个，则 `param2` 可以为 `param2/new`，此时复制 `param1` 文件到 `param2` 目录下并将副本重命名为 `new`。
  - `-r` ：复制目录。若不启用，则 `param1` 不能为目录。
  - `-p` ：同时复制详细信息。若不启用，则仅复制内容。
- `mv` ：移动 `param1` 文件 / 目录到 `param2` 目录下，覆盖同名旧文件（如果存在）。`param1` 可以有多个且可以使用 `*` 作为通配符，此时分别移动多个文件 / 目录到 `param2` 目录下。若 `param1` 只有 1 个，则 `param2` 可以为 `param2/new`，此时移动 `param1` 文件到 `param2` 目录下并重命名为 `new`。若 `param1` 本来就在 `param2` 目录下，则仅重命名。
- `rm` ：删除 `param` 文件。`param` 可以有多个且可以使用 `*` 作为通配符，此时分别删除多个文件。
  - `-r` ：删除目录。若不启用，则 `param1` 不能为目录。
  - `-f` ：强制执行。若不启用，则可能会弹出确认信息。
- `touch` ：新建 `param` 文件。`param` 可以有多个，此时分别创建多个文件。不能新建相同路径下已存在的同名文件。_新建文件的内容为空，可以理解为空白的文本文件。_
- `cat` ：显示 `param` 文件的内容。
  - `-n` ：显示行号。
- `tac` ：显示 `param` 文件的内容，按行倒序。
- `more` ：分页显示 `param` 文件内容。按 `Space` 翻页，按 `Enter` 换行，按 `Q` 退出。
- `less` ：分页显示 `param` 文件的内容。按 `PageUp` 上一页，按 `PageDown` 下一页，按 `↑` 上一行，按 `↓` 下一行。按 `/` 后可以输入关键词，然后按 `Enter` 搜索关键词，按 `N` 定位到关键词下一个所在位置。
- `head` ：显示 `param` 文件的前 10 行。
  - `-n` ：若启用，则改为显示 `param` 文件的前 `num` 行。
- `tail` ：显示 `param` 文件的后 10 行。
  - `-n` ：若启用，则改为显示 `param` 文件的后 `num` 行。
  - `-f` ：打开监视模式。_此模式下可以实时查看是否有其他用户正在修改文件内容。_
- `ln` ：生成链接文件。默认创建 `param1` 文件的**链接** `param2`。_Linux 的**链接**是源文件的复制，但与源文件共享 **i 节点**，且与源文件必须处于同一硬盘分区才能创建。_
  - `-s` ：若启用，则改为创建 `param1` 文件 / 目录的**符号链接** `param2`；若不启用，则 `param1` 不能为目录。_Linux 的**符号链接**（软连接）就是 Windows 的**快捷方式**，无需与源文件处于同一硬盘分区。_

### 详细信息
```
total：总用量
typemode. count owner group size month date time filename
```
- `typemode` ：**文件类型**与**访问权限**（见下）。
- `count` ：引用计数。
- `owner` ：**所有者**。
- `group` ：**所属组**。
- `size` ：文件大小。默认单位是**字节**。
- `month/date/time` ：最后修改时间（月 / 日 / 时分）。
- `filename` ：文件名。

### 文件类型
文件类型显示在 `typemode` 的第 1 位。
- `-` ：普通文件（文本文件或二进制文件）
- `d` ：目录
- `l` ：符号链接

### 访问权限
访问权限显示在 `typemode` 的后 9 位。其中前 3 位是**所有者**的权限，中间 3 位是**所属组**的权限，后 3 位是**其他人**（不是**所有者**也不属于**所属组**的用户）的权限。
- `r` ：读取权限。文件的读取权限可以查看文件内容；目录的读取权限可以查看该目录下的文件名及其详细信息。
- `w` ：写入权限。文件的写入权限可以修改文件内容；目录的写入权限可以创建 / 删除 / 移动该目录下的文件。
- `x` ：执行权限。文件的执行权限可以执行文件；目录的执行权限可以进入该目录（包括切换当前目录到该目录、修改该目录下的文件权限等）。
- `-` ：占位符。表示没有这项权限。

## 权限管理命令
```
chmod num param
chmod num param/
chmod -R num param/
chown name param
chown name param/
chgrp name param
chgrp name param/
umask num
umask
umask -S
```
- `chmod` ：修改 `param` 文件 / 目录的访问权限为 `num`，其中 `num` 可以是 **ugo 格式** 或者 **bin 格式**（见下）。`param` 可以有多个且可以使用 `*` 作为通配符，此时修改每个文件的访问权限。
  - `-R` ：递归修改。若启用且 `param` 是目录，则同时修改该目录下所有文件的访问权限，但以此法不会取消该目录下文件的执行权限（`x`）。
- `chown` ：修改 `param` 文件 / 目录的**所有者**为 `name` 用户名。`param` 可以有多个且可以使用 `*` 作为通配符，此时修改每个文件的**所有者**。
- `chgrp` ：修改 `param` 文件 / 目录的**所属组**为 `name` 所属组名称。`param` 可以有多个且可以使用 `*` 作为通配符，此时修改每个文件的**所属组**。_每个用户都有一个默认的所属组，这个所属组名称与该用户名相同。_
- `umask` ：修改以后新建目录的默认访问权限，`num` 为 **xor 格式**（见下）。若 `num` 为空，则改为以四位数的形式显示现在新建目录的默认访问权限，其中后 3 位为 xor 格式；若 `num` 不为空，则同时修改以后新建文件的默认访问权限，但以此法不会给新建文件设置默认的执行权限（`x`）。
  - `-S` ：以 ugo 格式显示现在新建目录的默认访问权限，启用时 `num` 需为**空**。

### ugo 格式
- `u` ：**所有者**
- `g` ：**所属组**
- `o` ：**其他人**
- `+` ：添加
- `-` ：取消
- `=` ：访问权限设置为
- `r` ：读取权限
- `w` ：写入权限
- `x` ：执行权限
 
例如： `u+x` 表示给**所有者**添加执行权限， `g-wx` 表示取消**所属组**的写入 / 执行权限， `o=rw` 表示把其他人的访问权限设置为只允许读取 / 写入权限。

### bin 格式
- `1` ：执行权限
- `2` ：写入权限
- `3` ：写入 / 执行权限
- `4` ：读取权限
- `5` ：读取 / 执行权限
- `6` ：读取 / 写入权限
- `7` ：读取 / 写入 / 执行权限

按照**所有者 所属组 其他人**的顺序表示即可。例如：ugo 格式的 `rwxrwxrwx` 转换为 bin 格式 `777`，ugo 格式的 `rw-r--r--` 转换为 bin 格式 `644`。

### xor 格式
即 bin 格式与 `777` 的异或。

## 文件搜索命令
```
find param/ opt "str"
find param/ opt1 "str1" -a opt2 "str2"
find param/ opt1 "str1" -o opt2 "str2"
find param/ opt "str" -exec cmd opt params \;
find param/ opt1 "str1" -a opt2 "str2" -exec cmd opt params \;
find param/ opt1 "str1" -o opt2 "str2" -exec cmd opt params \;
find param/ opt "str" -ok cmd opt params \;
find param/ opt1 "str1" -a opt2 "str2" -ok cmd opt params \;
find param/ opt1 "str1" -o opt2 "str2" -ok cmd opt params \;
locate param
locate -i param
updatedb
which param
whereis param
grep "str" param
grep -i "str" param
grep -n "str" param
grep -v "str" param
grep -r "str" param/
grep -ri "str" param/
grep -rn "str" param/
grep -rl "str" param/
grep -A7 "str" param
grep -B11 "str" param
grep -r -C5 "str" param/
```
- `find` ：在 `param` 目录下搜索满足 `str` 条件的文件，显示搜索到的所有文件。
  - `-name` ：若启用，则 `str` 为待搜索的文件名，可以使用 `*` 作为通配符。
  - `-iname` ：启用 `-name` 选项，但不区分大小写。
  - `-size` ：若启用，则 `str` 为待搜索的文件大小，按**数据块格式**（见下）。可以使用前置字符 `+` / `-` 来表示大于 / 小于。
  - `-type` ：若启用，则 `str` 为待搜索的文件类型。用 `d` / `l` / `f` 表示目录 / 符号链接 / 普通文件（文本文件或二进制文件）。
  - `-user` ：若启用，则 `str` 为待搜索文件的**所有者**用户名。
  - `-group` ：若启用，则 `str` 为待搜索文件的**所属组**名称。
  - `-amin` ：若启用，则 `str` 为待搜索文件的最后访问时间距离此刻的分钟数。可以使用前置字符 `+` / `-` 来表示大于 / 小于。
  - `-cmin` ：若启用，则 `str` 为待搜索文件的最后变更时间（指文件**属性**的变更）距离此刻的分钟数。可以使用前置字符 `+` / `-` 来表示大于 / 小于。
  - `-mmin` ：若启用，则 `str` 为待搜索文件的最后修改时间（指文件**内容**的修改）距离此刻的分钟数。可以使用前置字符 `+` / `-` 来表示大于 / 小于。
  - `-inum` ：若启用，则 `str` 为待搜索文件的 **_i_ 节点**编码。
  - `-a` ：在 `param` 目录下搜索同时满足 `str1` 条件和 `str2` 条件的文件，`str1` 和 `str2` 的格式分别由 `opt1` 和 `opt2` 决定。
  - `-o` ：在 `param` 目录下搜索满足 `str1` 条件和 `str2` 条件至少其一的文件，`str1` 和 `str2` 的格式分别由 `opt1` 和 `opt2` 决定。
  - `-exec` ：在 `param` 目录下搜索满足 `str` 条件的文件，并分别对搜索到的每个文件执行 `cmd opt params` 命令行，使用 `{}` 指代搜索到的文件路径。
  - `-ok` ：在 `param` 目录下搜索满足 `str` 条件的文件，并分别对搜索到的每个文件弹出确认信息，确认后对该文件执行 `cmd opt params` 命令行，使用 `{}` 指代搜索到的文件路径。
- `locate` ：在**资料库**中搜索 `param` 文件名，可以使用 `*` 作为通配符。
  - `-i` ：不区分大小写。
- `updatedb` ：更新**资料库**。`params` 需为**空**。
- `which` ：显示 `param` 命令的路径，同时显示命令别称。
- `whereis` ：显示 `param` 命令或配置的路径，同时显示命令或配置的帮助路径。
- `grep` ：在 `param` 文件中搜索关键词 `str` 所在行，显示搜索到的所有匹配行的内容。
  - `-i` ：不区分大小写。
  - `-n` ：显示行号。
  - `-v` ：若启用，则改为在 `param` 文件中搜索关键词 `str` 所**不在**行。
  - `-r` ：若启用，则改为遍历 `param` **目录**（递归地包括各个下级目录）中的所有文件，搜索关键词 `str` 所在行，显示搜索到的所有匹配行的内容及其所在文件路径。
  - `-l` ：若启用，则不显示搜索到的内容，只显示搜索到的匹配行所在的文件路径。
  - `-Anum` ：同时显示每个匹配行的后 `num` 行内容。
  - `-Bnum` ：同时显示每个匹配行的前 `num` 行内容。
  - `-Cnum` ：同时启用 `-Anum` 和 `-Bnum`。
### 数据块格式
即为文件大小与 1 个数据块大小的比值，其中 1 个数据块大小为 512 字节。

## 帮助查询命令
```
man param
man 1 param
man 5 param
whatis param
apropos param
info param
help param
```
- `man` ：查看命令或配置的帮助手册。`param` 为命令名称或配置名称。
- `man 1` ：查看命令的帮助手册。`param` 为命令名称。
- `man 5` ：查看配置的帮助手册。`param` 为配置名称。
- `whatis` ：显示命令简介。`param` 为命令名称。
- `apropos` ：显示配置简介。`param` 为配置名称。
- `info` ：查看命令或配置的帮助手册，进入**光标模式**。`param` 为命令名称或配置名称。
- `help` ：查看**内置命令**的帮助信息。`param` 为命令名称。_Linux 的**内置命令**是指没有路径的命令。_

## 时间设置命令
```
date time
date
```
- `date` ：修改系统时间为 `time`。`time` 为**时间格式**（见下）。若 `time` 为空，则改为显示当前系统时间。
### 时间格式
`MMDDhhmm[YYYY][.ss]`
- `MM` ：月份2位数。**必需**。
- `DD` ：日期2位数。**必需**。
- `hh` ：小时2位数。**必需**。
- `mm` ：分钟2位数。**必需**。
- `YYYY` ：年份4位数。可选。
- `ss` ：秒钟2位数。可选。

## 用户管理命令
```
useradd name
passwd name
passwd
who
w
uptime
```
- `useradd` ：新建用户。`name` 为用户名。
- `passwd` ：设置或更改用户密码。`name` 为用户名。若 `name` 为空，则默认为当前用户。若当前用户非管理员，则 `name` 需为**空**。
- `who` ：查看在线用户的**登录记录**（见下）。`params` 需为**空**。
- `w` ：查看在线用户的**详细记录**（见下）。`params` 需为**空**。
- `uptime` ：查看在线用户的详细记录，只显示第一行。`params` 需为**空**。

### 登录记录
```
username term time (IP)
```
- `username` ：用户名。
- `term` ：登录终端。`tty` 开头表示本地终端，`pts` 开头表示远程终端。
- `time` ：登录时间。格式为 `YYYY-MM-DD hh:mm`。
- `IP` ：远程终端的主机 IP 地址。

### 详细记录
```
systime up time, users, load average:
USER TTY FROM LOGIN@ IDLE JCPU PCPU WHAT
```
- `systime` ：当前系统时间。
- `up time` ：系统待机时间（从上一次服务器重启 / 关机到现在）。
- `users` ：在线用户数量。
- `load average` ：显示系统在过去的 1 / 5 / 15 分钟内的均衡负载指数。
- `USER` ：用户名。
- `TTY` ：登录终端。`tty` 开头表示本地终端，`pts` 开头表示远程终端。
- `FROM` ：登录终端的主机 IP 地址。`-` 表示本机。
- `IDLE` ：用户累计空闲时间。
- `JCPU` ：用户累计占用的 CPU 运时。
- `PCPU` ：用户当前执行的命令所占用的 CPU 运时。
- `WHAT` ：用户当前执行的命令名称。`-bash` 表示没有当前执行的命令。

## 压缩解压命令
```
gzip param
gzip -1 param
gzip -9 param
gzip -d param
gzip -f param
gzip -df param
gzip -r param/
gzip -dr param/
gzip -k param
gzip -dk param
gunzip param
gunzip -f param
gunzip -r param/
gunzip -k param
tar -cf param0 param
tar -czf param0 param
tar -rf param0 param
tar -uf param0 param
tar -tf param0
tar -xf param0
tar -xf param0 param
tar -xf param0 -C param1
tar -xf param0 param -C param1
tar -xzf param0
tar -xzf param0 -C param1
zip param1 param2
zip -1 param1 param2
zip -9 param1 param2
zip -r param1 param2/
zip -r param1 param2/ -x "str"
zip -e param1 param2
zip -u param1 param2
zip -m param1 param2
unzip param
unzip param -d param1
unzip -o param
unzip -n param
unzip -o param -d param1
unzip -n param -d param1
unzip param -x "str"
unzip param -d param1 -x "str"
unzip -l param
```
- `gzip` ：把 `param` 文件压缩为 `.gz` 格式，并后缀相应的扩展名。`param` 可以有多个且可以使用 `*` 作为通配符，此时分别对每个文件进行压缩。默认不会对符号链接进行压缩。
  - `-1` / `-9` ：让压缩速度尽可能快 / 慢，让压缩比率尽可能低 / 高。`1` ~ `9` 之间的任何一个整数也可以作为选项，压缩效果在二者之间的相应位置。
  - `-d` ：若启用，则改为对 `.gz` 格式的 `param` 文件进行解压，并恢复压缩前的文件名。`param` 可以有多个且可以使用 `*` 作为通配符，此时分别对每个文件进行解压。
  - `-f` ：覆盖同名旧文件（如果存在）。若不启用，如果压缩 / 解压后的文件名已在相同路径下存在，则不会对该文件执行命令；若启用，则对符号链接也会进行压缩。
  - `-r` ：若启用，则改为遍历 `param` 目录（递归地包括各个下级目录）中的所有文件，分别对每个文件进行压缩 / 解压。
  - `-k` ：若启用，则先对 `param` 文件进行备份，然后再对备份文件进行压缩 / 解压；若不启用，则不会保留压缩 / 解压前的文件。
- `gunzip` ：执行 `gzip`，并启用选项 `-d`。
- `tar` ：对磁带终端进行操作，具体命令由启用的第一个选项指定。_只有古董 Linux 才会配备磁带，设备文件名为 `/dev/rmt0`。_
  - `-f` ：若启用，则改为对 `.tar` 格式的 `param0` 文件进行操作，具体命令由启用的第一个选项指定。此选项不能作为启用的第一个选项，必须作为启用的最后一个选项。
  - `-c` ：若启用，则只能作为第一个选项，指定命令：创建 `.tar` 格式的 `param0` 打包文件，对 `param` 文件 / 目录进行备份并添加到 `param0`。`param` 可以有多个且可以使用 `*` 作为通配符，此时每个文件 / 目录会被依次备份并添加到 `param0`。
    - `-z` ：若同时启用 `-cz`，则先对 `.tar` 格式的临时文件进行操作，具体命令由选项 `-c` 指定，然后再把 `.tar` 格式的临时打包文件压缩为 `.tar.gz` 格式，并重命名为 `param0`。
  - `-r` ：若启用，则只能作为第一个选项，指定命令：不创建新的打包文件，直接对 `param` 文件 / 目录进行备份并添加到 `param0`（不覆盖），此时 `param0` 需为已存在的 `.tar` 格式打包文件；若不启用 `-r` / `-u`，则 `param` 不能是相同路径下已存在的文件。
  - `-u` ：若启用，则只能作为第一个选项，指定命令：同上，但新添加的备份会覆盖 `param0` 中的同名旧备份；若不启用 `-r` / `-u`，则 `param` 不能是相同路径下已存在的文件。
  - `-t` ：若启用，则只能作为第一个选项，指定命令：查看 `.tar` 格式的 `param0` 文件的内部信息，此时 `param` 需为**空**。
  - `-x` ：若启用，则只能作为第一个选项，指定命令：对 `.tar` 格式的 `param0` 文件进行解包（保留解包前的文件）。如果解包后的文件名已在相同路径下存在，则会覆盖同名旧文件。若 `param` 不为空，则改为在 `param0` 的内部信息中搜索 `param` 文件名，只对搜索到的文件依次进行备份并解包。
    - `-C` ：若启用，则解包到 `param1` 目录；若不启用，则默认为当前目录。
    - `-z` ：若同时启用 `-xz`，则先对 `.tar.gz` 格式的 `param0` 文件进行备份并解压，恢复为 `.tar` 格式的临时打包文件，然后再对 `.tar` 格式的临时文件进行操作，具体命令由选项 `-x` 指定，此时 `param` 需为**空**。
- `zip` ：创建 `.zip` 格式 `param1` 压缩文件，对 `param2` 文件进行备份和压缩，把压缩后的备份添加到 `param1`，并显示压缩比率。`param2` 可以有多个且可以使用 `*` 作为通配符，此时每个文件会被依次备份和压缩并添加到 `param1`。
  - `-1` / `-9` ：让压缩速度尽可能快 / 慢，让压缩比率尽可能低 / 高。`1` ~ `9` 之间的任何一个整数也可以作为选项，压缩效果在二者之间的相应位置。
  - `-r` ：压缩目录。若不启用，则 `param2` 不能为目录。
  - `-x` ：跳过文件名为 `str` 的文件。可以使用 `*` 作为通配符。
  - `-e` ：压缩时加密。若启用，则会弹出输入框提示，设置密码后按 `Enter` 保存。
  - `-u` ：若启用，则不创建新的压缩文件，直接对 `param2` 文件进行备份和压缩并添加到 `param1`（不覆盖），此时 `param1` 需为已存在的 `.zip` 格式压缩文件；若不启用，则 `param1` 不能是相同路径下已存在的文件。
  - `-m` ：若启用，则跳过备份步骤，直接进行压缩（不保留压缩前的文件）。
- `unzip` ：对 `.zip` 格式的 `param` 文件进行解压（保留解压前的文件）。`param` 可以有多个且可以使用 `*` 作为通配符，此时分别对每个文件进行解压。若为加密压缩文件，则会弹出输入框提示，设置密码后按 `Enter` 解压。
  - `-d` ：若启用，则解压到 `param1` 目录；若不启用，则默认为当前目录。
  - `-o` / `-n` ：若不启用，如果解压后的文件名已在相同路径下存在，则会分别对每个弹出确认信息，询问**是否**覆盖旧文件（若选**否**则保留旧文件，舍弃解压后的同名文件）；若启用，则不会弹出确认信息，全部选为**是** / **否**。
  - `-x` ：若启用，则解压后舍弃文件名为 `str` 的文件。可以使用 `*` 作为通配符。
  - `-l` ：若启用，则改为查看 `param` 文件的内部信息，不进行解压。
## TODO
```
◆网络命令
write      给在线用户发送即时通讯。param为收信人用户名。
           回车开始输入通讯内容，Ctrl+D保存并发送。
wall       给所有在线用户发送即时广播。param为广播内容。
ping       测试网络。param为请求包收件人IP。
           Ctrl+C结束测试。
-c         若启用，则param2为请求包收件人IP，param1为请求包发送次数。
ifconfig   给网卡设置IP。param1为网卡名称，param2为设置的IP。
           若param为空，则改为执行：查看当前网卡信息。
mail       给用户发送电子邮件。param为收件人用户名。
           回车开始输入邮件内容，Ctrl+D保存并发送。
           若param为空，则改为执行：进入当前用户的电子邮箱。
last       显示所有用户的在线记录以及服务器的重启记录。param需为空。
id         显示用户的uid。param为用户名。
lastlog    显示所有用户列表及其最后登录时间。param需为空。
-u         若启用，则改为执行：显示某用户的用户列表及其最后登录时间。param为用户的uid。
traceroute 发送数据包并跟踪。param为接受数据包的网址。
netstat    查看当前网络信息。
-t         查看TCP协议的当前网络信息。
-u         查看UDP协议的当前网络信息。
-a         查看本机的所有当前网络信息。
-l         查看本机当前监听的网络信息。
-r         查看本机当前路由的网络信息。
-n         查看IP和端口。
           常用组合：-tlun查看本机当前监听的端口；-an查看本机的所有当前网络连接；-rn查看本机当前路由表。
setup      进入网络配置图形界面。
nmtui      进入网络配置图形界面。
------电子邮箱------
邮件列表中>N开头的邮件是新邮件（未读）。
输入help按回车可查看电子邮箱内可用的命令列表。
输入邮件编号按回车可查看该邮件的内容。
输入h按回车可回到邮件列表。
输入d按空格并输入邮件编号按回车可删除该邮件。
输入q按回车可退出电子邮箱。
------在线记录------
username term IP Sun month date timein - timeout (span)
username：           用户名。
term：               登录终端。tty开头表示本地终端，pts开头表示远程终端。
IP：                 远程终端的主机IP。
Sun month date：     登录日期（星期 月 日）。
timein - timeout：   登陆时间 - 登出时间。
span                 在线时长。
------用户列表------
username term IP lastlogtime
username：    用户名。
term：        登录终端。tty开头表示本地终端，pts开头表示远程终端。
IP：          远程终端的主机IP。
lastlogtime： 最后登录时间（星期 月 日 时:分:秒 时区 年）。

◆关机重启命令
shutdown   执行关机或重启命令。param为执行时间，格式为hh:mm或now。
-h         执行关机命令。
-r         执行重启命令。
-c         取消前一个还未执行的关机命令。
halt       立即关机。param需为空。
reboot     立即重启。param需为空。
poweroff   立即断电。param需为空。
init 0     立即关机。param需为空。
init 6     立即重启。param需为空。

◆运行级别命令
init 1   切换至单用户模式（安全模式）。param需为空。
init 2   切换至多用户模式，禁用NFS服务。param需为空。
init 3   切换至多用户模式，开启NFS服务。param需为空。
init 5   切换至图形界面运行模式。param需为空。
runlevel 显示之前运行级别以及当前运行级别。param需为空。
         空格右边数字表示当前运行级别。
         空格左边数字表示之前运行级别。若为N则表示未切换过运行级别。

◆登出命令
logout  登出当前用户。param需为空。
        Ctrl+D快捷键执行logout。

◆Vim文本编辑器
vi  用Vim文本编辑器打开param文件。
    若param为不存在的文件，则改为执行：新建param文件并用Vim文本编辑器打开。
   （下文的616/639为举例之用，可换为任意正整数。）
------内部命令------
↑      向上移动光标。
↓      向下移动光标。
←      向左移动光标。
→      向右移动光标。
u      撤销上一步修改。连续执行可连续撤销多步。无法撤销已保存的内容。
ZZ     保存并退出。
i      进入输入模式，输入的字符会插入到光标左侧。
a      将光标强制向右移动（光标在行尾也会移动到不存在的空位）并进入输入模式，输入的字符会插入到光标左侧。
I      将光标移动至行首并进入输入模式，输入的字符会插入到光标左侧。
A      将光标强制移动到行尾右侧不存在的空位并进入输入模式，输入的字符会插入到光标左侧。
o      在光标所在行下方插入新行并进入输入模式，输入的字符会插入到光标左侧。
O      在光标所在行上方插入新行并进入输入模式，输入的字符会插入到光标左侧。
s      删除光标所在处字符并进入输入模式，输入的字符会插入到光标左侧。
Esc    退出输入模式。
0      将光标移动至行首。
$      将光标移动至行尾。
gg     将光标移动至第1行。
G      将光标移动至最后1行。
616G   将光标移动至第616行。
x      删除光标所在处字符，并存入剪切板。
616x   删除自光标所在处起的616个字符，并存入剪切板。
       无法跨行。若自光标所在处起直到行尾不足616个字符，则改为执行：删除自光标所在处起直到行尾的内容，并存入剪贴板。
D      删除自光标所在处起直到行尾的内容，并存入剪贴板。
dd     删除光标所在行，并存入剪贴板。
d↑     删除光标所在行及其上一行，并存入剪贴板。
d↓     删除光标所在行及其下一行，并存入剪贴板。
616dd  删除自光标所在行起的616行，并存入剪贴板。
dG     删除光标所在行及其下方所有内容，并存入剪贴板。
Y      复制光标所在行存入剪贴板。
yy     复制光标所在行存入剪贴板。
y↑     复制光标所在行及其上一行存入剪贴板。
y↓     复制光标所在行及其下一行存入剪贴板。
616yy  复制自光标所在行起的616行存入剪贴板。
yG     复制光标所在行及其下方所有内容存入剪贴板。
p      粘贴上一次存入剪贴板的内容至光标所在行下方。
       若上一次存入剪贴板的内容由x/616x/D所创建，则改为执行：粘贴上一次存入剪贴板的内容至光标右侧。
P      粘贴上一次存入剪贴板的内容至光标所在行上方。
       若上一次存入剪贴板的内容由x/616x/D所创建，则改为执行：粘贴上一次存入剪贴板的内容至光标左侧。
r      用接下来输入的第1个字符替换光标所在处的字符。
R      进入替换模式，用输入的字符替换光标所在处的字符。
Esc    退出替换模式。
/      搜索关键词（输入/keyword按回车则搜索关键词keyword）。
n      将光标移动至下一个关键词的位置。
------编辑命令------
以:开头的命令为编辑命令，输入编辑命令后回车执行。编辑命令也属于内部命令。
在宿主目录下的.vimrc文件中（没有的话就新建）写入编辑命令（省略开头的: ）可以使其永久生效。
以后该用户用Vim文本编辑器打开的任意文件都会立即自动执行这些命令。
: set nu                显示行号。
: set nonu              取消显示行号。
: 616                   将光标移动至第616行。
: 616,639d              删除第616至第639行，并存入剪贴板。
: set ic                以后搜索关键词时不区分大小写。
: set noic              以后搜索关键词时区分大小写。
: %s/str1/str2/g        将全文中所有str1字符串替换为str2字符串。str1中可用^或$表示行首或行尾标记。
                        若str2为空，则改为执行：将全文中所有str1字符串删除。
                        若str1为单独的^或$且str2不为空，则改为执行：在所有行首添加str2字符串。
: 616,639s/str1/str2/g  将第616至639行中所有str1字符串替换为str2字符串。str1中可用^或$表示行首或行尾标记。
                        若str2为空，则改为执行：将第616至639行中所有str1字符串删除。
                        若str1为单独的^或$且str2不为空，则改为执行：在第616至639行的所有行首添加str2字符串。
: %s/str1/str2/c        执行: %s/str1/str2/g，但每次替换前询问（y/n/a/q/l/^E/^Y）。
: 616,639s/str1/str2/c  执行: 616,639s/str1/str2/g，但每次替换前询问（y/n/a/q/l/^E/^Y）。
                        上述两条命令执行过程中弹出的询问信息需用键盘回复：
                        按Y键表示替换此处，按N键表示不替换（跳过）此处，按A键表示替换全部。
                        按Q键表示结束替换（跳过全部），按L键表示替换此处并结束替换（跳过全部）。
                        按Ctrl+E表示向上翻页，按Ctrl+Y表示向下翻页。
: x                     保存并退出Vim编辑器。
: q                     退出Vim编辑器，不保存修改。
: q!                    退出Vim编辑器，强制不保存修改。
: w                     保存修改，没有修改也会更新修改时间。
: w param               将当前文件（已修改的）内容另存为param文件。
: wq                    保存并退出Vim编辑器，没有修改也会更新修改时间。
: wq!                   强制保存并退出Vim编辑器（只读文件也会保存）。
                        此命令需要所有者权限或管理员权限才能执行。
: r param               将param文件的所有内容导入当前文件（从光标所在位置开始）。
: !cmdline              执行外部命令cmdline。
                        cmdline是完整的命令行，包含必要的cmd opt param。
                        执行完毕后按回车可回到内部命令。
: r !cmdline            执行外部命令cmdline，并把执行结果导入当前文件（从光标所在位置开始）。
                        cmdline是完整的命令行，包含必要的cmd opt param。
: map △☆ inkey        自定义快捷键。△表示在此处按Ctrl+V，☆表示在此处按要定义的快捷键（可以是组合键）。
                        inkey为快捷键所要触发的内部命令（一串按键序列，Esc键需用<ESC>表示）。
: ab str1 str2          自定义替换字段。接下来在输入模式中输入的所有str1字符串都会在空格或回车后自动替换成str2字符串。
```
