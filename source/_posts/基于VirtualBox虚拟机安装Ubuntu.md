---
title: 基于VirtualBox虚拟机安装Ubuntu
copyright: true
categories: 
  - Linux
tags: 
  - VirtualBox
  - Ubuntu

---
<blockquote class="blockquote-center">Variety is the spice of life.
![](/myimages/21.jpg)

<!-- more -->

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=320 height=86 src="//music.163.com/outchain/player?type=2&id=1873672&auto=1&height=66"></iframe>

---
# 安装VirtualBox #
1. [VirtualBox官网](https://www.virtualbox.org/wiki/Downloads/)下载安装之，默认设置即可
2. 运行VirtualBox,Ctrl+G打开全局设定，自行设置默认虚拟电脑位置、语言等选项
3. 同样在官网下载安装VirtualBox扩展包，在全局设定的“扩展”选项中添加扩展包，扩展包名称为`Oracle VM VirtualBox Extension Pack`

	- 扩展包用于支持USB 2.0和USB 3.0设备，VirtualBox RDP，磁盘加密，NVMe和PXE启动英特尔卡
	- 一般安装的扩展包版本应该与VirtualBox版本相同

----------
# 安装Ubuntu #
## 创建虚拟机 ##
1. 运行VirtualBox，新建一个虚拟机

	- 虚拟机名称随意
	- 操作系统类型选择Linux
	- 版本选择Ubuntu（64-bit）
2. 设置内存大小

	- 此内存为虚拟机所占的系统内存
	- 一般设置不超过主机系统内存的1/2
3. 创建虚拟硬盘

	- 按照提示选择“现在创建虚拟硬盘”
	- 虚拟硬盘文件类型选择为默认的“VDI(VirtualBox 磁盘映像)”
	- 选择“动态分配”的虚拟硬盘分配方式
	- 设置虚拟硬盘文件的名称、位置和虚拟硬盘的大小
4. 点击“创建”即创建了一台虚拟机，相当于一个没有安装操作系统的电脑主机

## 在虚拟机上安装Ubuntu操作系统 ##
1. [Ubuntu官网](https://www.ubuntu.com/download/desktop/)下载桌面版系统“Ubuntu Desktop”，点击ReleaseNotes，选择Ubuntu Desktop and Server类型的版本，当前最新版本为`Ubuntu 18.04.2 LTS`
2. 在VirtualBox中启动创建好的虚拟机，弹出“选择启动盘”对话框，选择下载好的虚拟光盘`ubuntu-18.04.2-desktop-amd64.iso`，点击“启动”即可进入操作系统安装界面
3. 按照提示设置安装Ubuntu，可能持续几十分钟
4. 安装完毕后，重启Ubuntu

## 安装VirtualBox增强功能 ##
安装VirtualBox增强功能可以实现：共享文件夹、粘贴板以及鼠标无缝移动等。
安装步骤：

	- 进入Ubuntu操作系统，在上方“设备”选项卡中选择“安装增强功能”，此时桌面出现一个光盘图标
	- Ctrl+Alt+T打开终端，逐步输入如下命令
    	- cd /media/
    	- ls
    	- cd 上面一行的内容（是你设置的虚拟硬盘的名称）
    	- ls
    	- cd 上面一行的内容（VBOXADDITIONS_版本号）
    	- ls
    	- sudo ./VBoxLinuxAddionions.run
	- 按照提示输入安装Ubuntu系统时设置的密码回车即可（密码输入是隐式的）

# 配置Ubuntu #
## 设置服务器镜像源 ##

	目的：将默认的服务器更改为获取资源最快的国内服务器
    	- 打开“软件和更新”
    	- 选择下载服务器
    	- 选择最佳服务器，授权
    	- 关闭“软件和更新”，重新载入可用软件包列表

## apt终端命令 ##
apt（Advanced Packaging Tool）是Ubuntu的安装包管理工具。

1. 更新可用软件包列表

		sudo apt-get update
2. 更新已安装的包

		sudo apt-get upgrade
3. 安装软件

		sudo apt install XXX

		安装Google浏览器：
		- 从https://www.google.com/chrome/?platform=linux下载安装文件
		- sudo apt install libappindicator1 libindicator7
		- sudo dpkg -i XXX.deb
		- sudo apt -f install
		
		安装搜狗输入法：
		- 在系统设置的语言支持中将“键盘输入法系统”修改为fcitx
		- 访问http://pinyin.sougou.com/linux/下载安装文件
		- 在终端输入 sudo dpkg -i XXX.deb; sudo apt -f install
4. 卸载软件

		sudo apt remove XXX
5. 查找软件

		apt-cache search XXX

## deb格式的安装包 ##
deb是Debian Linux的安装格式，在Ubuntu中同样可以使用。

	sudo dpkg -i XXX.deb

