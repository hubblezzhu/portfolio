---
layout: post
title: openEuler kernel compile
date: 2024-08-02 16:40:18
description:
tags: openEuler
categories: openEuler
---

基于 openEuler 编译 linux kernel

## 版本

Build OS: openEuler 23.03 x86 \
Linux Source: linux 6.1.19

## 下载源码

```shell
dnf install kernel-source
cd /usr/src/linux-6.1.19-7.0.0.17.oe2303.x86_64/
```

或者

```shell
wget https://mirror.bjtu.edu.cn/kernel/linux/kernel/v6.x/linux-6.1.19.tar.gz
tar -zxvf linux-6.1.19.tar.gz
```

## 安装依赖

```shell
dnf install make gcc bison flex -y
dnf install elfutils-devel -y
dnf install openssl-devel -y
dnf install ncurses-devel -y
dnf install dwarves -y
dnf install git -y
dnf install bc -y
```

## 修改编译配置项

使用 openEuler 23.03 原有config

```shell
cp /boot/config-6.1.19-7.0.0.17.oe2303.x86_64 .config
```

按需微调

```shell
make menuconfig
```

## 编译 & 安装

```shell
make -j 3
```

编译成功

```bash
root@localhost.localdomain:/usr/src/linux-6.1.19-7.0.0.17.oe2303.x86_64 git:(master) make -j 3
  DESCEND objtool
  DESCEND bpf/resolve_btfids
  CALL    scripts/checksyscalls.sh
  TEST    posttest
arch/x86/tools/insn_decoder_test: success: Decoded and checked 5752124 instructions
  TEST    posttest
arch/x86/tools/insn_sanity: Success: decoded and checked 1000000 random instructions with 0 errors (seed:0x84d49b4f)
Kernel: arch/x86/boot/bzImage is ready  (#4)

```

```shell
make modules_install
make install

reboot
```

reboot 后在系统启动选择系统界面就可以选择新编的内核了

## 卸载

```bash
rm -rf  /boot/vmlinuz-5.15.122
rm -rf  /boot/initrd.img-5.15.122
rm -rf  /boot/System-map-5.15.122
rm -rf  /boot/config-5.15.122
rm -rf  /lib/modules/5.15.122
rm -rf  /var/lib/initramfs-tools/5.15.122

update-grub
```
