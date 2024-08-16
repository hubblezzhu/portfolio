---
layout: post
title: openEuler install libbpf
date: 2024-08-02 16:40:22
description:
tags: openEuler eBPF
categories: openEuler
---

openEuler install libbpf

## 版本

openEuler: 23.03 x86 \
linux src code: 6.1

## 步骤

```shell
cd /usr/src/linux-6.1.19-7.0.0.17.oe2303.x86_64

make headers_install
make modules_prepare


```

```shell

cd tools/lib/bpf
make
make install
```

安装后的 libbpf.so 和 libbpf.a 在 /usr/local/lib64 下

```bash
root@localhost.localdomain:/usr/src/linux-6.1.19-7.0.0.17.oe2303.x86_64/tools/lib/bpf git:(master) ll /usr/local/lib64 | grep bpf
drwxr-xr-x. 2 root root    4096 Mar 27 12:52 bpf
-rw-r--r--. 1 root root 4241178 Jun 30 15:32 libbpf.a
lrwxrwxrwx. 1 root root      15 Jun 30 15:32 libbpf.so -> libbpf.so.1.1.0
lrwxrwxrwx. 1 root root      15 Jun 30 15:32 libbpf.so.1 -> libbpf.so.1.1.0
-rwxr-xr-x. 1 root root 1979496 Jun 30 15:32 libbpf.so.1.1.0
```
