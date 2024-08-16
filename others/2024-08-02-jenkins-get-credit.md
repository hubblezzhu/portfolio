---
layout: post
title: Jenkins get credit
date: 2024-08-02 16:40:12
description:
tags: Jenkins
categories: Jenkins
---

通过 jenkins script console 获取所有 jenkins 账号，密码

## 步骤

jenkins HOME 页面 -> manage jenkins -> script console

运行

```shell
com.cloudbees.plugins.credentials.SystemCredentialsProvider.getInstance().getCredentials().forEach{
  it.properties.each { prop, val ->
    println(prop + ' = "' + val + '"')
  }
  println("-----------------------")
}
```
