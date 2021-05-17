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
mkfs.ext4 /dev/vdf1
mkdir /dm
mount /dev/vdf1 /dm
```

设置永久挂载

```shell
vi /etc/fstab
/dev/vdf1	/dm	ext4	defaults	0	0
mount -a
```

#### 操作系统设置

##### 禁用 SELINUX

使用 setenforce 临时关闭

```shell
getenforce # 查看 SELINUX 状态，如果不是 disabled 状态使用 senenforce 设置
setenforce 0
```

也可以修改 config 文件禁用

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

修改 config 文件则永久生效

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

切换到 dmdba执行安装

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

接着按提示导入 key，设置时区，设置目录为`/dm/dmdba/dmdbms`

再切回 root 用户执行脚本

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

通过 dminit 来设置参数

```shell
./dminit help # 查看参数说明，按照要求设置参数

./dminit PATH=/dm/dmdata DB_NAME=DMDB INSTANCE_NAME=DBSERVER PORT_NUM=5238 LOG_SIZE=300 EXTENT_SIZE=16 SYSDBA_PWD=[password] SYSAUDITOR_PWD=[password]
```

#### 启用 DM 数据库服务

切到 root 用户

将服务注册到操作系统

```shell
cd /dm/dmdba/dmdbms/script/root
./dm_service_installer.sh -t dmserver -dm_ini /dm/dmdata/DMDB/dm.ini -p DBSERVER
```

使用 systemctl 管理服务

```shell
systemctl enable DmServiceDBSERVER.service
systemctl start DmServiceDBSERVER.service
```

## 数据库维护与管理

### 表与用户创建

登录数据库

```shell
cd /dm/dmdba/dmdbms/bin
./disql SYSDBA/[password]@localhost:5238
```

创建表空间 DMTBS，文件存储在 `/dm/dmdata`中，分为两个数据文件 DMTBS01.DBF 和 DMTBS02.DBF，每个文件初始大小为 64M，每次自动扩展 2M，单个数据文件最大 10G

```sql
SQL> CREATE TABLESPCE "DMTBS" DATAFILE '/dm/dmdata/DMTBS01.DBF' SIZE 64 AUTOEXTEND ON NEXT 2 MAXSIZE 10240, '/dm/dmdata/DMTBS02.DBF' SIZE 64 AUTOEXTEND ON NEXT 2 MAXSIZE 10240 CACHE = NORMAL;
```

创建用户 DMTEST，密码 [password]，默认表空间为 DMTBS

```sql
SQL> CREATE USER "DMTEST" IDENTIFIED BY [password] DEFAULT TABLESPACE "DMTBS"
```

创建角色 TESTROLE，具有创建表，创建视图，创建索引的权限

```sql
SQL> CREATE ROLE TESTROLE;
SQL> GRANT CREATE TABLE, CREATE VIEW, CREATE INDEX TO TESTROLE;
```

将角色 RESOURCE，TESTROLE 赋给 DMTEST 用户，并赋予 DMTEST 查询 DBA_TABLESPACES 的权限

```sql
SQL> GRANT RESOURCE, TESTROLE TO DMTEST;
SQL> GRANT SELECT ON DBA_TABLESPACES TO DMTEST;
```

在 DMTEST 用户下创建 EMP 表和 DEPT 表。

```sql
SQL> CREATE TABLE EMP (
    EMP_ID INT NOT NULL,
    EMP_NAME VARCHAR(20),
    EMAIL VARCHAR(50),
    PHONE_NUM VARCHAR(20),
    HIRE_DATE DATE,
    JOB_ID VARCHAR(10),
    MANAGER_ID INT,
    SALARY INT,
    DEPT_ID INT
);
SQL> CREATE TABLE DEPT (
    DEPT_ID INT NOT NULL,
    DEPT_NAME VARCHAR(30),
    LOCATION_ID INT,
    LOCATION_ADDR VARCHAR(30)
);
```

### 使用 sql 脚本

假设有脚本`/dm/script1.sql`

```sql
INSERT INTO EMP (EMP_ID, EMP_NAME) VALUES (1, 'NAME');
INSERT INTO EMP (EMP_ID, EMP_NAME, EMAIL) VALUES (2, 'NAME', 'a@b');
INSERT INTO DEPT (DEPT_ID, DEPT_NAME) VALUES (1, 'DEPTNAME');
```

连接到数据库后可直接调用该脚本

```
SQL> start /dm/script1.sql
```

### 创建视图

创建视图 VIEW_HIREDATE 存放 HIRE_DATE 为 2006-01-01 至 2009-01-01 的条目

```sql
SQL> CREATE VIEW VIEW_HIREDATE AS SELECT * FROM EMP WHERE HIRE_DATE BETWEEN '2006-01-01' AND '2009-01-01';
```

创建视图 VIEW_SALARY 查询平均 SALARY 大于 12000 的 DEPT 

```sql
SQL> CREATE VIEW VIEW_SALARY AS SELECT * FROM DEPT WHERE DEPT_ID IN (
    SELECT DEPT_ID
    FROM EMP
    GROUP BY DEPT_ID
    HAVING AVG(SALARY) > 12000
);
```

### 创建索引

在列 EMP_NAME 添加索引 IND_EMP_NAME

```sql
-- 创建索引表空间
SQL> CREATE TABLESPACE INDEX1 DATAFILE '/dm/dmdata/DMDB/index1_01.dbf' size 64 autoextend on next 10 mamxsize 2000;
-- 创建索引
SQL> CREATE INDEX IND_EMP_NAME ON EMP(EMP_NAME) TABLESPACE INDEX1;
```

### 数据库备份

#### 物理备份

##### 冷备

使用 DMRMAN 进行冷备， DMAP 服务为打开状态， 并关闭数据库实例

默认备份方式为完全备份

```shell
# 关闭数据库实例
systemctl stop DmServiceDBSERVER.service
cd /dm/dmdba/dmdbms/bin
./dmrman
# 完全备份
RMAN> backup database '/dm/dmdata/DMDB/dm.ini' full backupset '/dm/dmbak/DMDB_bak'
# 增量备份
RMAN> backup database '/dm/dmdata/DMDB/dm.ini' increment with backupdir '/dm/dmbak' backupset '/dm/dmbak/DMDB_bak'
# 查看备份信息
RMAN> show backupset '/dm/dmbak/DMDB_bak'
RMAN> show backupset '/dm/dmbak/DMDB_bak' info meta
```

##### 热备

使用 SQL 进行备份，DMAP 服务和数据库实例都是打开状态，为了保证备份数据的一致性，需要配置并开启本地归档

```sql
-- 打开归档
SQL> ALTER DATABASE MOUNT;
SQL> ALTER DATABASE ADD ARCHIVELOG 'type=local,dest=/dm/dmarch,file_size=64,space_limit=0';
-- 如果需要修改归档配置则可使用 ALTER DATABASE MODIFY ARCHIVELOG
SQL> ALTER DATABSE ARCHIVELOG;
SQL> ALTER DATABASE OPEN;
-- 检查归档状态
SQL> SELECT name, status$, arch_mode FROM v$database;

