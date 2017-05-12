---
title: Archlinux 安装备忘
layout: post
---

# 安装过程

重新安装 Archlinux( 2016.06.01 )，记录安装过程以作备忘。

## 准备安装环境

1. grub2
   
	需要通过 grub 来引导安装过程。之前安装的 debian，已经有 grub 环境，跳过这一步。在 windows 环境下可以安装 [easyBCD][easybcd]
	
2. 下载最新 Archlinux 完整镜像：

	https://www.archlinux.org/download/
	
3. 备份当前系统数据

## 引导安装过程

重启进入 grub2 界面，按 `c` 进入命令输入窗口，输入以下命令（注意将 isofile 和 /dev/sda5 替换为自己机器的路程，前者为 iso 文件在分区下的路径，/dev/sda5 为存放 iso 文件的块文件路径）

```grub
set root=(hd0,msdos5)
isofile=/Download/archlinux-2016.06.01-dual.iso
linux (loop)/arch/boot/x86_64/vmlinuz img_dev=/dev/sda5 img_loop=$isofile earlymodules=loop
initrd (loop)/arch/boot/x86_64/arch_iso.img
boot
```

如果正常，则进入 archlinux iso 系统环境

## 安装系统

可以对照安装系统环境下 `~/install.txt` 文件进行，主要进行以下操作：

1. 格式化分区
2. 连接网络（无线网络可以使用 `wifi-menu` 命令进行配置）
3. 挂载要安装系统的分区到 `/mnt` 下
4. 安装基本系统： `pacstrap /mnt base`
5. 编辑 `/etc/pacman.d/mirrorlist`，配置软件源，建议使用 `mirrors.aliyun.com/archlinux` 源
5. 生成 fstab 文件： `genfstab -p /mnt >> /mnt/etc/fstab`
6. chroot 到新系统下： `arch-chroot /mnt`
   - 安装其它软件（特别注意应该安装网络相关的包，如 wireless-tools、iw 等）
   - 设置时区（将 /usr/share/zoneinfo/zone 下相应的时区链接到 `/etc/localtime` 文件）
   - 配置语言支持，编辑选中 `/etc/locale.gen` 文件中相应语言，并使用命令 `locale-gen` 生成
   - 设置 root 密码
   - 安装 grub 并配置（使用 `grub-install`）， **再次确认 `/boot/grub/grub.cfg` 配置**
7. 退出 chroot，并重启机器

正常情况，重启后 grub 将引导系统进入

## 安装后配置

* 添加普通用户
* 安装图形化界面，习惯使用 xfce4：

  ```bash
  pacman -S sudo xorg xorg-apps xorg-server xf86-video-intel lightdm-gtk-greeter xfce4 xfce4-goodies chromium fcitx-im fcitx-configtool
  ```
	
* 启动图形界面

  ```bash
  systemctl enable lightdm; systemctl start lightdm
  ```
	
* 安装中文字体和编辑器

  ```bash
  pacman -S community/wqy-microhei community/wqy-zenhei community/wqy-microhei-lite community/wqy-bitmapfont noto-fonts-clj vim emacs
  ```

* 安装基本开发环境

  ```shell
  pacman -S base-devel
  ```
	
# 后记

到这里系统基本安装完成，网络比较好的情况下半小时搞定整个过程。后续继续调整图形界面和安装软件，可以投入使用。

这里还要反省一下自己，不要过度的挑剔和依赖于工具。没有最好的工具，或者说总有更好的工具，重要的是我们能用它们作些什么。

希望这次重装系统能多用些日子 :-)


	
[easybcd]: http://neosmart.net/EasyBCD/
