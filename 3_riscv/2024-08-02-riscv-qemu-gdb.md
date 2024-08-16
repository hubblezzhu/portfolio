---
layout: post
title: riscv enable qemu gdb
date: 2024-08-02 16:40:32
description:
tags: riscv qemu
categories: riscv
---

## 使用QEMU和GDB调试Linux内核

## 拉起 qemu 内核环境

```bash

# 下面 -s 指定开启 gdb 远程调试端口 ：1234
../qemu-9.0.0/build/qemu-system-riscv64 \
    -s \
    -M virt \
    -cpu rv64 \
    -smp 8 \
    -m 32G \
    -kernel ./Image \
    -append "rootwait root=/dev/vda ro" \
    -drive file=rootfs_ubuntu_riscv.ext4,format=raw,id=hd0 \
    -device virtio-blk-device,drive=hd0 \
    -nographic \
    -netdev user,id=net0 -device virtio-net-device,netdev=net0


# 还可以添加 -S 指定 os 挂起，等 gdb attach 来调试引导
```

## gdb 调试

```shell
root@isrc bin # ./riscv64-unknown-linux-gnu-gdb
GNU gdb (GDB) 14.1
Copyright (C) 2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "--host=x86_64-pc-linux-gnu --target=riscv64-unknown-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word".
(gdb)
(gdb)
(gdb) target remote localhost:1234
```
