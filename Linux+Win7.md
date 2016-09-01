# System_tips
1 Linux+Win7安装方法（从Win7中安装Linux）
http://jingyan.baidu.com/article/c275f6bacc3326e33c756743.html

笔记本（win7旗舰版64位系统）
4G的U盘（装Linux的不用很大，装window的要大些）
预先准备软件：软碟通，EasyBCD
Linux系统（iso镜像文件），此次我用的是旗下的ubuntu12.04版本

一：进行磁盘分割，大概分出有30G左右的空间出来（我分割了大概40G）
详情请在我的另一篇经验上看：http://jingyan.baidu.com/article/a17d52853ff59e8098c8f2ae.html
1.注意：用来装双系统是不需要用建立空白卷！！！这样在装的时候就会显示是FreeSpace！！！
2.打不开网址的话，可以在百度经验下搜索“win7 旗舰版下如何分离磁盘空间与合并磁盘空间
”
2
二：制作U盘系统盘。具体如下：
1.插入U盘，打开软碟通，打开-选择下好的系统iso文件-启动-写入硬盘映像
2.然后如下图所示，“硬盘驱动器”选择你U盘所在的驱动器（比如我的是j盘），“写入方式”选择“USB-HDD+”
3.接着单击“格式化”，按默认即可（一般默认文件系统位FAT32）
4.紧跟着便捷启动，选择syslinux
5.最后单击“写入”
               In the end它会显示你写入成功！
三：U盘启动，进行安装即可

2 双系统安装完成后没有Win7启动项，需要再Linux中安装grub，然后修改grub.cfg进行reboot: 或者是直接在命令窗口sudo update-grub2，这样就能找到windows的引导程序了

win7基础上，硬盘安装cent OS 7.0 minimal 的iso镜像：


1. 安装界面分区时，手动分配/boot,/swap会有各种问题，有可能是安装的bug。直接使用自动分配，即可解决。


2. 安装cent OS后，启动界面win7启动项丢失。解决方案：

一： 

   a. 在linux的命令行下:  vi /boot/grub2/grub.cfg 

    b: Shift+G, o 启动编辑模式，在末尾加入如下命令: 

                   menuentry "win7 32bit" {

                   insmod chain

                   insmod ntfs

                   set root =(hd0,msdos1)      //这里的hd0,指的是第一块硬盘，msdos1指的是第一个分区。

                   chainloader +1

                    }

     c: ESC 退出编辑模式，输入:X 然后回车，保存

     d: reboot

二:

   a. 在linux的命令行下:  vi /boot/grub2/grub.cfg 

    b: Shift+G, o 启动编辑模式，在末尾加入如下命令: 

                   menuentry "win7 32bit" {

                   insmod chain

                   insmod ntfs

                   search -f /ntldr --set root

                   chainloader +1

                    }

     c: ESC 退出编辑模式，输入:X 然后回车，保存

     d: reboot

三: (猜可能有用)

在启动项界面，点击C进入编辑模式，在grub命令行下输入上述一和二中的中括号中的命令。应该有同样的效果。因为上述的代码也是在grub命令中执行的。比如，还可以在grub命令行下，输入 linux命令,如 ls，查看整个硬盘的分区情况。


再次重启时，在启动项上就有了win7 选项了。

四：（没试过）

menuentry "Windows7"

{

set root=(hd0,msdos1)

chainloader +1

boot

}



总之，系统启动和grub文件配置有密切关系。


PS: 硬盘安装linux时，有几点注意项：

1. FAT32在windows和linux上都能识别，可以作为放iso文件的格式。但是有一个问题，文件大小不能大于4G。所以只够放一个minimal 的iso，DVD的iso刚好超4G，不够放。所以，用网上的方法，把分区格式为ext3,这是window无法识别，但是linux可以识别的放大文件的格式。在windows上安装工具Ext2Fsd.exe后，可以在window上挂载ext3，可读可写了，但是每次重启必须都启动一次这个工具。所以在dos上找iso镜像时，实际还是找不到这个分区，应该还是windows不能识别ext3格式的问题。看网上应该还有其他引导方式。没空试了，以后再说。先用无桌面的minimal iso了。

2. 另外，在用工具EasyBCD配置grub时，加入了如下命令。

title Centos

#root (hd0,5)  这个命令没用到。

kernel /isolinux/vmlinuz linux repo=hd:/dev/sda6:/      

initrd /isolinux/initrd.img

其中，iso的文件夹isolinux要放到c盘下，还有其他一些可以参照硬盘安装cent OS教程。这里的sda6指的是第一个硬盘的第6个分区。这是在告诉系统到哪里找linux的iso镜像。注意点在linux下和实际用wingrub工具看到的编号可能不同。比如wingrub下，E盘编号是5，但是在这里用sda5就不行，启动时总提示找不到image。实际换到了sda6，就可以找到image了。然后到了Cent OS的安装界面上选择手动分区时发现linux下的分区编号确实和windows下不一样。这里的E盘确实是sda6了。所以才会有之前困扰一天的问题。另外，如果用u盘或第二个硬盘加载iso，相应的对应的位置应该是（hd1,0）和sdb1 等等的方式了。(hd1,0),表示第二个硬盘的第0个分区，而sda1表示第一个硬盘的第一个分区，sdb1表示第二个硬盘的第1个分区。这些以后再试。
