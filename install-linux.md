# 换ubuntu软件安装源（使用阿里源）

>
>* sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak	备份原文件，防止出错，还可以还原
>* sudo gedit /etc/apt/sources.list	//编辑sources.list
>* 将sources.list里的原内容删除，使用下面阿里云地址替换
```	
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse   
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse  
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse  
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse  
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse  
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse  
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse  
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse  
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse  
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse 
``` 
>
>* 保存、退出。
>* sudo apt update  
>* sudo apt upgrade    
>
>
>* 将下面的内容复制到source.list中，将使用清华源替换原文件
```
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-security main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-security main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main multiverse restricted universe

```
>* 保存，退出
>* sudo apt update  
>* sudo apt upgrade 

# 搜狗输入法
>* sudo apt-get install fcitx
>>* 安装完成后，重启电脑，在设置中配置fcitx
>* dpkg -i ............deb  
		（如果报错，则执行 sudo apt -f install    安装依赖，安装完成后，再执行dpkg -i ............deb）


# 花屏问题
>* 安装系统时可能出现花屏的情况，结果办法  
>>* a、开机时，没有启动ubuntu之前，点击"e" 输入”nomodeset“，再按F10，进入安装界面。  
>>* b、开机后执行下面操作，可永久解决花屏问题   
>>>* b.1、sudo vi /etc/default/grub    
		将文件中的 GRUB_CMDLINE_LINUX_DEFAUL T="quiet splash"改为GRUB_CMDLINE_LINUX_DEFAULT="quiet splash nomodeset"
		
>>>* b.2、 sudo update-grub

# 无法创建或修改文件 提示 **xxxxxxx : Read-only file system**
>* 重新挂载根目录 /
>>* 

# 蓝牙鼠标延时
>* 获取蓝牙鼠标的蓝牙地址（我的MAC地址：E4:D7:38:B0:03:54）
>* sudo su	需要root权限
>* cd /var/lib/bluetooth/xxxx/E4:D7:38:B0:03:54	进入蓝牙鼠标文件
>* gedit info	配置info文件
>* 将下面内容复制到info文件里  
>	 
```
[ConnectionParameters]  
MinInterval=6  
MaxInterval=6  
Latency=59  
Timeout=300 
```
>* 将电脑重启后，蓝牙鼠标恢复正常

# 下载 vscode速度太慢的问题
>* 例如下载vscode 1.6.8版本,官网的下载链接是 "https://az764295.vo.msecnd.net/stable/30d9c6cd9483b2cc586687151bcbcd635f373630/code_1.68.1-1655263094_amd64.deb"  速度很慢,
>* 解决办法:
>>* 将官网下载链接的 "az764295.vo.msecnd.net" 部分替换成 "vscode.cdn.azure.cn", 替换后的下载链接 "https://vscode.cdn.azure.cn/stable/30d9c6cd9483b2cc586687151bcbcd635f373630/code_1.68.1-1655263094_amd64.deb"

