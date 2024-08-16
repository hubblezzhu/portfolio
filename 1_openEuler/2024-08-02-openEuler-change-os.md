---
layout: post
title: openEuler change os
date: 2024-08-02 16:40:16
description:
tags: openEuler
categories: openEuler
---

多内核版本下，命令行修改启动OS

## 版本

Build OS: openEuler 20.03

## 查看现有的可用的内核

```shell
cat /boot/efi/EFI/openEuler/grub.cfg  | grep menuentry
```

```shell
menuentry 'openEuler (4.19.90) 20.03 (LTS-SP1)' --class openeuler --class gnu-linux --class gnu --class os --unrestricted $menuentry_id_option 'gnulinux-4.19.90-2109.1.0.0108.oe1.x86_64-advanced-21c67e53-7db1-431b-860b-4b1fc663d9c2' {
menuentry 'openEuler (4.19.90-2109.1.0.0108.oe1.x86_64) 20.03 (LTS-SP1)' --class openeuler --class gnu-linux --class gnu --class os --unrestricted $menuentry_id_option 'gnulinux-4.19.90-2109.1.0.0108.oe1.x86_64-advanced-21c67e53-7db1-431b-860b-4b1fc663d9c2' {
menuentry 'openEuler (0-rescue-c02df42ec66148379954b32190252324) 20.03 (LTS-SP1)' --class openeuler --class gnu-linux --class gnu --class os --unrestricted $menuentry_id_option 'gnulinux-0-rescue-c02df42ec66148379954b32190252324-advanced-21c67e53-7db1-431b-860b-4b1fc663d9c2' {
menuentry 'System setup' $menuentry_id_option 'uefi-firmware' {
```

## 修改启动os

```shell
grub2-set-default 'openEuler (4.19.90) 20.03 (LTS-SP1)'
```

## 重启

```shell
reboot
```
