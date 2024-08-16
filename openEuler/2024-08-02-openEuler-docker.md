---
layout: post
title: openEuler install docker
date: 2024-08-02 16:40:19
description:
tags: openEuler
categories: openEuler
---

openEuler 安装 docker

## 删除旧版本

```bash
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

## 配置docker yum repo

```shell
vi /etc/yum.repos.d/docker-ce.repo


[docker-ce-stable]
name=Docker CE Stable - $basearch
baseurl=https://download.docker.com/linux/centos/9/$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg

```

## 安装

```bash
yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

## 启动

```bash
systemctl enable docker
systemctl start docker
```

## 安装指定版本

查询可用版本

```bash
yum list docker-ce --showduplicates | sort -r
```

安装制定版本 \
如 18.09

```bash
yum install docker-ce-3:18.09.9-3.el7 docker-ce-cli-1:18.09.9-3.el7 containerd.io docker-buildx-plugin docker-compose-plugin -y
```

Attention: \
docker-ce 和 docker-ce-cli 的版本字符串可能不一样，需要分别制定