# 更改权限，所有者，密码 添加用户  改变所属组 
## 更改权限
>* 给予文件可执行权限
>>* chmod a+x filename	  
>* 给予文件夹下的所有文件可执行权限
>>* chmod a+x folder/ -R
>* 给予文件夹下某类文件可执行权限
>>* chmod a+x folder/*.sh	 
## 获取所有权限
>* 更改某文件所有者
>>* chown username filename
>* 更改某文件下所有文件和文件夹的所有者
>>* chown username folder/ -R
## 添加用户
>* 添加普通用户
>>* sudo adduser username
`
[sudo] password: 
正在添加用户"tt"...
正在添加新组"tt" (1006)...
正在添加新用户"tt" (1006) 到组"tt"...
创建主目录"/home/tt"...
正在从"/etc/skel"复制文件...
输入新的 UNIX 密码： 
重新输入新的 UNIX 密码： 
passwd：已成功更新密码
正在改变 tt 的用户信息
请输入新值，或直接敲回车键以使用默认值
    全名 []: 
    房间号码 []: 
    工作电话 []: 
    家庭电话 []: 
    其它 []: 
这些信息是否正确？ [Y/n] y
`
>* 存在问题...
>>* username in not in sudoers files
>* 解决
>>*  切换到可以sudo 的用户b
>>* 编辑配置文件
>>>* vim /etc/sudoers
>>* 增加配置, 在打开的配置文件中，找到root ALL=(ALL) ALL, 在下面添加一行,其中xxx是你要加入的用户名称
>>>* xxx ALL=(ALL) ALL
## 更改密码
>>* sudo  passwd username
>>* "输入新密码"
>>* "确认密码"

## 改变所属组


## 更改用户名
>* 获取root权限
>>* sudo su
>* 给予新名称较高的权限
>>* vim /etc/sudoers
>>* 在 `root	ALL=(ALL:ALL) ALL` 下面增加：
>>>* newname	ALL=(ALL:ALL) ALL

>* 修改 /etc/shadow,将最后一行 oldname 修改为 newname  例如将`novotech:$6$PVAJZ3FI$dX4p3RxAnwgF2lxgL.P7xgH1YY6TH4McMtpkoep/zkaevFrs1yrk/I3I/piCY1zUzUq1ZOo/TzJW2JUxyTtRi.:18838:0:99999:7:::`改为`tdgk:$6$PVAJZ3FI$dX4p3RxAnwgF2lxgL.P7xgH1YY6TH4McMtpkoep/zkaevFrs1yrk/I3I/piCY1zUzUq1ZOo/TzJW2JUxyTtRi.:18838:0:99999:7:::`
>> :%s/novotech/tdgk/g
>
>* 修改目录名称
>>* cd /home
>>* mv oldname newname

>* 修改/etc/passwd
>* 将最后一行的 “oldname”改成“newname”  例如 `novotech:x:1000:1000:tdgk,,,:/home/tdgk:/bin/bash`  改成  `tdgk:x:1000:1000:tdgk,,,:/home/tdgk:/bin/bash`
>>* :%s/novotech/tdgk/g
>:
>* 修改所属用户组
>>* sudo vim /etc/group
>>* :%s/novotech/tdgk/g
>* 重启设备后 生效

## 更改主机名
>* 临时更改
>>* sudo hostname <new-hostname>
>* 永久更改
>>* ①hostnamectl set-hostname <new-hostname>   如果没有用使用下面的办法②
>>* ②-1 修改 /etc/hostname文件  将文件中的名称 改成需要的主机名
>>* ②-2 修改 /etc/hosts 文件,将文件中的名称改成需要的主机名
>* 重启后生效

# 添加桌面---安装字体

>* 将图标添加到桌面
>>* nautilus /usr/share/applications   
	  (参见：https://jingyan.baidu.com/article/90bc8fc8a82656f653640c3b.html)
>* 可以使用vim查看xxx.desktop文件，修改里面的属性	  
```
[Desktop Entry]
Name=Terminal  // 名称
Comment=Use the command line
Keywords=shell;prompt;command;commandline;cmd;
TryExec=gnome-terminal
Exec=/home/zhongsy/visual_script/test_visual.sh   // 命令
Icon=/home/zhongsy/视觉调试.jpg     // 图标
Type=Application
X-GNOME-DocPath=gnome-terminal/index.html
X-GNOME-Bugzilla-Bugzilla=GNOME
X-GNOME-Bugzilla-Product=gnome-terminal
X-GNOME-Bugzilla-Component=BugBuddyBugs
X-GNOME-Bugzilla-Version=3.18.3
Categories=GNOME;GTK;System;TerminalEmulator;
StartupNotify=true
X-GNOME-SingleWindow=false
OnlyShowIn=GNOME;Unity;
Actions=New
X-Ubuntu-Gettext-Domain=gnome-terminal

Name[en_US]=visul_debug
```

>
>* 安装字体
>>* 安装fira code : $ sudo apt install fonts-firacode
>
>* vscode-sync  同步的token号 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

# 自定义命令
>* 打开 **~/.bashrc** 文件, 找到以 **alias** 开头的代码段,添加命令, 例如
>>* alias hh='code /home/hdb/HDB_tempt/0—文档/install-linux.md'
>>* source ~/.bashrc

# ssh使用   
>* 连接服务器  
>>* ssh username@192.168.6.61	(username=hdb   192.168.6.61  服务器地址) 
>
>* 从服务器复制文件到本地  
>>* sshpass -p 68866565 scp -r -d folderaddr/file username@IP_addr:folderaddr
> 
>* 举例:复制服务器中的文件/Downloads/system_setup.txt到本地~/Downloads/目录下  
>>* sshpass -p 68866565 scp -r -d /Downloads/system_setup.txt username@192.168.3.85:/home/hdb/Downloads/
## ssh 免密登录
>* 获取root权限  `sudo su`
>* 使用root权限生成主机公约和密匙
>>* ssh-keygen  -t rsa
>>>* Enter file in which to save the key (/root/.ssh/id_rsa): /home/hdb/.ssh/id_rsa
>>>* 生成路径选择 `~/.ssh/id_rsa`
>>>>* 会在 `~/.ssh/id_rsa` 生成文件 `id_rsa.pub`
>* 在从机（需要免密登录的设备）创建 `~/.ssh/authorized_keys`
>>* touch ~/.ssh/authorized_keys
>* 将主机生成的文件 `id_rsa.pub` 拷贝到从机 `~/.ssh/` 中。
>* 在从机中将 `~/.ssh/id_rsa.pub` 写入到 `~/.ssh/authorized_keys` 中
>>* cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

#桌面远程连接
>* 使用方案 'vnc4server + xfce4 + xrdp'

>* 安装 ***vnc4server + xfce4***
>>* sudo apt-get install vnc4server xfce4
>* 启动vnc server命令：(下文中的:1 表示1号桌面, 因为共有4个桌面可以选择,打开1号桌面)
>>* vncserver -geometry 1280x800 -alwaysshared :1
>>>* 停止服务的命令:
>>>>*  vncserver -kill :1
>* 修改VNC 启动脚本
>>* cd ~/.vnc
>>* vim xstartup
```
#!/bin/sh
 
# Uncomment the following two lines for normal desktop:
#unset SESSION_MANAGER
#exec /etc/X11/xinit/xinitrc
 
#[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
#[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
#xsetroot -solid grey
#vncconfig -iconic &
#x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
#x-window-manager &
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
vncconfig -iconic &
xfce4-session &
```
>* 关闭vnc 服务器:
>>* vncserver -kill :1
>* 以后每次需要远程桌面时,只需要执行如下命令即可:
>>* vncserver -geometry 1280x800 -alwaysshared :1

>* 客户端连接工具: ***vnc viewer***



# ultraEdit 破解
>* linux版本 没有永久破解版，只是使用30天试用版，试用结束后，执行下列命令，还可以再接着用---
```
rm -rfd ~/.idm/uex   
rm -rf ~/.idm/*.spl   
rm -rf /tmp/*.spl   
```
# 创建开机自启动	
## 方法1 ： （测试无效）
>* [来源网址]（https://blog.csdn.net/t624124600/article/details/111085234）
>* CAN初始化 开机启动 *** 不需要root权限 ***
>>* 将文件拷到启动目录
>>>* sudo cp caninit.sh /etc/init.d
>>* 给予可执行权限
>>>* sudo chmod a+x caninit.sh
>>* 添加开机启动
>>>* sudo update-rc.d caninit.sh defaults 90
>>* 删除开机启动
>>>* sudo update-rc.d -f caninit.sh remove

## 方法二：（有效，启动的是root权限的应用，即，开机启动项对所有用户都是有效的）
>*  *** 需要 root 权限 ***
>>* 修改 ***rc-local.service*** 文件
>>>* cd /lib/systemd/system
>>>* sudo vim rc-local.service
```
#### 文件中本身就有的
[Unit]
Description=/etc/rc.local Compatibility
Documentation=man:systemd-rc-local-generator(8)
ConditionFileIsExecutable=/etc/rc.local
After=network.target

[Service]
Type=forking
ExecStart=/etc/rc.local start
TimeoutSec=0
RemainAfterExit=yes
GuessMainPID=no

####  需要自己添加
[Install]
WantedBy=multi-user.target
Alias=rc-local.service

```
>>* 创建 ***/etc/rc.local*** 脚本文件
>>>* sudo touch /etc/rc.local
>>* 给予rc.local可执行权限
>>>* sudo chmod a+x /etc/rc.local
>>* 启动服务
>>>* sudo systemctl enable rc-local
>>>* sudo systemctl start rc-local.service
>>>* sudo systemctl status rc-local.service
>>* 在rc.local中添加需要驱动的sheel文件绝对路径 即可开机运行命令

