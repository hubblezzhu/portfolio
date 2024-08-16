---
layout: post
title: riscv install cross compile toolchain
date: 2024-08-02 16:40:34
description:
tags: riscv
categories: riscv
---

编译安装 RISC-V 交叉编译工具链

## 版本

ubuntu 20.04

## 方式一：从 apt 源安装

```bash
apt-get install git -y
apt-get install build-essential -y
apt-get install gdb-multiarch -y
apt-get install gcc-riscv64-linux-gnu -y
apt-get install binutils-riscv64-linux-gnu -y
```

## 方式二：从 release 包安装

release地址：

https://github.com/riscv-collab/riscv-gnu-toolchain/releases

```bash
# 举例
wget https://github.com/riscv-collab/riscv-gnu-toolchain/releases/download/2024.04.12/riscv64-glibc-ubuntu-20.04-gcc-nightly-2024.04.12-nightly.tar.gz
tar -zxvf riscv64-glibc-ubuntu-20.04-gcc-nightly-2024.04.12-nightly.tar.gz

./riscv/bin/riscv64-unknown-linux-gnu-gcc -v
```

## 方式三：从源码编译

### 安装依赖

```bash
apt-get install autoconf -y
apt-get install automake -y
apt-get install autotools-dev -y
apt-get install curl -y
apt-get install python3 -y
apt-get install python3-pip -y
apt-get install libmpc-dev -y
apt-get install libmpfr-dev -y
apt-get install libgmp-dev -y
apt-get install gawk -y
apt-get install build-essential -y
apt-get install bison -y
apt-get install flex -y
apt-get install texinfo -y
apt-get install gperf -y
apt-get install libtool -y
apt-get install patchutils -y
apt-get install bc -y
apt-get install zlib1g-dev -y
apt-get install libexpat-dev -y
apt-get install ninja-build -y
apt-get install git -y
apt-get install cmake -y
apt-get install libglib2.0-dev -y
```

### 下载源码

```bash
git clone git@github.com:riscv-collab/riscv-gnu-toolchain.git
cd riscv-gnu-toolchain
git rm qemu musl spike pk   # 去掉无关模块
git submodule update --init --recursive --progress
```

### 编译

```bash
mkdir build
mkdir install

./configure --prefix=`pwd`/install --with-arch=rv64gc --with-abi=lp64d --enable-multilib
make linux -j 3
```

### 使用

```bash
/root/riscv-gnu-toolchain/install/bin/riscv64-unknown-linux-gnu-gcc -v
```
