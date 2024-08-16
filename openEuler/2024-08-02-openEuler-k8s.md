---
layout: post
title: openEuler install kubernetes
date: 2024-08-02 16:40:21
description:
tags: openEuler kubernetes
categories: openEuler
---

通过 kubeadm 安装 k8s 集群

## 版本

kubernetes 1.27 arm64 \
openEuler 23.03 arm64

## 准备 containerd

containerd 在 docker 的 yum repo 里

### 1、安装 containerd

```shell
vi /etc/yum.repos.d/docker-ce.repo


[docker-ce-stable]
name=Docker CE Stable - $basearch
baseurl=https://download.docker.com/linux/centos/9/$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg

```

```shell
yum install containerd.io -y
```

### 2、修改 containerd 配置文件

```shell
containerd config default > /etc/containerd/config.toml
sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml
```

### 3、启动 containerd

```shell
systemctl enable --now containerd
systemctl restart containerd
```

### 4、Forwarding IPv4 and letting iptables see bridged traffic

```shell
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

# sysctl params required by setup, params persist across reboots
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

```

openEuler 23.03 中 /etc/sysctl.conf 中给 net.ipv4.ip_forward 也赋值了，需要注释掉

```shell
sed -i 's/net.ipv4.ip_forward\=0/#net.ipv4.ip_forward\=0/g' /etc/sysctl.conf

```

```shell
# Apply sysctl params without reboot
sudo sysctl --system
```

## 安装 kubeadm

```shell
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
enabled=1
gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kubelet kubeadm kubectl
EOF

# Set SELinux in permissive mode (effectively disabling it)
sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

sudo systemctl enable --now kubelet
```

## 通过 kubeadm 拉起集群

### 1、关闭防火墙

```shell
systemctl stop firewalld
systemctl disable firewalld
```

### 2、关闭 swap

```shell
swapoff -a
sed -ri 's/.*swap.*/#&/' /etc/fstab
```

### 3、集群初始化

```shell
kubeadm init --apiserver-advertise-address=10.211.55.26 --service-cidr=10.96.0.0/12 --pod-network-cidr=10.244.0.0/16 -v=5
```

重置集群

```shell
kubeadm reset
```

### 4、拉起集群成功

node 信息

```bash
root@oe23-master:~ kubectl get nodes
NAME          STATUS   ROLES           AGE   VERSION
oe23-master   Ready    control-plane   24h   v1.27.3
```

集群信息

```bash
root@oe23-master:~ kubectl cluster-info
Kubernetes control plane is running at https://10.211.55.26:6443
CoreDNS is running at https://10.211.55.26:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

容器镜像

```bash
root@oe23-master:~ crictl images
IMAGE                                     TAG                 IMAGE ID            SIZE
quay.io/cilium/cilium                     <none>              14515a7951cd7       164MB
quay.io/cilium/operator-generic           <none>              05b024f6fd59f       20.1MB
registry.k8s.io/coredns/coredns           v1.10.1             97e04611ad434       14.6MB
registry.k8s.io/etcd                      3.5.7-0             24bc64e911039       80.7MB
registry.k8s.io/kube-apiserver            v1.27.3             39dfb036b0986       30.4MB
registry.k8s.io/kube-controller-manager   v1.27.3             ab3683b584ae5       28.2MB
registry.k8s.io/kube-proxy                v1.27.3             fb73e92641fd5       21.4MB
registry.k8s.io/kube-scheduler            v1.27.3             bcb9e554eaab6       16.5MB
registry.k8s.io/pause                     3.9                 829e9de338bd5       268kB
```

```bash
root@oe23-master:~ crictl ps
CONTAINER           IMAGE               CREATED             STATE               NAME                      ATTEMPT             POD ID              POD
d0ba45b45abac       97e04611ad434       24 hours ago        Running             coredns                   0                   22eec6fac05d9       coredns-5d78c9869d-9rkvn
63f23fcb30737       97e04611ad434       24 hours ago        Running             coredns                   0                   71451d3216f37       coredns-5d78c9869d-9xn6n
08bae8a51c355       05b024f6fd59f       24 hours ago        Running             cilium-operator           0                   e502e30148ca3       cilium-operator-77b8c5887b-qfh6f
0bfbb453ece26       14515a7951cd7       24 hours ago        Running             cilium-agent              0                   6f291faa51b6b       cilium-688s9
600d149c24330       fb73e92641fd5       24 hours ago        Running             kube-proxy                0                   cee0e1f67c413       kube-proxy-xtqdx
71bb92175ff03       ab3683b584ae5       24 hours ago        Running             kube-controller-manager   1                   9bf4eb30de426       kube-controller-manager-oe23-master
89ed0d1e110a1       bcb9e554eaab6       24 hours ago        Running             kube-scheduler            1                   bf9a85e11e0e0       kube-scheduler-oe23-master
beb2a7529f930       39dfb036b0986       24 hours ago        Running             kube-apiserver            1                   58c9e709ba41d       kube-apiserver-oe23-master
21a1327ce3fb0       24bc64e911039       24 hours ago        Running             etcd                      1                   2c5e3dfb520c4       etcd-oe23-master
```
