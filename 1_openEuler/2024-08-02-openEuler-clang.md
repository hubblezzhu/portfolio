---
layout: post
title: openEuler install clang
date: 2024-08-02 16:40:17
description:
tags: openEuler
categories: openEuler
---

openEuler 安装 clang

## 版本

openEuler： openEuler 23.03 \
clang ： clang15 \
llvm ： llvm15

## 步骤

```shell
dnf install clang15 llvm15 -y
```

```shell
export PATH=$PATH:/usr/lib64/llvm15/bin
```
