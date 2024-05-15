### 命令行安装Nvidia方法

首先，检测您的NVIDIA显卡型号和推荐的驱动程序。执行以下命令来完成。请注意，您的输出和推荐的驱动程序可能会有所不同：

```text
$ ubuntu-drivers devices
== /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
modalias : pci:v000010DEd00002206sv00001458sd0000403Fbc03sc00i00
vendor : NVIDIA Corporation
model : GA102 [GeForce RTX 3080]
driver : nvidia-driver-470 - distro non-free recommended
driver : nvidia-driver-470-server - distro non-free
driver : nvidia-driver-495 - distro non-free
driver : nvidia-driver-460-server - distro non-free
driver : xserver-xorg-video-nouveau - distro free builtin
```

从上述输出中，我们可以得出结论，当前系统安装了NVIDIA GeForce RTX 3080显卡，并且推荐安装的驱动程序是nvidia-driver-470。

安装驱动程序。如果您同意推荐的驱动程序，请随时再次使用ubuntu-drivers命令来安装所有推荐的驱动程序：

```text
$ sudo ubuntu-drivers autoinstall
```

或者，使用apt命令有选择地安装所需的驱动程序。例如：

```text
$ sudo apt install nvidia-driver-470
```

安装完成后，重新启动系统即可完成。

```text
$ sudo reboot
```

### 使用PPA软件仓库自动安装Nvidia Beta驱动程序的方法

使用graphics-drivers PPA软件仓库可以安装最新的Nvidia Beta驱动程序，但这可能会导致系统不稳定。首先将ppa:graphics-drivers/ppa软件仓库添加到您的系统中：

```text
$ sudo add-apt-repository ppa:graphics-drivers/ppa
```

接下来，确定您的显卡型号和推荐的驱动程序：

```text
ubuntu-drivers devices
== /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
modalias : pci:v000010DEd00002206sv00001458sd0000403Fbc03sc00i00
vendor : NVIDIA Corporation
model : GA102 [GeForce RTX 3080]
driver : nvidia-driver-470 - third-party non-free recommended
driver : nvidia-driver-460-server - distro non-free
driver : nvidia-driver-470-server - distro non-free
driver : nvidia-driver-495 - distro non-free
driver : xserver-xorg-video-nouveau - distro free builtin
```

安装Nvidia驱动程序与上述使用标准Ubuntu软件仓库的示例相同，您可以选择自动安装所有推荐的驱动程序：

```text
$ sudo ubuntu-drivers autoinstall
```

或使用apt命令有选择地安装驱动程序。例如：

```text
$ sudo apt install nvidia-driver-470
```

安装完成后，重新启动计算机：

```text
$ sudo reboot
```

### 使用官方[http://Nvidia.com](https://link.zhihu.com/?target=http%3A//Nvidia.com)驱动程序的手动安装步骤

识别您的NVIDIA VGA卡。以下命令可以帮助您识别您的Nvidia显卡型号：

```text
$ lshw -numeric -C display
或者
$ lspci -vnn | grep VGA
或者
$ ubuntu-drivers devices
```

下载官方Nvidia驱动程序。使用您的Web浏览器导航到官方Nvidia网站，并下载适用于您的Nvidia显卡的适当驱动程序。或者，如果您知道自己在做什么，可以直接从Nvidia Linux驱动程序列表中下载驱动程序。下载完成后，您应该得到一个类似下面所示的文件：

```text
$ ls
NVIDIA-Linux-x86_64-470.94.run
```

安装先决条件。编译和安装Nvidia驱动程序需要以下先决条件：

```text
$ sudo apt install build-essential libglvnd-dev pkg-config
```

禁用Nouveau Nvidia驱动程序。下一步是禁用默认的nouveau Nvidia驱动程序。按照此指南禁用默认的Nouveau Nvidia驱动程序。