***使用rc.local配置启动文件，所启动的程序均为root启动， 如果需要使用某用户启动的话，可使用如下命令***
>* su tdgk -c "/home/tdgk/nx_test/init_nx.sh"


## 方法三：bash启动 （用户级的 开机启动， 仅对用户自身有效，不需要 root权限）
>* 改方法只能用户登录 或用户远程登录后才会触发 bash启动
>* 进入 ~/.profile
>>* vim ~/.profile
>* 在最后一行添加绝对路径的脚本文件
>>* source /../../../../../.sh

## 方法三： （用户级的 登录启动，登录成功后开始运行）
>* 1、创建.sh 文件；2、给予可执行权限；3、将 需要执行的命令写入.sh；4、将.sh文件复制到 `/etc/profile.d` 目录下。
>* 重启 `reboot`   完成！


# 通过配置文件修改静态IP地址
>* 打开进入配置文件
>>* cd /etc/netplan/
>>>* 如果没有netplan目录，则需要安装，安装命令：
>>>>* sudo apt-get install netplan.io
>
>* 在终端打开配置文件 *.yaml
>>* sudo vim /etc/net/plan/01-network-manager-all.yaml
>>* 如果没有**yaml**文件，则说明需要使用命令生成：
>>>* sudo netplan generate
>>* 生成的文件可能不在 /etc/netplan/
目录下，使用**find**命令查找。
>>>* find / -name *.yaml 2>>/dev/null
>>>* 使用上述命令应该会找到很多 ***yaml*** 文件，将 ***network_manager.yaml*** 复制到/etc/netplan目录下。
>
>* 打开**yaml**文件后，输入如下内容：

```
network:
    ethernets:
        eth0:
            dhcp4: no
            addresses: [192.168.1.111/24]
            optional: true
            gateway4: 192.168.1.1
            nameservers:
                    addresses: [223.5.5.5,223.6.6.6]
    version: 2
```
>>* ***注：***
>>>* **eth0**:端口号。 ----可以使用 `ifconfig -a` 进行查询，需要修改哪个网口， 就将eth0修改。
>>>* **dhcp4**:关闭ipv4 DHCP自动分配ip地址
>>>* **addresses**:ip地址和子网掩码。 ----192.168.1.111/24：表示ip地址是192.168.1.111，使用24位子网掩码 255.255.255.0
>>>* **optional**:不懂
>>>* **gateway4**:网关。 ----网关设置为192.168.1.1
>>>* **nameservers->addresses**:DNS1：223.5.5.5，DNS2：223.6.6.6-----如果需要连接外网，可以设置该项。
>
>* 保存，退出后，执行下列命令使配置文件生效
>>* sudo netplan apply
>
>* 使用终端检测上网是否正常
>>* ping www.baidu.com

## NX板 使用netplan 配置网络时出现的问题和解决
>* 问题描述:
>>* 接线情况,NX板GBE接外网, GIGE1接工业相机(内网),NX则无法上网.  在不断电的情况下,拔掉工业相机,则可以上网, 即使再工业相机接上,网络依然正常.
>* 原因分析
>>* 使用命令 **route -n** 查看NX板的路由表时,发现NX板默认使用的网卡是 eth1,即默认使用GIGE1网口,因此无法正常.
>* 解决:
>>* 修改 ***network_manager.yaml*** 配置文件, 将网卡 **eth0**以外的网卡的网关(gateway)注释掉即可.  如果想设置网关,可以使用命令 ***sudo route add -net 192.168.201.1 netmask 255.255.255.0 dev eth1*** （让对192.168.0.0的访问走eth1网卡，netmask 后面是子网掩码）.这个命令关机后失效, 如果想永久有效,将这条命令加入开机启动命令中.
 
