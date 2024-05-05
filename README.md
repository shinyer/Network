KALI CVM云实例桌面版部署
前置要求：
首先一台服务器Centos的

创建数据盘挂载20GB 一个月也就7块

下载镜像：
挂载好云硬盘之后通过Wget下载kali Linux的镜像

因为官方镜像下载可能会出现下载问题，所以我用的腾讯源
下载地址：
https://mirrors.cloud.tencent.com/kali-images/kali-2024.1/kali-linux-2024.1-installer-amd64.iso

开始部署：
通过fdisk-l命令查看磁盘

镜像写入vdb这块数据盘 非 root 的情况下记得sudo执行以下命令
命令： 
dd bs=4M if=kali iso镜像名 of=/dev/vdb

设置超时时间vi /etc/default/grub
将GRUB_TIMEOUT设置为60

保存后执行grub2-mkconfig -o /boot/grub2/grub.cfg

执行完毕后 reboot进行重启！(现在开始就不用管ssh了)
然后登录某云控制台，进入网页 VNC ！

在GRUB 菜单按c键进入命令行模式


执行以下命令：
set root=(hd1)
chainloader +1
boot

boot之后进入 kali的安装界面，hd1对应的是我的vdb数据盘不是系统盘哈！跟着下图选就对了！


 

注意下图，选择继续

接着选择继续

然后选择 运行shell



手动挂载系统盘到 /cdrom 输入命令mount /dev/vdb1 /cdrom

接着输入exit退出，进入以下界面，选择检测并挂在安装介质 回车！






密码设置完成后进入以下界面！选择向导整个磁盘

注意下图！安装到系统盘 系统盘 系统盘 重要的事情说三遍！回车

放在同一分区 推荐新手使用 看图

结束并写入



然后下图
注意！ 直接选择安装GRUB启动引导器！回车

选择/dev/vda

慢慢慢~注意注意注意 下图 安装完成后 选择返回 返回 返回！

然后选择配置软件包管理


这里下图全选 空格键是选择 然后继续

然后进入一个漫长的过程 一个字 等！

一个小时...一个半小时...终于完了！那么现在进行下一步！
不要问 问就是默认，选择 gdm3

等

这里的安装完成 就是真的安装完成了！按回车吧~

安装完成后会重启，刷新一下这个网页，当你看到这个界面 那么恭喜你！

登录进去 我们打开终端(现在不是root权限哈 记得sudo哦)输入sudoapt-get install xrdp
输入y继续

然受输入sudoapt-get install xfce4
如果默认有 那就不管咯~

最重要的一步sudo vi /etc/xrdp/startwm.sh
在以下位置添加echo “xfce4-session” >~/.xsession
注意 一定不要输入错误了哈！

保存后继续设置root密码，输入sudo passwd root设置root密码sudo passwd root

Root密码设置完成后启动xrdpservice xrdp start
输入密码

设置开机启动sudo systemctl enable xrdp

本机打开mstsc 输入服务器ip进行连接

注意：这里用root用户登录！

结束。
以上是某讯云搭建，没别的，主要是不卡，其他自行测试！
