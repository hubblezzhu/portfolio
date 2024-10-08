---
layout: post
title: net protocol TCP
date: 2024-10-08 16:40:15
description:
tags: net-protocol TCP
categories: net-protocol
---

TCP 协议
========






## Question

#### 1、什么时候会收到 RST 报文
1. connect一个不存在的端口
2. 向一个已经关掉的连接send数据
3. 向一个已经崩溃的对端发送数据（连接之前已经被建立）
4. close(sockfd)时，直接丢弃接收缓冲区未读取的数据，并给对方发一个RST。这个是由SO_LINGER选项来控制的；
5. a重启，收到b的保活探针，a发rst，通知b
