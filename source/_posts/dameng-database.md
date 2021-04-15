---
title: 达梦数据库的安装与使用
date: 2021-04-14 15:40:06
tags: database
categories: Database
---

在测试服务器上部署了一个达梦单点服务，做一下简要的记录

<!--more-->

## DM8 安装

### 安装前准备

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
vi /etc/fstab
/dev/vdf1	/dmk	ext4	defaults	0	0
mount -a
```

#### 操作系统设置

##### 禁用 SELINUX

使用`setenforce`临时关闭

```shell
getenforce # 查看 SELINUX 状态，如果不是 disabled 状态使用 senenforce 设置
setenforce 0
```

也可以修改`config`文件禁用

```shell
vi /etc/selinux/config
# 如果不是 disabled 状态则修改为disabled
SELINUX=disabled
reboot
```

##### 关闭防火墙

```shell
systemctl status firewalld # 查看防火墙状态
systemctl stop firewalld
systemctl disable firewalld
```

##### 设置文件最大打开数目

```shell
ulimit -n 65536
```

修改`config`文件则永久生效

```shell
vi /etc/security/limits.conf
dmdba		soft	nofile	65536
dmdba		hard	nofile	65536
dmdba		soft	nproc	10240
dmdba		hard	nproc	10240
dmdba		soft	data	unlimited
dmdba		hard	data	unlimited
dmdba		soft	fsize	unlimited
dmdba		hard	fsize	unlimited
```

##### 用户权限配置

```shell
groupadd dinstall
useradd -g dinstall dmdba
chown -R dmdba:dinstall /dm
```

##### 其他设置

关闭 CUPS, Postfix, rpcbind 服务

#### 上传安装包

一共有`DMInstall.bin`，`dm.key`和`DmJdbcDriver16.jar`三个文件

使用sftp上传到`/dm/software`下

### 软件安装

切换到 `dmdba`执行安装

```shell
su - dmdba
cd /dm/software/
chmod 755 *
./DMInstall.bin -i
```

再此遇到了`Bash script and /bin/bash^M: bad interpreter: No such file or directory`的报错，可能是由于安装脚本在 Windows 下修改过导致，将换行符由 CRLF 转为 LF 可解决

```shell
sed -i -e 's/\r$//' DMInstall.bin
```

接着按提示导入`key`，设置时区，设置目录为`/dm/dmdba/dmdbms`

再切回root用户执行脚本

```shell
su - root
/dm/dmdba/dmdbms/script/root/root_installer.sh
```

配置好环境变量

```shell
vi ~/.bash_profile
export DM_HOME="/dmsoft/dmdbms"
export PATH=$DM_HOME/bin:$PATH
source ~/.bash_profile
```

### 单点配置

#### 初始化数据库

```shell
su - dmdba
cd /dm
mkdir dmdata dmarch dmbak
cd dmdba/dmdbms/bin
```

通过 `dminit`来设置参数

```shell
./dminit help # 查看参数说明，按照要求设置参数

./dminit PATH=/dm/dmdata DB_NAME=DMDB INSTANCE_NAME=DBSERVER PORT_NUM=5238 LOG_SIZE=300 EXTENT_SIZE=16 SYSBDA_PWD=[password] SYSAUDITOR_PWD=[password]
```

#### 注册 DM 数据库服务

```shell
cd /dm/dmdba/dmdbms/script/root
./dm_service_installer.sh -t dmserver -dm_ini /dm/dmdata/DMDB/dm.ini -p DBSERVER
```