# 测试网速 iperf3
>
>* 安装iperf3: 需要到官网下载安装包   [iperf网址](https://iperf.fr/iperf-download.php)
>* **举例**
>>* 测试NX板网口的网速
>>>* 通过ssh 连接 NX 将NX板作为服务器：
>>>>* iperf3 -s -B 192.168.x.xxx			（-B  指定某个固定的网口）
>>>* 在PC端 使用下列命令 测试网速
>>>>* iperf3 c 192.68.x.xxx -p 5201
>
>* 提高网速，使用命令
>>* sudo ifconfig mtu 9000 up


# 测试音频 sox
>
>* sudo apt-get install sox # 安装
>* 以下安装是为了支持mp3格式，可选
>>* sudo apt-get install lame
>>* sudo apt-get install libsox-fmt-mp3

# 批量替换 某路径的字符串
>* 例如将 `~/Downloads`目录下的所有文件（包含子目录）中的字符串 “gk/” 替换成 “zf”
`
sed -i s/"gk\/"/"zf"/g `grep -rl "gk/" ./ `

	其中，`grep 中的 ` 不是 单引号，时"ESC"下的那个键。
`

# jetson-NX 刷机
>* 插上USB烧写线,接着recovery跳针开机
>* 进入目录,执行 nx-recovery.sh 脚本

# jetson 将系统由eMMC转移到SSD中
## jetson如果时初次转移到SSD
>
>* 查询是否有设备SSD
>>* lsblk
>>>* 如果显示有“nvme0n1” 则表示识别到固态硬盘
>
>* 格式化固态硬盘
>>* 进入parted
>>>* sudo parted /dev/nvme0n1 
>>* 将磁盘设置为gpt格式
>>>* mklabel gpt
>>* 从磁盘的4096给扇区开始将余下容量设置为GPT格式
>>>* mkpart logical 4096s 100%
>>* 退出
>>>* quit
>
>* 进行分区
>>* sudo fdisk /dev/nvme0n1
```
Command (m for help): n
Partition number (2-128, default 2): (单击ENTER,使用默认值)
First sector (34-250069646, default 2048): (单击ENTER,使用默认值)
Last sector, +sectors or +size{K,M,G,T,P} (2048-4095, default 4095): (单击ENTER,使用默认值)

Created a new partition 2 of type 'Linux filesystem' and of size 1 MiB.

Command (m for help): q
```
>
>* 格式化分区
>>* sudo mke2fs -t ext4  /dev/nvme0n1p1
>>* 将文件夹 ***rootOnNVMe*** 复制到 jetson中，例如 ~/Downloads目录下
>>* 进入 ***rootOnNVMe***目录
>>>* cd ~/Downloads/rootOnNVMe
>>* 给予.sh文件可执行权限
>>>* chmod a+x *.sh
>>* 执行复制文件
>>>* sudo ./copy-rootfs-ssd.sh
>>* 等待执行结束后，最后再执行：
>>>* sudo ./setup-service.sh
>>* 重启jetson
>>>* reboot

## 将系统从SSD转移到EMMC
>* 挂载emmc文件系统
>>* sudo mount /dev/mmcblk0p1 /mnt
>* 删除文件 ***/mnt/etc/setssdroot.conf***
>>* sudo rm /mnt/etc/setssdroot.conf
>>>*  在 ***setup-service.sh***中有说明
## 再次将系统由EMMC转移到SSD
>* 创建空文件 ***/etc/setssdroot.conf***
>>* sudo mkdir /etc/setssdroot.conf
>>>*  在 ***setup-service.sh***中有说明

### 在系统运行期间  可以出现 系统自动从 SSD转移到EMMC  (原因未知)
>* 解决:
>>* 首先使用命令 **lsblk** 查看 板子能否识别到 SSD, 如果可以显示 **nvme0n1p1**,说明固态硬盘是好的
>>* 需要重新挂载磁盘 **nvme0n1p1**  命令是: sudo mount /dev/nvme0n1p1 /mnt, 然后执行  sudo ./setup-service.sh, 重启后,系统就会从SSD启动.
>>>* 在上述执行sudo mount xxxxxx 的时候可能会报错误: ***mount: /mnt: cannot mount; probably corrupted filesystem on /dev/nvme0n1p1***
>>>>* 解决: 执行 ***sudo fsck.ext4 -v /dev/nvme0n1p1***  然后 一路输入 **y** 即可修复磁盘"nvme0n1p1". 然后重新执行  sudo mount /dev/nvme0n1p1 /mnt   和  sudo ./setup-service.sh  即可.

# 安装ROS系统  
> [参考网址]（https://blog.csdn.net/KIK9973/article/details/118755045）
>
>* 替换镜像源
>>* 如果时PC端安装，可将镜像源替换成“阿里源”， 如果是 ARM板安装  可将镜像源替换成“清华源”
>
>* 接下来是将ros 的下载源设置为中科大源
>>* sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.ustc.edu.cn/ros/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list'
>* 设置公钥： 在终端输入如下命令后回车
>>* sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
>* 更新最新可用软件包列表
>>* sudo apt update
>
>* 安装ROS
>>* sudo apt install ros-melodic-desktop-full
>
>* 设置环境变量
>>* echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
>>* source ~/.bashrc
>
>* 下载其他功能组件
>>* sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
>
>* rosdep init 初始化
>>* sudo rosdep init
>>>* 教程上说，这步可能会报错。 我执行的时候，这一步一次就通过了，没有报错。如果报错，请参考网址解决。
>
>* rosdep update 更新
>>* rosdep update
>>>* 出现更新超时  “time out” 报错,按照参考教程办法 可以解决问题。
>
>* 再次执行 rosdep update
>>* 这次可能会报错， 按照教程办法 关闭终端，重新打开终端，在新终端中输入 rosdep update    如果还是失败， 重复执行，  我大概重复了5次 才成功。

# 电脑间文件互传： FTP服务器
## 服务器安装
>* 安装FTP服务器 (for ubuntu)
>>* sudo apt-get install vsftpd
>* 配置文件
>>* sudo vim /etc/vsftpd.conf
>>* 在vsftpd.conf中找到 **local_enable=YES** 和 **write_enable=YES** 这两行。如果这两行前面有***#*** 将注释符号删除
>>* 保存，退出！
>* 输入命令，重启FTP服务
>>* sudo /etc/init.d/vsftpd restart

##安装客户端
>
>* 安装客户端（for linux）
>>* sudo apt-get install filezilla
>*

# 关闭和开启图形界面
>* 关闭用户图形界面，使用tty登录。
>>* sudo systemctl set-default multi-user.target
>
>* 开启用户图形界面。
>>* sudo systemctl set-default graphical.target
>
>* 重启后生效：
>>* reboot

# DeepStream安装
 ***************************************************************************************************/

一、安装GStreamer
	1.1、安装
		$sudo apt-get install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-doc gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-pulseaudio
	1.2、使用GStreamer创建工程
		pkg-config --cflags --libs gstreamer-1.0
	1.3、获取例程
		$sudo git clone https://gitlab.freedesktop.org/gstreamer/gst-docs
	1.4、验证
	1.4.1、编译
		$gcc basic-tutorial-1.c -o basic-tutorial-1 `pkg-config --cflags --libs gstreamer-1.0`
	1.4.2、运行
		。。。。。。。。。。。。。

二、关闭默认驱动，使用NVIDIA驱动
	2.1、参看计算机使用驱动
		$lsmod | grep nouveau     输入命令后，如有输出。。。。表示目前使用的是linux默认驱动nouveau。
	
	2.2、关闭nouveau驱动
	2.2.1打开文件：
		$ sudo vim /etc/modprobe.d/blacklist.conf
		在文末输入：
			blacklist nouveau
			options nouveau modeset=0
	2.2.2保存、退出后执行命令：$sudo update-initramfs -u	进行更新	
	2.3电脑重启
	2.4使用命令：$lsmod | grep nouveau	若无输出，则说明默认驱动nouveau关闭成功。
	2.5、下载NVIDIA驱动
		打开“软件和更新————>附加驱动”下载nvidia-driver-465
	2.6、电脑重启
	2.7使用命令：$nvidia-smi	查看NVIDIA驱动版本

三、安装CUDA11.1
	3.1、下载、安装
		进入网址：https://developer.nvidia.com/cuda-11.1.1-download-archive?target_os=Linux  下载cuda11.1.1版本软件。。。。。或直接使用命令下载：
			$wget https://developer.download.nvidia.com/compute/cuda/11.1.1/local_installers/cuda_11.1.1_455.32.00_linux.run
			（若出现“3.29G  2.28MB/s    剩余 1s    s段错误 (核心已转储)”错误，基本都是“stack size ”太小，使用命令：$ulimit -s 102400	将“stack size ”增到到100M。	
			然后使用命令：$wget -c https://developer.download.nvidia.com/compute/cuda/11.1.1/local_installers/cuda_11.1.1_455.32.00_linux.run,接着下载。）
			
			$sudo sh cuda_11.1.1_455.32.00_linux.run
	3.2、配置.bashrc文件
		$vim ~/.bashrc		在文末添加：
		export PATH=/usr/local/cuda-11.1/bin${PATH:+:${PATH}}
		export LD_LIBRARY_PATH=/usr/local/cuda-11.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
		保存、退出。
		使用命令：$ source ~/.bashrc
	3.3、可以使用nvcc -V查看CUDA版本
四、安装cuDNN
	4.1、下载cudnn-11.2-linux-x64-v8.1.1.33
	4.2、复制文件
		$ sudo cp (cudnn目录)/cuda/include/cudnn.h /usr/local/cuda-11.1/include
		$ sudo cp (cudnn目录)cuda/lib64/libcudnn* /usr/local/cuda-11.1/lib64
		$ sudo chmod a+r /usr/local/cuda-11.1/include/cudnn.h 
		$ sudo chmod a+r /usr/local/cuda-11.1/lib64/libcudnn*

五、安装tensorRT
	5.1、下载TensorRT
	5.2、解压到	
		$ sudo tar -xzvf TensorRT-7.2.3.4.Ubuntu-18.04.x86_64-gnu.cuda-11.1.cudnn8.1.tar.gz -C /usr/local/	将TensorRT解压到/usr/local

	5.3、使用命令：
			vim ~/.bashrc	在文末添加:export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/TensorRT-7.2.3.4/lib		保持、退出。
		执行命令：source ~/.bashrc

	5.4、使用命令：$ python3 -V	查看python3版本
	5.5、安装
		$ cd /usr/local/TensorRT-7.2.3.4/python
		$ sudo -H pip3 install tensorrt-7.2.3.4-cp36-none-linux_x86_64.whl   (若没有安装pip3,执行命令：$ sudo apt install python3-pip	进行安装)
		$ cd ../uff/		计划将TensorRT与TensorFlow一起使用时，安装uff才是必要的)
		$ sudo -H pip3 install uff-0.6.9-py2.py3-none-any.whl

