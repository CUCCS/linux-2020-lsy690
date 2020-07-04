# 实验一

## 1.启动已经安装好的Ubuntu Server（记得在此虚拟机未启动的时候设置该虚拟机的两块网络网卡：host-only和NAT）
![](网卡1.png)
![](网卡2.png)

## 2.在Windows环境下下载putty

## 3.在步骤2启动的虚拟机命令行输入命令 ifconfig -a 查看网卡状态
![](网卡状态未打开.png)

### 此时发现网卡状态未打开，手动手动启动并动态获取IP，使用命令
sudo ifconfig enp0s8 up     sudo dhclient enp0s8 打开
![](网卡已打开.png)

### 此时发现网卡已经打开，并知道IP为192.168.56.101

#### 在坚定网卡是否打开的时候，看错了IP位置，导致了putty连接不上的一些问题

## 4.使用刚才下载的putty连接这台虚拟机

### 一开始不知道怎么打开putty，在百度和知乎的帮助下才把putty和虚拟机连接起来
![](putty连接虚拟机.png)
#### 连接虚拟机遇到的问题
![](putty连接虚拟机的失败.png)
一开始是因为IP出错导致错误，改了之后发现拒绝连接，最后在百度上了解到了是我没有安装ssh-server,之后在虚拟机中输入命令：sudo apt-get install openssh-server,问题解决！

### 此时可以看到putty已经连接到了虚拟机

## 5.把用于ubuntu16.04.1镜像文件从Windows复制进虚拟机，可以使用psftp
cd /home/cuc
put ubuntu16.04.01
这里有两个要注意的点
（1）我们的镜像文件需要放在psftp目录下
（2）如果无法识别这个镜像文件，可以改名为：1.iso

## 7..回到putty登录的虚拟机命令行

# 在当前用户目录下（/home/cuc）创建一个用于挂载iso镜像文件的目录
mkdir loopdir


# 挂载iso镜像文件到该目录
mount -o loop ubuntu-16.04.1-server-amd64.iso loopdir
![](挂载iso到目录.png)

# 创建一个工作目录用于克隆光盘内容
mkdir cd
 
# 同步光盘内容到目标工作目录
# 一定要注意loopdir后的这个/，cd后面不能有/
rsync -av loopdir/ cd

![](同步iso内容.png)
# 卸载iso镜像
umount loopdir


# 进入目标工作目录
cd cd/


# 编辑Ubuntu安装引导界面增加一个新菜单项入口
vim isolinux/txt.cfg

添加以下内容到该文件后强制保存退出

label autoinstall
  menu label ^Auto Install Ubuntu Server
  kernel /install/vmlinuz
  append  file=/cdrom/preseed/ubuntu-server-autoinstall.seed debian-installer/locale=en_US console-setup/layoutcode=us keyboard-configuration/layoutcode=us console-setup/ask_detect=false localechooser/translation/warn-light=true localechooser/translation/warn-severe=true initrd=/install/initrd.gz root=/dev/ram rw quiet


提前阅读并编辑定制Ubuntu官方提供的示例preseed.cfg，并将该文件保存到刚才创建的工作目录/home/cuc/cd/preseed/ubuntu-server-autoinstall.seed
修改isolinux/isolinux.cfg，增加内容timeout 10（可选，否则需要手动按下ENTER启动安装界面）
 
# 重新生成md5sum.txt

sudo su -
cd /home/cuc/cd && find . -type f -print0 | xargs -0 md5sum > md5sum.txt


# 封闭改动后的目录到.iso
IMAGE=custom.iso
BUILD=/home/cuc/cd/


mkisofs -r -V "Custom Ubuntu Install CD" \
            -cache-inodes \
            -J -l -b isolinux/isolinux.bin \
            -c isolinux/boot.cat -no-emul-boot \
            -boot-load-size 4 -boot-info-table \
            -o $IMAGE $BUILD

### 报错，查了百度，知道了genisoimage未安装，于是用下面两条指令安装了genisoimage

apt-get update

apt-get install genisoimage

### 安装过程和安装完毕后如下图
![](安装过程.png)

![](安装完毕.png)
这样在虚拟机（/home/cuc/cd/）这个目录下就会出现custom.iso这个镜像，使用命令

mv custom.iso ../

然后打开psftp窗口

get custom.iso

从虚拟机中将custom.iso这个镜像文件复制出来，这个文件就是我们需要的无人值Linux文件，打开VirtualBox 从custom.iso镜像中安装系统。

## 总结：在安装无人值守Linux安装镜像时，遇到了很多问题，同时也扩宽了自己的知识域。在putty连接虚拟机时，有IP错误，知道了IP的位置，也有拒绝连接的，知道了自己需要安装ssh-server。实验的结果并不是很理想，并没有很顺利的完成，但至少也从中学到了一点知识，也知道了大部分问题都可以自己百度解决。