---
title: windows 安装小记
layout: post
---

今天很难得的不加班。忽然很怀念帝国时代II，可惜手头没有 windows 系统，安装之。

1. 机器使用我的一台 24G SSD 盘无风扇主机，之前安装 linuxmint
2. 插上 120GB 的笔记本退下来的硬盘，作双系统
3. 下载 win8pe 维护工具盘镜像（ISO），复制到 /boot 目录下
4. 安装 syslinux，提取 `/usr/lib/syslinux/memdisk` 到 /boot 目录下
5. 重启进入 grub 界面（启动时按 `esc` 键）
6. 指定 grub 加载 iso 镜像
   ```
   set root=(hd0,msdos1)
   linux16 /boot/memdisk iso raw
   initrd16 /boot/win8pe.iso
   boot
   ```
7. 引导入 winpe 界面，使用工具，如 ghost 等安装系统
8. 在 linux 中，使用 `grub-mkconfig` 生成配置文件，替换掉 `/boot/grub/grub.cfg` 配置
9. 重启，grub 菜单中选择进入 windows

之前没少折腾过硬盘安装系统和 U 盘维护工具，无论是 windows 还是 linux，这次安装 windows 相对顺利。还是认为精力不应该浪费把玩工具上，干想干的事，哪怕是娱乐也好。

AOE2, how do you turn this on?
