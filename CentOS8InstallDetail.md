## CentOS 8 装机前准备
备份data projects
备份设置，包括
- IP设置 
- sublime配置文件 
- 坚果云盘 
- zotero配置
## CentOS 8 装机过程
- 第一个问题 设置boot为uefi 才能识别U盘
- 第二个问题 安装报错 似乎centos7也遇到过 在第一个列表输入e 修改安装代码 https://www.cnblogs.com/yanjieli/p/11905638.html 主要在hd:LABEL=***/dev/sda4***修改
- 

## CentOS 装机后配置顺序

#### CentOS 镜像使用帮助
https://mirrors.tuna.tsinghua.edu.cn/help/centos/
#### ELRepo 镜像使用帮助
https://mirrors.tuna.tsinghua.edu.cn/help/elrepo/
```
#编辑 /etc/yum.repos.d/elrepo.repo 文件，在 mirrorlist= 开头的行前面加 # 注释掉；
#并将 elrepo.org/linux 替换为 mirrors.tuna.tsinghua.edu.cn/elrepo。
sed -e 's!^mirrorlist=!#mirrorlist=!g' \
    -e 's!elrepo\.org/linux!mirrors.tuna.tsinghua.edu.cn/elrepo!g' \
    -e 's!http://mirrors\.tuna!https://mirrors.tuna!g' \
    -i /etc/yum.repos.d/elrepo.repo
```
#### EPEL 镜像使用帮助
https://mirrors.tuna.tsinghua.edu.cn/help/epel/
```
# 当前tuna已经在epel的官方镜像列表里，所以不需要其他配置，mirrorlist机制就能让你的服务器就近使用tuna的镜像。
# 如果你想强制 你的服务器使用tuna的镜像，可以修改/etc/yum.repos.d/epel.repo，将mirrorlist和metalink开头的行注释掉。
# 接下来，取消注释这个文件里baseurl开头的行，并将其中的http://download.fedoraproject.org/pub替换成https://mirrors.tuna.tsinghua.edu.cn

sed -e 's!^metalink=!#metalink=!g' \
    -e 's!^#baseurl=!baseurl=!g' \
    -e 's!//download\.fedoraproject\.org/pub!//mirrors.tuna.tsinghua.edu.cn!g' \
    -e 's!http://mirrors\.tuna!https://mirrors.tuna!g' \
    -i /etc/yum.repos.d/epel.repo /etc/yum.repos.d/epel-testing.repo
```

#### Centos添加用户进入组而不离开当前组
```
usermod -a -G groupA user
```
#### CentOS 8 Chrome安装
直接用rpm包安装 下载好最新版U盘拷贝
or
```
sudo yum localinstall 
```
#### CentOS 中文输入法
```
sudo yum install -y ibus-libpinyin
```
#### CentOS 8 sublime-text 安装
https://blog.csdn.net/alinathz/article/details/106444638
sublime-text3 安装
```
#首先导入Sublime Text 3 GPG密钥：

sudo rpm -v --import https://download.sublimetext.com/sublimehq-rpm-pub.gpg

#导入后，将稳定的通道存储库添加到RHEL 8/CentOS 8：

sudo wget -P /etc/yum.repos.d/ https://download.sublimetext.com/rpm/stable/x86_64/sublime-text.repo

#在CentOS 8/RHEL 8上安装Sublime Text 3 请运行以下命令安装：

sudo dnf install sublime-text
```
#### Centos 8 安装R
一定要注意PowerTools的激活 否则会找不到R-devel
https://cloud.tencent.com/developer/article/1626639
https://www.liujason.com/article/531.html
```
#参考清华镜像安装epel库
sudo yum install epel-release
#然后配置好 参考前文
#下一步很关键
sudo dnf config-manager --set-enabled PowerTools
sudo yum install R
```
会安装大量软件 小心bug 耐心等待


#### Centos 8 安装Rstudio
```
sudo yum localinstall path_to_Rstudio_rpm
```

#### CentOS 8 安装anaconda
https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/
``` 
conda config --set auto_activate_base false 
```

#### CentOS 8 配置sublime-text 3 中文输入 补全
安装Kite 补全神器
```
bash -c "$(wget -q -O - https://linux.kite.com/dls/linux/current)"
```
sublime-text3 配置从无到有很恶心 如果以前配置过就很好。

主要有
- conda 环境管理
- anaconda 编译
- sublimeREPL 页面交互
- 等等

首先安装package control

关于conda配置sublimetext3虚拟环境
##### Anaconda turns your Sublime Text 3 into a full featured Python development IDE
- https://damnwidget.github.io/anaconda/anaconda_settings/ 推荐每个project使用一种环境
- 在每个project下面有两个类型的文档
- *未完待续*