六、安装依赖库：
	6.1 $ sudo apt install \
     libssl1.0.0 \
     libgstrtspserver-1.0-0 \
     libjansson4
	6.2、安装librdkafka
		$ sudo git clone https://github.com/edenhill/librdkafka.git
		(如果报错，fatal: unable to access.....。将上面命令中的https 改成git,执行命令：sudo git clone git://github.com/edenhill/librdkafka.git)
		$ cd /usr/local/librdkafka/
		$ cd librdkafka/
		$ git reset --hard 7101c2310341ab3f4675fc565f64f0967e135a6a
		$ sudo ./configure
		$ sudo make	(若出现 “/usr/bin/env: "python": 没有那个文件或目录” 	则执行 、$ sudo ln -s /usr/bin/python3.6 /usr/bin/python	建立软链接。)
	
七、安装deepstream toolkit
	7.1、下载	deepstream_sdk_v5.1.0_x86_64.tbz2
	7.2、$ sudo tar -jxvf deepstream_sdk_v5.1.0_x86_64.tbz2 -C /	解压到根目录
	7.3、$ cd /opt/nvidia/deepstream/deepstream-5.1/	进入目录
	7.4、$ sudo ./install.sh	安装文件
		
		(sudo ln -sf /usr/local/cuda-11.1/targets/x86_64-linux/lib/libcudnn_adv_train.so.8.0.1 /usr/local/cuda-11.1/targets/x86_64-linux/lib/libcudnn_adv_train.so.8
sudo ln -sf /usr/local/cuda-11.1/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8.0.1 /usr/local/cuda-11.1/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8
sudo ln -sf /usr/local/cuda-11.1/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8.0.1 /usr/local/cuda-11.1/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8
sudo ln -sf /usr/local/cuda-11.1/targets/x86_64-linux/lib/libcudnn_adv_infer.so.8.0.1 /usr/local/cuda-11.1/targets/x86_64-linux/lib/libcudnn_adv_infer.so.8
sudo ln -sf /usr/local/cuda-11.1/targets/x86_64-linux/lib/libcudnn_ops_train.so.8.0.1 /usr/local/cuda-11.1/targets/x86_64-linux/lib/libcudnn_ops_train.so.8
sudo ln -sf /usr/local/cuda-11.1/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.8.0.1 /usr/local/cuda-11.1/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.8
)


	7.5、$ sudo ldconfig

八、安装DeepStream例程中需要的要求的库（例程README中有要求）
	8.1、sudo apt-get install libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev \
   libgstrtspserver-1.0-dev libx11-dev

##			************************************使用过程中出现的问题和解决******************************
>* 使用deepstream 或gstreamer时.报 “libxxxxx.so”不存在
>>* 使用 `gst-inspect-1.0 -b` 查看 blacklist 是否有很多库 没有被加载
>>>* 如果有很多，同时 使用find / -name libxxxx.so    发现库存在，可以实现如下办法解决
>>>* 删除gstreamer缓存，让它重新加载库
>>>>* rm ~/.cache/gstreamer-1.0/registry.aarch64.bin
/==================================================================================================/


# Docker安装
 ***************************************************************************************************/

1、卸载旧版本docker
	sudo apt-get remove docker docker-engine docker-ce docker.io

2、更新ubuntu的apt源索引
	sudo apt-get update

3、配置安装包允许apt通过HTTPS使用仓库
	sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

4、添加Docker官方GPG key
	4.1、curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
	4.2、sudo apt-key fingerprint 0EBFCD88

5、设置Docker稳定版仓库
	sudo add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) \
  stable"

