---
layout: post
title: trace-cmd usage
date: 2024-08-02 16:40:37
description:
tags: trace trace-cmd
categories: trace
---

trace-cmd 使用

## 环境

openEuler23_03 6.1.19-7.0.0.17.oe2303.aarch64

## 安装

```bash
dnf install trace-cmd -y
```

## 使用

#### 列出可用的 tracer

```bash
trace-cmd list -t
```

#### 列出可追踪的 function

```bash
trace-cmd list -f
```

#### 追踪特定函数

1、开始记录

```bash
trace-cmd record -P 281760 -p function_graph -g do_sys_open
```

另外还可以添加 -l <func> 过滤函数，支持通配符

2、查看结果

```bash
trace-cmd report
```

## 常用跟踪函数

```bash
ksys_read
ksys_write
```