#### R包安装和管理
source edit:

首先在tools下面的global options里面修改packages的primary Cran repository，要等待一小会儿才会出现全部列表，测试合肥中科大的可以，也可以选择清华的。


```
#在Rstudio中
install.packages("BiocManager")
BiocManager::install(c("WGCNA ", "DOSE","org.Hs.eg.db","topGO"))
BiocManager::install(c("ggplot2", "WGCNA", "clusterProfiler", "DOSE", "org.Hs.eg.db", "topGO", "STRINGdb", "GEOquery","hugene10sttranscriptcluster.db", "magrittr", "Biobase", "limma", "fields", "gplots", "dplyr"))
```
KEGG缺xml2 Rcurl 
```
sudo yum install libxml2-devel
# 其他包需要 保证R包的安装 一百多兆
sudo yum install make gcc gcc-c++ libcurl-devel libxml2-devel openssl-devel texlive-* libjpeg-turbo-devel
postgresql-devel
```
Rstudio闪退
可能是因为显卡不匹配

**安装显卡驱动**

https://blog.csdn.net/edrlyh/article/details/105117572?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param
```
nvidia-detect -v
# 提示没有的话 centos8 会给你装一 输入yes 但其实安装的不是我要的内核 不应该选yes
# Probing for supported NVIDIA devices...
# [10de:1c30] NVIDIA Corporation GP106GL [Quadro P2000]
# This device requires the current 440.64 NVIDIA driver kmod-nvidia
# WARNING: Xorg log file /var/log/Xorg.0.log does not exist
# WARNING: Unable to determine Xorg ABI compatibility
# WARNING: The driver for this device does not support the current Xorg version
sudo yum install kernal-devel 
sudo yum install elfutils-libelf-devel
yum list | grep kernel #查系统当前内核版本

# 下载驱动
wget https://cn.download.nvidia.cn/XFree86/Linux-x86_64/418.88/NVIDIA-Linux-x86_64-418.88.run
#修改权限
chmod a+x ./NVIDIA-Linux-x86_64-418.88.run
#运行
./NVIDIA-Linux-x86_64-418.88.run

# 刚好我的内核跟这个博客一样 安装如下报错了
# 1.ERROR: You appear to be running an X server; 
# 表示你不能在图形界面下安装驱动,解决办法:
#修改系统启动模式,不再默认进入图形界面,而是进入字符界面!!注意!!
# CentOS 8不再支持修改/etc/inittab来定义启动模式了,请使用以下命令进行操作
systemctl get-default  
# -->这个命令表示查看当前启动模式,可以忽略

systemctl set-default multi-user.target  
# -->这个命令表示设置成启动进入字符模式

reboot now
# 这样重启后就进入字符模式了,完事后可以用以下命令修改启动模式.
systemctl set-default graphical.taget

# 2.ERROR: The Nouveau kernel driver is currently in use by your system.

# 表示系统已经安装了Nouveau的显卡驱动,需要禁用或者干掉这个驱动

# A.编辑/etc/modprobe.d/blacklist.conf
sudo vi /etc/modprobe.d/blacklist.conf

# 这个文件可能是全新的,不要管它,直接加入
blacklist nouveau

lsmod|grep nouveau 
#无输出表示禁用成功

## 我没有操作下面命令
# B.运行命令备份与重建initramfs
# mv /boot/initramfs-$(uname -r).img # /boot/initramfs-$(uname -r).img.bak

# dracut -v /boot/initramfs-$(uname -r).img $(uname -r)
# 重启系统
#reboot now
#你会发现字符都变大了,不过没关系,驱动安装完之后就正常了.
```

#### 文献管理软件 Zotero和坚果云

#### Libreoffice软件 PDF软件
从官网的镜像下载目前版本
https://mirrors.cloud.tencent.com/libreoffice/libreoffice/stable/7.0.2/rpm/x86_64/

