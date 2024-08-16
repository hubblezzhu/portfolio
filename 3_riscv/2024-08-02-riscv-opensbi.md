---
layout: post
title: riscv opensbi compile
date: 2024-08-02 16:40:31
description:
tags: riscv opensbi
categories: riscv
---

编译构建 opensbi

## 版本

ubuntu 20.04

## 从源码编译

```bash
git clone git@github.com:riscv-software-src/opensbi.git


cd opensbi
export ARCH=riscv
export CROSS_COMPILE=/root/workspace/riscv/bin/riscv64-unknown-linux-gnu-

make clean
make DEBUG=1 PLATFORM=generic PLATFORM_RISCV_XLEN=64 FW_PAYLOAD_PATH=../u-boot/u-boot.bin

```

安装到执行目录

```bash
cp build/platform/generic/firmware/fw_payload.bin /root/workspace/run_debug/
cp build/platform/generic/firmware/fw_payload.elf /root/workspace/run_debug/
```

## 启动 opensbi + kernel

```bash

```
