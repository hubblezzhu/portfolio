---
layout: post
title: send linux kernel patch
date: 2024-08-02 16:40:36
description:
tags: patch
categories: others
---

向内核提交riscv patch

## 步骤

### 1、安装 git send-email

```bash
sudo apt install git git-email
```

交互式指导参考：https://git-send-email.io/#step-1

### 2、配置 smtp server

修改.gitconfig 文件，参考

```conf
[user]
        name = Zhu Hengbo
        email = zhuhengbo@iscas.ac.cn
[sendemail]
        from = Zhu Hengbo <zhuhengbo@iscas.ac.cn>
        smtpserver = xxxxxx
        smtpserverport = xxxxxx
        smtpencryption = xxxxxx
        smtpuser = zhuhengbo@iscas.ac.cn
        smtppass = xxxxxx
        supresscc = all
        confirm = always
```

### 3、制作 patch，检查

```bash
git format-patch -1
```

```bash
cd linux
./scripts/checkpatch.pl 0001-riscv-add-tracepoints-for-page-fault.patch
```

### 4、发送 patch

获取 maintainer 信息

```bash
cd linux
./scripts/get_maintainer.pl 0001-riscv-add-tracepoints-for-page-fault.patch
```

```bash
git send-email --to xxxxxx --cc xxxxxx 0001-riscv-add-tracepoints-for-page-fault.patch
```

### 5、Tips

##### 1、在 .gitconfig 中添加配置

```conf
[sendemail.linux]
	tocmd ="`pwd`/scripts/get_maintainer.pl --nogit --norolestats "
	cccmd ="`pwd`/scripts/get_maintainer.pl --nogit --nogit-fallback --norolestats --nom"
```

发送patch

```shell
git send-email --identity=linux ./0001-riscv-add-tracepoints-for-page-fault.patch
```

这样会直接帮忙把多个 --to 的收件人 和 --cc 的抄送人填好

##### 2、patch 需要更新

如果patch需要更新，在 commit msg 中加上

```txt
---
Changes in v2:
- xxxxxx
```

然后重新生成 Patch

```bash
git format-patch -1 --subject-prefix="[PATCH vXXX]"
```

之后重新发送patch 即可
