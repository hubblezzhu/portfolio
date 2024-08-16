---
layout: post
title: openEuler yum config
date: 2024-08-02 16:40:26
description:
tags: openEuler
categories: openEuler
---

openEuler 各版本 yum 源配置 \
优选 阿里云，阿里云没有的使用 openatom \

openEuler 22.03 及之后版本自带的 yum 源配置ok

## 清理原有yum源

```shell
cd /etc/yum.repos.d
mkdir backup
mv *.repo backup/
```

## openEuler 20.03

```shell
dnf config-manager --add-repo=https://mirrors.aliyun.com/openeuler/openEuler-20.03-LTS/OS/x86_64/
dnf config-manager --add-repo=https://mirrors.aliyun.com/openeuler/openEuler-20.03-LTS/everything/x86_64/
dnf makecache
```

## openEuler 20.09

```shell
dnf config-manager --add-repo=https://archives.openeuler.openatom.cn/openEuler-20.09/OS/x86_64/
dnf config-manager --add-repo=https://archives.openeuler.openatom.cn/openEuler-20.09/everything/x86_64/

## 禁用 gpgkey check
sed -i "s/gpgcheck=1/gpgcheck=0/g" /etc/dnf/dnf.conf

dnf makecache
```

## openEuler 21.03

```shell
dnf config-manager --add-repo=https://archives.openeuler.openatom.cn/openEuler-21.03/OS/x86_64/
dnf config-manager --add-repo=https://archives.openeuler.openatom.cn/openEuler-21.03/everything/x86_64/

## 禁用 gpgkey check
sed -i "s/gpgcheck=1/gpgcheck=0/g" /etc/dnf/dnf.conf

dnf makecache
```

## openEuler 21.09

```shell
dnf config-manager --add-repo=https://archives.openeuler.openatom.cn/openEuler-21.09/OS/x86_64/
dnf config-manager --add-repo=https://archives.openeuler.openatom.cn/openEuler-21.09/everything/x86_64/

## 禁用 gpgkey check
sed -i "s/gpgcheck=1/gpgcheck=0/g" /etc/dnf/dnf.conf

dnf makecache
```
