---
title: Emacs 使用 fcitx 输入法配置备忘
layout: post
categories: tools
---

习惯用 Emacs 用来写一些笔记，但五笔输入法( By Yuwen Dai )用起来不是很顺手，还是 fcitx 更好用一些。

在 debian8.2 ，英文 locale 下，作以下配置：

1. 安装 fcitx

   ```shell
   sudo apt-get install fcitx fcitx-frontend-all fcitx-config-gtk fcitx-table-all
   ```
	
2. 配置默认输入法 （使用 `im-config` 命令，并按提示进行）

3. 配置 fcitx（使用 `fcitx-config-gtk3` 命令，这里我习惯将输入法换出快捷键设置为 `Ctrl-\`）

4. 设置 emacs locale 环境变量:

   ```shell
   alias emacs="LC_CTYPE=zh_CN.UTF8 emacs"
   ```
	
5. 如果在 emacs 中可以调出 fcitx 输入框，但文字不能正确上屏，则需要确认是否包含zh_CN.UTF-8

   ```shell
   locale -a
   ```
	
如果没有，则可以使用 `sudo dpkg-reconfigure locales`，并选择之
	
至此，emacs 可以正常使用 fcitx 输入法。


