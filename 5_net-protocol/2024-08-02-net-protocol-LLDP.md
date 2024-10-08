---
layout: post
title: net protocol LLDP
date: 2024-08-02 16:40:14
description:
tags: net-protocol LLDP
categories: net-protocol
---

LLDP 协议


## LLDP 协议的过程

LLDP（Link Layer Discovery Protocol，链路层发现协议）是用于交换设备之间链路信息的开放标准协议，通常用于网络设备间发现彼此的存在和拓扑信息。它工作在数据链路层（OSI模型的第2层），可以帮助管理员快速了解网络拓扑结构。LLDP协议的过程主要包括以下几个步骤：

1. LLDP数据帧的生成

每个支持LLDP的设备都会生成包含设备信息的LLDP数据帧。这些数据帧被称为LLDPDU（LLDP数据单元），包括以下主要信息：

	•	设备的标识信息（如系统名称、端口名称）
	•	设备的能力（如路由器、交换机等）
	•	设备的管理地址
	•	端口信息（如接口速度、双工模式等）

这些信息是通过TLV（Type-Length-Value）格式表示的，每个TLV都有一个类型标识符、长度字段和实际数据。

2. LLDP数据帧的发送

LLDP协议通过多播的方式在网络中发送LLDPDU。这些帧被发送到专用的多播MAC地址01:80:C2:00:00:0E，该地址保证帧只能在本地链路上传播，而不会被转发到其他网络。

LLDP数据帧一般以周期性方式发送，默认发送间隔通常为30秒（可以根据配置进行调整）。此外，LLDP设备还会在设备启动时发送一些初始的LLDPDU，用于快速发现。

3. LLDP数据帧的接收

当设备收到LLDPDU后，会解析其中的TLV信息，并将这些信息保存在一个称为邻居表（Neighbor Table）的数据库中。每条邻居信息通常会包括以下内容：

	•	邻居设备的标识信息
	•	邻居设备的端口信息
	•	邻居设备的能力（如交换机、路由器等）

这些邻居信息为网络管理员提供了清晰的网络拓扑视图，方便设备管理和排查故障。

4. 老化和超时机制

每个LLDPDU都有一个TTL（生存时间）字段，用于标识该信息的有效时间。如果在TTL时间内未收到来自同一设备的更新LLDPDU，该设备的信息将从邻居表中删除。

5. LLDP的管理和控制

LLDP信息可以通过SNMP（Simple Network Management Protocol，简单网络管理协议）进行收集和管理。管理员可以通过SNMP访问网络设备中的LLDP邻居表，获取网络的拓扑信息。这使得LLDP成为网络管理系统（如NMS）进行自动网络拓扑发现的常用手段之一。

LLDP的优势

	•	跨厂商互操作性：LLDP是一个开放标准协议（IEEE 802.1AB），与厂商无关。
	•	简单易用：协议简单，易于配置，能够迅速生成网络拓扑图。
	•	安全性：LLDP不转发数据包，信息仅限于本地链路，有一定的安全性。

总结

LLDP协议通过定期在网络中广播链路信息，使设备能够自动发现并记录网络中的邻居设备。这为网络管理员提供了一种轻松管理和监控网络拓扑的方式，帮助提高网络运行效率和故障诊断速度。




## Question