> 警告  
> 根据您的Nvidia VGA型号，您的系统可能会出现问题。在这个阶段，准备好动手解决问题。重新启动后，您可能会完全没有图形界面。确保您的系统已启用SSH，以便能够远程登录，或者使用CTRL+ALT+F2切换到TTY控制台，并继续安装。

在继续下一步之前，请确保重新启动您的系统。

停止桌面管理器。为了安装新的Nvidia驱动程序，我们需要停止当前的显示服务器。最简单的方法是使用telinit命令切换到运行级别3。在执行以下Linux命令后，显示服务器将停止工作，请确保在继续之前保存所有当前的工作（如果有）：

```text
$ sudo telinit 3
```

按下CTRL+ALT+F1并使用您的用户名和密码登录以打开一个新的TTY1会话，或通过SSH登录。

安装Nvidia驱动程序。执行以下Linux命令开始安装Nvidia驱动程序，并按照向导进行操作：

```text
$ sudo bash NVIDIA-Linux-x86_64-470.94.run
```

Nvidia驱动程序现已安装。重新启动您的系统。

```text
$ sudo reboot
```

配置NVIDIA X Server设置。重新启动后，您应该能够从"Activities"菜单中启动NVIDIA X Server设置应用程序。


### 集成显卡的切换
打开 NVIDIA X Server Setting，如果没有的话就安装一下：

|   |   |
|---|---|
|1|sudo apt-fast install nvidia-settings nvidia-prime -y|

在 PRIME Profiles 选项可以切换性能模式和按需使用模式，单独使用 intel 集显选项是灰色的，可以使用命令开启，执行后重启电脑。

|                                      |                                                                                                                                            |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| 1  <br>2  <br>3  <br>4  <br>5  <br>6 | # intel 集显  <br>sudo prime-select intel  <br># 性能模式，nvidia 独显  <br>#sudo prime-select nvidia  <br># 按需切换  <br>#sudo prime-select on-demand |

重启后就能在 Setting - About 里面看到显卡只有集显了，原先是有独显和集显。

Ubuntu 22.04 的默认显示服务器用的 Wayland，禁用独显后可能会导致输入法启动不起来（中文输入法是用的搜狗），手动执行 `fcitx` 看到和 Wayland 相关的错误。

|   |   |
|---|---|
|1  <br>2  <br>3  <br>4  <br>5  <br>6  <br>7  <br>8  <br>9  <br>10  <br>11  <br>12  <br>13  <br>14  <br>15|# nathan @ gs-ubuntu in ~ [17:55:57]   <br>$ fcitx  <br>(INFO-21681 addon.c:151) Load Addon Config File:fcitx-kimpanel-ui.conf     <br>...  <br>  <br>(ERROR-21681 ime.c:432) fcitx-keyboard-tr-otk already exists  <br>(ERROR-21681 ime.c:432) fcitx-keyboard-us already exists  <br>auth ok  <br>(ERROR-21681 xim.c:239) Start XIM error. Another XIM daemon named ibus is running?  <br>(ERROR-21681 instance.c:443) Exiting.  <br>Warning: Ignoring XDG_SESSION_TYPE=wayland on Gnome. Use QT_QPA_PLATFORM=wayland to run on Wayland anyway.  <br>Warning: Ignoring XDG_SESSION_TYPE=wayland on Gnome. Use QT_QPA_PLATFORM=wayland to run on Wayland anyway.  <br>start  <br>sgim_gd_cell.bin copy fail  <br>No such file or directory: No such file or directory|

我也没有深入去看具体什么原因，既然跟 Wayland 有关，那就换成 Xrog 试试，logout 后 login 的界面选择 xrog 登录，输入法恢复正常。
### 挂载windows的硬盘
## 第二个方法

这个方法是永久性的，即使关机重启后也可以使用。  
打开/etc/fstab:

```bash
sudo vim /etc/fstab
```

在最后一行添加：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200404185211566.png)

### 梯子
clash-verge选择tunmode后还要手动配置代理127.0.0.1:7890