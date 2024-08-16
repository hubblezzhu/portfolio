---
layout: post
title: ubuntu compile kernel
date: 2024-08-02 16:40:41
description:
tags: ubuntu
categories: ubuntu
---

基于 ubuntu 编译 linux kernel

## 版本

Build OS: ubuntu 22.04 LTS x86 \

## 下载源码

```shell
apt-get update
apt-get install tar -y
apt-get install linux-source -y

# 解压源码
cd /usr/src/linux-source-5.15.0
tar -jxvf linux-source-5.15.0.tar.bz2

cd linux-source-5.15.0
```

## 安装依赖

```shell
apt-get install libelf-dev gcc make libssl-dev dwarves libncurses-dev bison flex -y
```

## 修改编译配置项

使用原有config

```shell
cp /boot/`uname -r`.config
```

按需微调

```shell
make menuconfig
```

## 修改原config中的证书

```shell
scripts/config --disable SYSTEM_TRUSTED_KEYS
scripts/config --disable SYSTEM_REVOCATION_KEYS
```

## 编译 & 安装

```shell
make -j 3
```

```shell
make modules_install
make install

reboot
```

reboot 后在系统启动选择系统界面就可以选择新编的内核了
