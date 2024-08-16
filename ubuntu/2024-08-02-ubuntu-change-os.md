---
layout: post
title: ubuntu change os
date: 2024-08-02 16:40:40
description:
tags: ubuntu
categories: ubuntu
---

多内核版本下，命令行修改启动OS

## 版本

Build OS: ubuntu 22.04 LTS x86

## 查看现有的可用的内核

```shell
cat /boot/grub/grub.cfg  | grep menuentry
```

```shell
menuentry 'Ubuntu' --class ubuntu --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-simple-18342a49-b884-4c12-8afc-b40d82e17ac3' {
submenu 'Advanced options for Ubuntu' $menuentry_id_option 'gnulinux-advanced-18342a49-b884-4c12-8afc-b40d82e17ac3' {
	menuentry 'Ubuntu, with Linux 5.15.0-56-generic' --class ubuntu --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-5.15.0-56-generic-advanced-18342a49-b884-4c12-8afc-b40d82e17ac3' {
	menuentry 'Ubuntu, with Linux 5.15.0-56-generic (recovery mode)' --class ubuntu --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-5.15.0-56-generic-recovery-18342a49-b884-4c12-8afc-b40d82e17ac3' {
	menuentry 'Ubuntu, with Linux 5.15.0-41-generic' --class ubuntu --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-5.15.0-41-generic-advanced-18342a49-b884-4c12-8afc-b40d82e17ac3' {
	menuentry 'Ubuntu, with Linux 5.15.0-41-generic (recovery mode)' --class ubuntu --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-5.15.0-41-generic-recovery-18342a49-b884-4c12-8afc-b40d82e17ac3' {
menuentry 'UEFI Firmware Settings' $menuentry_id_option 'uefi-firmware' {

```

## 修改启动os

```shell
vi /etc/default/grub


GRUB_DEFAULT=0
GRUB_TIMEOUT_STYLE=hidden
GRUB_TIMEOUT=0
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
GRUB_CMDLINE_LINUX_DEFAULT=""
GRUB_CMDLINE_LINUX=""

```

以 Ubuntu, with Linux 5.15.0-56-generic 为例 \
将 GRUB_DEFAULT 修改为

```shell
GRUB_DEFAULT="Advanced options for Ubuntu>Ubuntu, with Linux 5.15.0-56-generic"
```

更新 grub 配置

```shell
update-grub
```

## 重启

```shell
reboot
```
