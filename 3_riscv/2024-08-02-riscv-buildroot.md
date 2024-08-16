---
layout: post
title: riscv build rootfs by buildroot
date: 2024-08-02 16:40:29
description:
tags: riscv buildroot
categories: riscv
---

编译构建 buildroot

## 版本

ubuntu 20.04

## 从源码编译

### 安装依赖

```bash
apt-get install unzip -y
```

### 下载源码

```bash
git clone https://github.com/buildroot/buildroot.git

```

### 配置选项

```bash
cd buildroot
make menuconfig
```

#### 1、选择 RISC-V 架构

> **Target options ---> Target Architecture （i386）---> (x) RISCV**

#### 2、选择 ext 文件系统

> **Filesystem images ---> [*] ext2/3/4 root filesystem**

下方的exact size可以调整ext文件系统大小配置，默认为60M，这里需要调整到500M以上，因为需要编译qemu文件进去

#### 3、配置 glibc

在可视化页面按 `/` 即可进入搜索模式，在搜索模式分别输入下面参数，然后打开对应的配置

> **BR2_TOOLCHAIN_BUILDROOT_GLIBC**

#### 4、配置 qemu

在可视化页面按 `/` 即可进入搜索模式，在搜索模式分别输入下面参数，然后打开对应的配置

> **BR2_USE_WCHAR** BR2_USE_WCHAR=y => BR2_TOOLCHAIN_USES_GLIBC
> **BR2_PACKAGE_QEMU** > **BR2_TARGET_ROOTFS_CPIO** > **BR2_TARGET_ROOTFS_CPIO_GZIP**

### 编译

```bash
make -j 15
```

结果文件

```bash
root@isrc:~/riscv/buildroot# ll output/images/
total 2347252
-rw-r--r-- 1 root root  825046016 May 16 08:42 rootfs.cpio
-rw-r--r-- 1 root root  175294696 May 16 08:43 rootfs.cpio.gz
-rw-r--r-- 1 root root 1073741824 May 16 08:43 rootfs.ext2
-rw-r--r-- 1 root root  826019840 May 16 08:43 rootfs.tar
```
