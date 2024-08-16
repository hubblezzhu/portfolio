---
layout: post
title: openEuler enable cgroup v2
date: 2024-08-02 16:40:20
description:
tags: openEuler cgroupv2
categories: openEuler
---

openEuler 切换 cgroup v2

## 版本

openEuler： openEuler 23.03 x86

## 步骤

```shell
grubby --update-kernel=ALL --args=systemd.unified_cgroup_hierarchy=1
reboot

```
