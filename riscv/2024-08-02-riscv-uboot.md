---
layout: post
title: riscv uboot compile
date: 2024-08-02 16:40:35
description:
tags: riscv
categories: riscv
---

构建 uboot

## 版本

ubuntu 20.04

## 从源码编译

### 安装依赖

```bash
apt-get install python3-venv -y
apt-get install python3-pip -y


python3 -m pip install sphinx -y
python3 -m pip install sphinx_rtd_theme -y
python3 -m pip install ninja -y

```

### 下载源码

```bash
# 以 qemu 9.0 版本为例
wget https://mirrors.aliyun.com/blfs/conglomeration/qemu/qemu-9.0.0.tar.xz

tar -xvJf qemu-9.0.0.tar.xz

```

### 编译

```bash

cd qemu-9.0.0
# ../configure
./configure --enable-kvm --enable-virtfs --enable-slirp --target-list=riscv64-linux-user,riscv64-softmmu

make -j 15

```