官方中文帮助
https://zh-cn.libreoffice.org/get-help/install-howto/linux/
```
LibreOffice_7.0.2_Linux_x86-64_rpm.tar.gz
LibreOffice_7.0.2_Linux_x86-64_rpm_sdk.tar.gz
LibreOffice_7.0.2_Linux_x86-64_rpm_langpack_zh-CN.tar.gz
LibreOffice_7.0.2_Linux_x86-64_rpm_helppack_zh-CN.tar.gz
```
安装代码
```
# 建立安装文件 解压缩
sudo mkdir /usr/libreoffice

sudo tar -zxvf ~/Downloads/LibreOffice_7.0.2_Linux_x86-64_rpm.tar.gz -C /usr/libreoffice
sudo tar -zxvf ~/Downloads/LibreOffice_7.0.2_Linux_x86-64_rpm_sdk.tar.gz -C /usr/libreoffice
sudo tar -zxvf ~/Downloads/LibreOffice_7.0.2_Linux_x86-64_rpm_langpack_zh-CN.tar.gz -C /usr/libreoffice
sudo tar -zxvf ~/Downloads/LibreOffice_7.0.2_Linux_x86-64_rpm_helppack_zh-CN.tar.gz -C /usr/libreoffice
# 本地安装
sudo yum install ./LibreOffice_7.0.2.2_Linux_x86-64_rpm/RPMS/*.rpm
sudo yum install ./LibreOffice_7.0.2.2_Linux_x86-64_rpm_sdk/RPMS/*.rpm
sudo yum install ./LibreOffice_7.0.2.2_Linux_x86-64_rpm_langpack_zh-CN/RPMS/*.rpm
sudo yum install ./LibreOffice_7.0.2.2_Linux_x86-64_rpm_helppack_zh-CN/RPMS/*.rpm

#complete！
```
#### Markdown中的数学公式生成
inline math:

`$c = \sqrt{a^{2}+b_{xy}^{2}+e^{x}} $`

Math Block:
```math
C=\sqrt{a^2+b_{xy}^2+e^x}
```

#### Anaconda 卸载

- 首先
``` sudo rm -rf path/anaconda3 ```，然后切记要修改```~/.bashrc```将其中ANACONDA
相关的行删除。例如 ```# added by Anaconda2 installer```后的所有内容。


#### 如何挂载exfat格式硬盘
https://blog.csdn.net/u013948852/article/details/99849262
```
sudo yum install -y http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm
sudo yum install exfat-utils fuse-exfat
sudo fdisk -l
sudo mkdir /mnt/usb_exfat
sudo mount.exfat-fuse /dev/sdc2 /mnt/usb_exfat/
```

#### Anaconda CentOS 安装
https://zhuanlan.zhihu.com/p/64930395

默认conda init那里是no 对于新手最好还是填yes.

如果开机自动启动base环境可以进行以下设置更改.
```
conda config --set auto_activate_base false
```
选yes后环境变量设置完成，如果要看效果，需要关闭当前Terminal，重新打开一个，即可看到base环境启动。
如果没有，```conda activate ```试试看。

#### Anaconda 介绍、安装、使用及常用命令（macOS  Win10）
https://www.jianshu.com/p/f456f414bb3b

测试后 Centos linux可用

#### Anaconda 环境配置
##### 新建环境
``` conda -n 环境名 包名```

Example:  ``` conda -n myenv numpy```
##### 环境中安装包
见下段常用命令

##### 环境的设置
参考 https://blog.csdn.net/knidly/article/details/80327166
```
# 创建一个名为python34的环境，指定Python版本是3.4（不用管是3.4.x，conda会为我们自动寻找3.4.x中的最新版本）

conda create --name python34 python=3.4

 

# 安装好后，使用activate激活某个环境

activate python34 # for Windows

source activate python34 # for Linux & Mac

# 激活后，会发现terminal输入的地方多了python34的字样，实际上，此时系统做的事情就是把默认2.7环境从PATH中去除，再把3.4对应的命令加入PATH

 

# 此时，再次输入

python --version

# 可以得到`Python 3.4.5 :: Anaconda 4.1.1 (64-bit)`，即系统已经切换到了3.4的环境

 

# 如果想返回默认的python 2.7环境，运行

deactivate python34 # for Windows

source deactivate python34 # for Linux & Mac

#这里注意：新版本anaconda 废弃了source语句 现在
#激活环境用
conda activate 环境名
#退出用
conda deactivate 环境名

# 删除一个已有的环境

conda remove --name python34 --all
```
#### 启动Python IDE Spyder：

    在命令窗口直接输入spyder，系统会自动启动spyder3。

#### 常用的conda命令：

    1）、查看当前环境下已安装的包：conda list

    2）、查找package信息：conda search XXXX   （XXXX为你要查找的包名称）

    3）、安装package ：conda install -n XXXX（环境名称） XXXX（要安装的package名称）

                   # 如果不用-n指定环境名称，则被安装在当前活跃环境

                   # 也可以通过-c指定通过某个channel安装

    4）、更新package ：conda update -n XXXX（环境名称） XXXX（要更新的package名称）

    5）、删除package ：conda remove -n XXXX（环境名称） XXXX（要更新的package名称）
    6) 、查看已有环境 ：conda info -e

#### 环境变量设置
https://www.cnblogs.com/youyoui/p/10680329.html
#### JupyterLab 安装与配置
https://github.com/selfteaching/the-craft-of-selfteaching/blob/master/T-appendix.jupyter-installation-and-setup.ipynb

