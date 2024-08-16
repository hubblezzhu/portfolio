---
layout: post
title: ubuntu install perf
date: 2024-08-02 16:40:42
description:
tags: ubuntu perf
categories: ubuntu
---

ubuntu 源码编译 perf

## 下载源码

```shell
apt-get install linux-source -y

# 解压源码
cd /usr/src/linux-source-5.15.0/linux-source-5.15.0/tools/perf
```

## 安装依赖

```shell
apt-get install bison -y
apt-get install flex -y
apt-get install libelf-dev -y
apt-get install libdw-dev -y
apt-get install libunwind-dev -y
apt-get install systemtap-sdt-dev -y
apt-get install libaudit-dev -y
apt-get install libslang2-dev -y
apt-get install libgtk2.0-dev -y
apt-get install liblzma-dev -y
apt-get install libnuma-dev -y
apt-get install python3-dev -y
apt-get install libperl-dev -y
apt-get install libiberty-dev -y
apt-get install libbfd-dev -y
apt-get install libcap-dev -y
apt-get install libzstd-dev -y
apt-get install libtraceevent-dev -y
apt-get install pkg-config -y
```

## 编译 & 安装

```shell
make
```

```shell
make install
```

## 使用

```shell
/root/bin/perf -h
```
