---
layout: post
title: openEuler install qemu-kvm
date: 2024-08-02 16:40:24
description:
tags: openEuler
categories: openEuler
---

创建 openEuler VM

## 版本

x86 \
Host OS：ubuntu \
Guest OS：openEuler 2109 \
Guest OS：openEuler 2303

## 步骤

#### 1、安装依赖包

```bash
dnf install qemu-kvm libvirt virt-install bridge-utils -y
```

#### 2、下载镜像

```bash
wget https://repo.huaweicloud.com/openeuler/openEuler-21.09/ISO/x86_64/openEuler-21.09-x86_64-dvd.iso

wget https://repo.huaweicloud.com/openeuler/openEuler-23.03/ISO/x86_64/openEuler-23.03-x86_64-dvd.iso
```

#### 3、创建虚拟磁盘

以安装 openEuler 2109 为例

```bash
root@dell-PowerEdge-T150:/home/zhb/qemu# qemu-img create -f qcow2 oe2109_1.qcow2 200G
```

#### 4、定义 xml 文件

```bash
touch oe2109_1.xml
```

模版参考

```xml
<domain type='kvm'>
  <name>oe2109_1</name>                                 <!-- 虚拟机名字 -->
  <memory unit='MiB'>8192</memory>                      <!-- 内存大小  -->
  <currentMemory unit='MiB'>8192</currentMemory>
  <vcpu placement='static'>4</vcpu>                     <!-- cpu个数 -->
  <resource>
    <partition>/machine</partition>
  </resource>
  <os>
    <type arch='x86_64'>hvm</type>
    <boot dev='hd'/>                                    <!-- 启动顺序，优先hd，其次cdrom -->
    <boot dev='cdrom'/>
  </os>
  <cpu mode='host-passthrough'>
    <feature policy='disable' name='vmx'/>
  </cpu>
  <devices>
    <disk type='file' device='disk'>
      <driver name='qemu' type='qcow2'/>
      <source file='/home/zhb/qemu/oe2109_1.qcow2'/>    <!-- hd位置，第 3 步中创建的文件路径 -->
      <target dev='vda' bus='virtio'/>
    </disk>
    <disk type='file' device='cdrom'>
      <driver name='qemu' type='raw'/>
      <source file='/home/zhb/openEuler-21.09-x86_64-dvd.iso'/> <!-- 镜像路径 -->
      <target dev='hdb' bus='ide'/>
    </disk>

    <interface type='network'>
      <source network='default' bridge='virbr0'/>
      <model type='virtio'/>
      <dhcp/>                                                   <!-- 开启dhcp -->
    </interface>
    <graphics type='vnc' autoport='yes' listen='0.0.0.0'>
      <listen type='address' address='0.0.0.0'/>
    </graphics>
  </devices>
</domain>
```

#### 5、启动虚拟机

```bash
virsh define oe2109_1.xml
virsh start oe2109_1
```

查看虚拟机

```bash
virsh list --all
```

#### 6、通过 vnc 安装 os

获取 vnc 端口

```bash
root@dell-PowerEdge-T150:/home/zhb/qemu# virsh vncdisplay oe2109_1
:11
```

之后通过 vncviewer 等工具就可以连上 vm 安装界面了
