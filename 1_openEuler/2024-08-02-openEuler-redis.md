---
layout: post
title: openEuler install redis
date: 2024-08-02 16:40:23
description:
tags: openEuler
categories: openEuler
---

openEuler启动redis

## docker 启动 redis server

拉取镜像

```bash
docker pull redis:latest
```

拉起 redis server docker

```bash
docker run -itd --name my_redis -p 6379:6379 --restart=always redis
```

## docker启动 memtier_benchmark

拉取镜像

```bash
docker pull redislabs/memtier_benchmark:latest

```

查看 help 信息

```bash
docker run --rm redislabs/memtier_benchmark:latest --help
```

打流

```bash
docker run --rm --network=host --name my_benchmark redislabs/memtier_benchmark:latest -s 192.168.101.220 -n 50000 -c 10 -t 4 --ratio 1:0
```