6、再次更新apt源索引
	sudo apt-get update

7、安装最新版Docker CE（社区版）
	sudo apt-get install docker-ce

{

如果要安装指定版本的docker按如下操作（不需要也可以跳过这步操作）
	$ apt-cache madison docker-ce    # 列出可用的docker-ce版本
	$ sudo apt-get install docker-ce=18.06.1~ce~3-0~ubuntu    #安装指定的docker版本
}

8、拉取hello-world镜像测试docker容器
	sudo docker run hello-world

9、可使用docker -v查看docker版本


二、安装 nvidia-docker	（网址：https://blog.csdn.net/m0_37167788/article/details/104824390）

1、执行下 命令
$ curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \ 
	sudo apt-key add -

$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID)

$ curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list

$ sudo apt-get update

2、安装nvidia-docker
	sudo apt-get install -y nvidia-docker2


3、安装一个docker容器进行测试
	sudo pkill -SIGHUP dockerd

	sudo docker run --runtime=nvidia --rm nvidia/cuda:10.1-base nvidia-smi


/*-----------------------------------zmkg-proV2.2.tar镜像使用说明-------------------------------×/
1、 镜像导入
	在zmkg-proV2.2.tar目录下执行：
	$ sudo docker  load  --input  zmkg-proV2.2.tar


2、 构建配置目录
	$ mkdir   ~/zmkg-pro

3、 拷贝配置
	$ cp -r config ~/zmkg-pro

4、 创建容器
	$ sudo docker run  --runtime nvidia -it  --privileged -v /dev:/dev  -p 8088:8088 -v ~/zmkg-pro/config:/home/zmkg-pro/config zmkg-pro:v2.2 /bin/bash

5、 拉取视频流
流地址:
rtmp://主机IP:8088/live/stream1
rtmp://主机IP:8088/live/stream2
rtmp://主机IP:8088/live/stream3
rtmp://主机IP:8088/live/stream4
rtmp://主机IP:8088/live/stream5

注意:
目前配置里的数据源地址为测试视频,如果要接入摄像头则需要修改数据源地址.
如:
配置原来测试数据源stream_addr=config/cut2.mp4
修改后变为stream_addr=/dev/video0
其它源地址类似修改即可

/*------------------------------------------------------------------------------------------*/

