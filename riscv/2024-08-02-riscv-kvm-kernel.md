---
layout: post
title: riscv kernel compile
date: 2024-08-02 16:40:30
description:
tags: riscv
categories: riscv
---

编译安装 kvm risc-v 源码

## 版本

ubuntu 20.04

## 从源码编译

### 安装依赖

```bash
apt-get install bc -y
apt-get install tar -y
apt-get install libelf-dev gcc make -y
apt-get install libssl-dev dwarves libncurses-dev bison flex -y
```

### 下载源码

```bash
git clone git@github.com:kvm-riscv/linux.git kvm_riscv_linux
```

### 编译

```bash
cd kvm_riscv_linux

export ARCH=riscv
export CROSS_COMPILE=/root/workspace/riscv/bin/riscv64-unknown-linux-gnu-

# mkdir build-riscv64

# make -C linux O=`pwd`/build-riscv64 defconfig
# make -C linux O=`pwd`/build-riscv64 -j 3

make defconfig
make -j 15
```
