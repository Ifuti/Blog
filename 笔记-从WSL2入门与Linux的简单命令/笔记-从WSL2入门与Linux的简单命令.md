# Intro
在看《Linux C编程一站式学习》这本书，发现需要学习Linux，一番比对后决定从WSL2开始入手。

# Quick Start
## 安装
WSL2的安装极其简单，并且可以简单的迁移至非系统盘。
1. 在Windows功能面板中勾选**适用于Linux的Windows子系统**与**虚拟机平台**按提示重启。
2. 打开cmd，输入`wsl -l -o`便能看到所有可安装的发行版。
3. `wsl.exe --install Debian`安装Debian
4. `wsl --shutdown`先关闭wsl，然后才能迁移硬盘。
5. `wsl --export Debian 压缩包目标目录\Debian.tar`将Debian导出为.tar压缩包
6. `wsl --unregister Debian`先注销原系统，否则无法导入同名系统。
7. `wsl --import Debian 系统目标目录\ 压缩包所在目录\Debian.tar --version 2`将Debian导入到所需目录。
此时在cmd里输入`wsl`即可进入Debian，但进入后会发现是root账户，这是不好的。
创建一个账户后在cmd里输入`Debian.exe config --default-user <username>`即可更改默认账户。

## 配置apt镜像源
将`/etc/apt/sources.list`文件的内容更改成对应镜像链接即可。
这里以[中科大源](https://mirrors.ustc.edu.cn/help/debian.html)为例。
1. `cd /etc/apt/`进入目标目录
2. `sudo vi sources.list`以管理员权限启动vi以编辑文件。
3. 按`esc`进入命令模式，输入`gg`回到首行，再输入`dG`删除全部。
4. 按`i`进入输入模式，粘贴复制好的中科大源。
5. 输入`:`进入命令行模式`wq`按回车，保存并退出文件，完成更改。
这样就完成了apt镜像源的配置。

## Others
完成镜像源配置后输入`sudo apt update`更新apt列表。
WSL2的Debian并不自带man，建议安装一个。
在Linux下安装软件很简单，使用刚刚配置过的apt即可。
输入`sudo apt install man`即可安装man。

# 用户
创建一个新用户的命令是`useradd`，这个命令该怎么使用呢？
刚刚安装过的man就是在这种时候使用的。输入`man useradd`查看`useradd`的帮助文档。
可以看到用法是：
`useradd [选项] 用户名`
其中带中括号的部分（即选项）可以不输，即可以直接使用`useradd 用户名`来创建一个新用户。
遇到其他不懂的指令也可以直接使用man来进行查阅。

同理
删除账号用`userdel [选项] 用户名`，修改账号用`usermod [选项] 用户名`。
至于中间的选项，自行使用man查询即可。

修改用户密码需要root权限，否则就只能修改自己的密码，使用`passwd [选项] 用户名`来修改。

# 文件操作
## 文件处理常用命令
下面是几个常用的文件操作指令，学完了就能应付日常的文件操作需求了。
此处只是简单的介绍，详细用法请用`man`查看。
### ls（list files）: 列出目录及文件名。
用法：`ls 选项 目录`
有几个常用的选项要记住：
- -a ：全部的文件，连同隐藏文件（开头为 . 的文件）一起列出来。
- -l ：列出详细数据，包含文件的属性与权限等等数据。
如果最后的目录没有输入的话，则默认为当前目录。
多个选项可堆在一起使用，如输入-al即可同时使用-a和-l。

### cd（change directory）：切换目录
用法：`cd 路径`
这里的路径可以是相对路径也可以是绝对路径。

### pwd（print work directory）：显示目前的目录
用法：`pwd`
显示当前所在的文件夹。

### mkdir（make directory）：创建一个新的目录
用法：`mkdir [选项] 目录名`
加入-p可以直接创建多个目录。
加入-m可以直接配置目录权限。

### rmdir（remove directory）：删除一个空的目录
用法：`rmdir [选项] 目录名`
同样的，加入-p可以一次性删除多个空目录。

### cp（copy file）: 复制文件或目录
用法：`cp [选项] 源目录 目标目录`
加入-r可以进行递归复制，常用于复制整个目录。

### rm（remove）: 删除文件或目录
用法：`rm [选项] 目标目录`
加入-i可在删除前询问是否删除。
加入-r可进行递归删除，常用于删除整个目录。

### mv（move file）: 移动文件与目录，或修改文件与目录的名称
用法：`mv [选项] 源目录 目标目录`
如果目标文件已经存在。那么：
加入-f可以强制覆盖文件。
加入-i后会询问是否覆盖文件。
加入-u后只有当目标文件source比较新，才会覆盖文件。

## 目录权限
使用`ls -l`后显示的第一项即为目录的权限信息。
首字母代表目录类型，之后的九位字符每三个成一组，分别表示`用户`、`组`、`其他用户`的权限。
每一组的字符位置不变，分别为`rwx`，代表着`读`、`写`、`执行`，缺失的权限会由-替代。
使用`chmod`可以更改文件的权限。

# Reference
https://www.runoob.com/linux/linux-tutorial.html