Docker运行镜像时问题和解决：
一、nvidia-docker 数据流出现“Bind for 0.0.0.0:8088 failed: port is already allocated.”
	解决：
	1、$ sudo docker ps		//查看进程，发现相关的容器并没有在运行，而 docker-proxy 却依然绑定着端口：
	2、$ ps -aux | grep -v grep | grep docker-proxy		//检查docker镜像		
	3、$ sudo service docker stop				//停止 doker 进程
	4、$ docker rm $(docker ps -aq)				//删除所有容器
	5、$ sudo rm /var/lib/docker/network/files/local-kv.db	//删除 local-kv.db 这个文件
	6、$ sudo service docker start				//再启动 docker

# Docker容器 镜像 卸载
>* 批量删除所有容器（一般都会有守护，需要先关掉才能删除）
>>* docker stop `docker ps -a -q`
>>* docker rm `docker ps -a -q`
>* 批量删除所有镜像
>>* docker rmi `docker images -q`
>* 卸载docker环境
>>* docker --version   #看版本 确定是否还在
>>* apt-get remove docker （docker-ce）
>>* rm -rf /var/lib/docker*
/==================================================================================================/

# Jetson -NX

>* 查看信息指令：
>>* jtop
>>>* 如果没有安装，执行命令 ->`sudo pip3 install jetson-stats` 进行安装。
>>>* 如果安装失败，提示pip3未安装 则执行
>>>>* sudo apt install python3-pip

# libcurl 安装
>* 安装
>>* sudo ./configure --prefix=/usr/local/curl-7.80.0/ --with-wolfssl
>>* sudo make
>>* make install

>* 添加环境变量
>>* vim ~/.bashrc
>>* 添加 :
>>>* `export PATH=$PATH:/usr/local/curl-7.80.0/bin`
>>* source ~/.bashrc
>*
>* 验证
>>* curl --help
>*
>* CMakeLists.txt 追加“头文件路径”和 “库路径“
>* link_directories(/usr/local/curl-7.80.0/lib)
>* include_directories(/usr/local/curl-7.80.0/include)
>
## 可能存在问题和解决
>* 问题：
>>* /usr/bin/curl: /usr/local/lib/libcurl.so.4: no version information available (required by /usr/bin/curl)
>* 解决
>>* 首先定位一下 libcurl 的位置：
>>>* locale libcurl.so.4		或
>>>* find / -name libcurl.so.4 2>>/dev/null
```
    /usr/lib/x86_64-linux-gnu/libcurl.so.4
    /usr/lib/x86_64-linux-gnu/libcurl.so.4.3.0
    /usr/local/lib/libcurl.so.4
    /usr/local/lib/libcurl.so.4.4.0
```
>>* 将这个冲突的软链接删掉：
>>>* rm -rf /usr/local/lib/libcurl.so.4
>>* 然后，将 4.3.0 的静态库链接到上面：
>>>* ln -s /usr/lib/x86_64-linux-gnu/libcurl.so.4.3.0 /usr/local/lib/libcurl.so.4


# 精简 NX emmc  
>* 可以删除：
>>* sudo rm -r /usr/src/cudnn_samples_v8
>>* sudo rm -r /usr/src/jetson_multimedia_api
>>* sudo rm -r /usr/src/nvidia
>>* sudo rm -r /usr/src/tensorrt
>>* sudo rm -r /usr/local/cuda/targets/aarch64-linux/lib/*.a* \
	/usr/local/cuda/doc \
	/usr/local/cuda/samples
>>* sudo rm -r /usr/lib/aarch64-linux-gnu/libcudnn*.a
>* 删除cuda 静态库(*.a),文档(doc),例程(samples)  约1.7G
>>* sudo rm -r /usr/local/cuda/targets/aarch64-linux/lib/*.a* \
	/usr/local/cuda/doc \
	/usr/local/cuda/samples
>* 删除cuDNN 静态库  约1.5G 
>>* sudo rm -r /usr/lib/aarch64-linux-gnu/libcudnn*.a

# 删除cuDNN 静态库  约1.5G 
sudo rm -r /usr/lib/aarch64-linux-gnu/libcudnn*.a


# 在NX 上修改迈德威视 工业相机的IP地址
>* 编辑 /etc/sysctl.d/10-network-security.conf 将其中的 3 项 *.rp_filter=1  改成 *.rp_filter=0
>* ***有时候,上述命令存在问题, 经过试验发现,执行上述修改后,有的NX板依旧不能实现跨网段 广播的收,发. GigE口无法识别相机***,原因: 迈德威视相机驱动中 采用 "广播"的方式检测GigE口有没有接相机, 但是NX的配置文件中 过滤了广播消息. 通常修改上述文件,可关闭对广播消息的过滤. 但是,由于未知的原因,有时候 修改 **/etc/sysctl.d/10-network-security.conf** 中的配置后,却不能生效.

