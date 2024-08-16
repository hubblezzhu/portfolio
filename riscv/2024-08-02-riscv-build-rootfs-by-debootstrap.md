---
layout: post
title: riscv build rootfs by debootstrap
date: 2024-08-02 16:40:28
description:
tags: riscv
categories: riscv
---

基于 debootstrap 构建 RISC-V 64 Ubuntu 根文件系统

## 版本

VM OS：ubuntu 20.04 \
RISC-V root fs：RISC-V 64 Ubuntu 22.04 LTS

## 步骤

### 1、在 VM 上安装需要的程序

```bash
apt install debootstrap qemu qemu-user-static binfmt-support dpkg-cross --no-install-recommends
```

### 2、生成最小 bootstrap rootfs

```bash
# 在当前文件夹下生成 temp-rootfs
debootstrap --arch=riscv64 --foreign jammy ./temp-rootfs http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports
```

### 3、chroot, debootstrap

```bash
chroot temp-rootfs /bin/bash
/debootstrap/debootstrap --second-stage
```

### 4、修改 apt 源，安装 risc-v 包

```bash
cat >/etc/apt/sources.list <<EOF
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy main restricted universe multiverse
# deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy-updates main restricted universe multiverse
# deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy-updates main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy-backports main restricted universe multiverse
# deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy-backports main restricted universe multiverse

# deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy-security main restricted universe multiverse
# # deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy-security main restricted universe multiverse

deb http://ports.ubuntu.com/ubuntu-ports/ jammy-security main restricted universe multiverse
# deb-src http://ports.ubuntu.com/ubuntu-ports/ jammy-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy-proposed main restricted universe multiverse
# # deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy-proposed main restricted universe multiverse
EOF

apt-get update
```

```bash
mount -t proc proc  /proc
mount -t sysfs sys  /sys
# mount -o bind dev  /dev
# mount -o bind dev/pts  /dev/pts
```

```bash
apt-get install --no-install-recommends -y util-linux haveged openssh-server systemd kmod initramfs-tools conntrack ebtables ethtool iproute2 iptables mount socat ifupdown iputils-ping vim dhcpcd5 neofetch sudo chrony
```

```bash
apt install rsyslog -y
apt install language-pack-en-base -y
apt install language-pack-zh-hans -y
apt install net-tools -y
apt install resolvconf -y
apt install network-manager -y
apt install usbutils -y
apt install sysstat -y
apt install bash-completion -y
```

```bash
umount /proc /sys
# umount /dev/pts /dev
```

### 5、基本配置

```bash
cat >>/etc/network/interfaces <<EOF
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
EOF
```

```bash
cat >/etc/resolv.conf <<EOF
nameserver 223.5.5.5
nameserver 8.8.8.8
EOF
```

```bash
echo 'riscv-ubuntu2204' > /etc/hostname
echo "127.0.0.1 localhost" > /etc/hosts
echo "127.0.0.1 riscv-ubuntu2204" >> /etc/hosts
```

```bash
# 设置密码
passwd root
## 设置允许 root 登录
sed -i "s/#PermitRootLogin.*/PermitRootLogin yes/g" /etc/ssh/sshd_config
```

```bash
# 分区配置
cat >/etc/fstab <<EOF
# <file system> <mount pt>  <type>  <options>   <dump>  <pass>
/dev/root   /       ext2    rw,noauto   0   1
proc        /proc       proc    defaults    0   0
devpts      /dev/pts    devpts  defaults,gid=5,mode=620,ptmxmode=0666   0   0
tmpfs       /dev/shm    tmpfs   mode=0777   0   0
tmpfs       /tmp        tmpfs   mode=1777   0   0
tmpfs       /run        tmpfs   mode=0755,nosuid,nodev  0   0
sysfs       /sys        sysfs   defaults    0   0
EOF
```

至此，根文件系统中的 软件包都已安装完毕，退出 chroot

```bash
exit
```

### 6、制作 ext 镜像

制作 ext4 格式

```bash
# 40G 根目录
dd if=/dev/zero of=rootfs_ubuntu_riscv.ext4 bs=1M count=40960
mkfs.ext4 rootfs_ubuntu_riscv.ext4
mkdir -p tmpfs

mount -t ext4 rootfs_ubuntu_riscv.ext4 tmpfs/ -o loop
cp -af temp-rootfs/* tmpfs/
umount tmpfs

chmod 777 rootfs_ubuntu_riscv.ext4
```

制作 ext2 格式

```bash
# 40G 根目录
dd if=/dev/zero of=rootfs_ubuntu_riscv.ext2 bs=1M count=40960
mkfs.ext2 rootfs_ubuntu_riscv.ext2
mkdir -p tmpfs

mount -t ext2 rootfs_ubuntu_riscv.ext2 tmpfs/ -o loop
cp -af temp-rootfs/* tmpfs/
umount tmpfs

chmod 777 rootfs_ubuntu_riscv.ext2
```

### 7、qemu 启动

```bash
../qemu-9.0.0/build/qemu-system-riscv64 \
    -M virt \
    -cpu rv64 \
    -smp 8 \
    -m 32G \
    -kernel ./Image \
    -append "rootwait root=/dev/vda ro" \
    -drive file=rootfs_ubuntu_riscv.ext4,format=raw,id=hd0 \
    -device virtio-blk-device,drive=hd0 \
    -nographic \
    -netdev user,id=net0 -device virtio-net-device,netdev=net0
```

其中 Image 为交叉编译好的内核镜像

### 8、启动后即可进入基于 ubuntu 的riscv OS，已经安装好 apt-get

```bash
root@riscv-ubuntu2204:~# apt-get -v
apt 2.4.12 (riscv64)
Supported modules:
*Ver: Standard .deb
*Pkg:  Debian dpkg interface (Priority 30)
 Pkg:  Debian APT solver interface (Priority -1000)
 Pkg:  Debian APT planner interface (Priority -1000)
 S.L: 'deb' Debian binary tree
 S.L: 'deb-src' Debian source tree
 Idx: Debian Source Index
 Idx: Debian Package Index
 Idx: Debian Translation Index
 Idx: Debian dpkg status file
 Idx: Debian deb file
 Idx: Debian dsc file
 Idx: Debian control file
 Idx: EDSP scenario file
 Idx: EIPP scenario file


root@riscv-ubuntu2204:~# lscpu
Architecture:          riscv64
  Byte Order:          Little Endian
CPU(s):                8
  On-line CPU(s) list: 0-7


root@riscv-ubuntu2204:~# free -g
               total        used        free      shared  buff/cache   available
Mem:              31           0          30           0           0          30
Swap:              0           0           0
```

## 可能遇到的问题

### 1、制作 ext 镜像时，没有 /dev/zero 文件

```bash
 mknod -m 666 /dev/zero c 1 5
```

### 2、qemu 不支持 -netdev user

```bash
# 重新编译 qemu，添加 --enable-slirp 参数
cd qemu-9.0.0
# ../configure
./configure --enable-kvm --enable-virtfs --enable-slirp --target-list=riscv64-linux-user,riscv64-softmmu

make -j 15
```

### 3、制作 ext 镜像时，Operation not permitted

重启下vm后，继续后续步骤

```bash
reboot
```

## 参考

https://blog.csdn.net/flyfish1986/article/details/130500977 \

https://github.com/rajnesh-kanwal/linux/wiki/Running-CTR-basic-demo-on-QEMU-RISC%E2%80%90V-Virt-machine#3-build-root-fs \

https://github.com/carlosedp/riscv-bringup/blob/master/Ubuntu-Rootfs-Guide.md