-- 完全备份
SQL> backup database full backupset '/dm/dmbak/DMDB_bak2';

-- 增量备份
SQL> backup database increment backupset '/dm/dmbak/incr_DMDB_bak2';
```

##### 自动备份作业

首先启动代理

```sql
SQL> SP_INIT_JOB_SYS(1);
```

可借助管理工具直接在 GUI 上编辑生成作业

创建 JOB1，每周日晚 22:00 对数据库做完全备份

```sql
SQL> call SP_CREATE_JOB('JOB1',1,0,'',0,0,'',0,'周日全备');
SQL> call SP_JOB_CONFIG_START('JOB1');
SQL> call SP_ADD_JOB_STEP('JOB1', '全备', 5, '01000/dm/dmbak', 1, 2, 0, 0, NULL, 0);
SQL> call SP_ADD_JOB_SCHEDULE('JOB1', '周日执行', 1, 2, 1, 1, 0, '22:00:00', NULL, '2021-04-20 02:34:52', NULL, '');
SQL> call SP_JOB_CONFIG_COMMIT('JOB1');
```

创建 JOB2，每周一到周六 22：00 对数据库做增量备份


```sql
SQL> call SP_CREATE_JOB('JOB2',1,0,'',0,0,'',0,'平时增备');
SQL> call SP_JOB_CONFIG_START('JOB2');
SQL> call SP_ADD_JOB_STEP('JOB2', '增备', 5, '01000/dm/dmbak|/dm/dmbak', 1, 2, 0, 0, NULL, 0);
SQL> call SP_ADD_JOB_SCHEDULE('JOB1', '周一到周六执行', 1, 2, 1, 126, 0, '22:00:00', NULL, '2021-04-20 02:34:52', NULL, '');
SQL> call SP_JOB_CONFIG_COMMIT('JOB2');
```

#### 逻辑备份

使用 dexp 对数据库执行逻辑导出

```shell
cd /dm/dmdba/dmdbms/bin
./dexp SYSDBA/[password]@localhost:5238 FILE=dmdb_full.dmp LOG=dmdb_full.log DIRECTORY=/dm/dmbak FULL=y
```

使用 dimp 导入

```shell
./dimp SYSDBA/[password]@localhost:5238 FILE=/dm/dmbak/dmdb_full.dmp LOG=/dm/dmbak/dimp.log
```

## 配置 ODBC

编译好 ODBC 后，配置 odbc.ini 和 odbcinst.ini

```shell
# odbc.ini
[dm8]
DESCRIPTION = DM ODBC DSND
DRIVER = DM8 ODBC DRIVER
SERVER = localhost
UID = SYSDA
PWD = [password]
TCP_PORT = 5238

# odbcinst.ini
[DM8 ODBC DRIVER]
DESCRIPTION = ODBC DRIVER FOR DM8
DRIVER = /dm/dmdba/dmdbms/bin/libdodbc.so
```

修改文件属性即可连接

```shell
chmod 777 odbc.ini
chmod 777 odbcinst.ini
su - dmdba
isql dm8
```