>* 上述问题解决:
>>* 修改 /etc/sysctl.conf文件, 通常这个文件里放着关于 系统和内核的一些设置. 对文件做如下修改:
将
```
#net.ipv4.conf.default.rp_filter=1
#net.ipv4.conf.all.rp_filter=1
```
修改
```
net.ipv4.conf.default.rp_filter=0
net.ipv4.conf.all.rp_filter=0

net.ipv4.conf.eth1.rp_filter = 0
net.ipv4.conf.eth2.rp_filter = 0
net.ipv4.conf.eth3.rp_filter = 0
net.ipv4.conf.eth4.rp_filter = 0
```
>>* 使用命令 ***sysctl -p***  使配置生效.
>* 出现问题: 每次开机还是需要都需要执行 **sysctl -p** 才能生效
>>* 临时解决方案: 开机自动执行 **sysctl -p** .

>>>* 补充说明: 上述修改可永久生效,但是存在安全问题...  如果只是想临时使用,可使用如下办法:
>>>* 在***/proc/sys/net/ipv4/conf/*** 目录下放着 所有网卡的配置的文件,我们如果想让某一个(例如eth3)接收到广播消息,可以修改-->
>>>* **/proc/sys/net/ipv4/conf/all/** , **/proc/sys/net/ipv4/conf/default/** , **/proc/sys/net/ipv4/conf/eth3/** 目录下的文件 ***rp_filter*** 将3个文件中的1 改成0 ,命令如下
>>>>* sudo su
>>>>* echo "0" > /proc/sys/net/ipv4/conf/all/rp_filter \
	echo "0" > /proc/sys/net/ipv4/conf/default/rp_filter \
	echo "0" > /proc/sys/net/ipv4/conf/eth3/rp_filter
>>>>* sysctl -p

# 一些常用命令
>* 查看软连接
>>* sudo ldconfig -v
>
>* 添加库路径：可以在 ` ~/.bashrc` 或 `/etc/ld.so.conf.d ` 
>
>* 列出 可执行程序所依赖的所有库
>>* ldd -v xxxx

# 安装modbus调试工具 modbus-utils
>* 安装依赖
>>* sudo apt install libmodbus-dev
>* 下载源码 以及编译安装
>>* git clone https://github.com/Krzysztow/modbus-utils
>>* cd modbus-utils
>>* git submodule update --init
>>* mkdir build
>>* cd build
>>* cmake ..
>>* make 
>>>* 在进行 ***cmake ..*** 时可能报错: ***install TARGETS given no RUNTIME DESTINATION for executable target***,则需要修改CMakeLists.txt,
>>>>* 在原文 ***install(TARGETS modbus_server modbus_client DESTINATION ${CMAKE_INSTALL_BINDIR}PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE)***  之前添加 ***include(GNUInstallDirs)***,具体如下
```
include(GNUInstallDirs)
install(TARGETS modbus_server modbus_client DESTINATION ${CMAKE_INSTALL_BINDIR}
        PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE)
```

## 使用modbus-utils
>* modbus_client 功能选项
```
-m : rtu / tcp
-a : slave id
-r : 写入或读取的开始地址
-t : 功能码，
	(0x01) Read Coils, (0x02) Read Discrete Inputs, (0x05) Write Single Coil
	(0x03) Read Holding Registers, (0x04) Read Input Registers, (0x06) WriteSingle Register
	(0x0F) WriteMultipleCoils, (0x10) Write Multiple register
-b : 串口波特率
-d : 数据位
-s : 停止位
-p : odd 、 even 、none（奇偶校验）
-c : 读取的数目
```
>>* 示例:
>>>* ./modbus_client --debug -mrtu -a1 -r1 -t0x10 -b38400 -d8 -s1 -peven /dev/ttyUSB0 0x1234 0x4321

>* modbus_server 功能选项
```
-m : rtu / tcp
-a : slave id

#若是rtu，有如下选项
-b : 串口波特率
-d : 数据位
-s : 停止位
-p : odd 、 even 、none（奇偶校验）

#若是tcp，有如下选项
-p：端口号
```

# 非 ***root*** 状态下打开串口的办法
>* 将当前用户添加到 **dialout** 用户组
>>* sudo usermod -aG dialout username
>* 重启电脑
>>* reboot

# deepstream 或gstreamer 调试
>* 在代码中添加:
>>* GST_DEBUG_BIN_TO_DOT_FILE(GST_BIN(pipeline), GST_DEBUG_GRAPH_SHOW_ALL, "gstream-03");
>* 运行代码:
>>* GST_DEBUG_DUMP_DOT_DIR=/home/hdb/proj ./execFile/basic_3	
>>>* 执行结束后,会在目录 **/home/hdb/proj** 中生成 **gstream-03.dot** 文件
>* 将 .dot文件转换成 .png文件
>>* dot -Tpng gstream-03.dot  > gst.png		

