---
layout: post
title: net protocol BGP
date: 2024-10-08 16:40:15
description:
tags: net-protocol BGP
categories: net-protocol
---

BGP 协议
========






## Question

#### 1、BGP 状态机

•	Idle：等待事件触发。
•	Connect：尝试建立TCP连接。
•	Active：继续尝试建立TCP连接。
•	OpenSent：发送 Open 消息，等待回应。
•	OpenConfirm：等待 Keepalive 消息确认。
•	Established：建立连接并交换路由信息。
