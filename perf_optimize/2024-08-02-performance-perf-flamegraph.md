---
layout: post
title: perf flamegraph usage
date: 2024-08-02 16:40:27
description:
tags: perf flamegraph
categories: performance-tool
---

perf 火焰图使用

## 下载 火焰图 脚本

```bash
cd ~
wget https://github.com/brendangregg/FlameGraph/archive/refs/tags/v1.0.tar.gz
tar -zxvf v1.0.tar.gz
```

## 抓取 perf data

以 redis-server 为例 \
-F 是采样频率，建议质数 \
建议使用 dwarf 方式，栈信息更准确

```bash
perf record -F 97 -p `pidof redis-server` -g --call-graph=dwarf sleep 60
```

## 生成火焰图

```bash
perf script | /root/FlameGraph-1.0/stackcollapse-perf.pl | /root/FlameGraph-1.0/flamegraph.pl > redis_server_`hostname`.svg
```
