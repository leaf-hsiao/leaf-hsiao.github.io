---
title: 达梦数据库的安装与使用
date: 2021-04-14 15:40:06
tags: database
categories: Database
---

在测试服务器上部署了一个达梦单点服务，做一下简要的记录

<!--more-->

## DM8 安装

### 系统环境设置

#### 配置安装目录

单独申请了一块盘`/dev/vdf`，分区格式化后将其挂载到 `/dm` 下

```shell
fdisk /dev/vdf
mkfx -t ext4 /dev/vdf1
mkdir /dm
mount /dev/vdf1 /dm
```

设置永久挂载

```shell
vim /etc/fstab

# 添加 /dev/vdf1	/dmk	ext4	defaults	0	0

mount -a
```

