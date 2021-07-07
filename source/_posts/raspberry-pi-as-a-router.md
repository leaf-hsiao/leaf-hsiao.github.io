---
title: 将树莓派改造成路由
date: 2021-07-07 17:01:35
tags: 
  - OpenWrt
  - Raspberry Pi
  - Docker
---

Xbox 到手后，光速开通了 XGP ，白嫖是如此的快乐，再看看 Steam 库里正价购买的命运2，瞬间觉得亏了一个亿。随即遇到的问题便是主机加速，租房且有室友的情况下不方便改变现有网络结构，单独买个加速盒又觉得性价比太低，突然想起买来即吃灰的树莓派，正好能用上

<!-- more -->

![Drawing_of_Raspberry_Pi_model_B_rev2](/images/raspberrypi/Drawing_of_Raspberry_Pi_model_B_rev2.svg "树莓派 Model B")(https://commons.wikimedia.org/wiki/File:Drawing_of_Raspberry_Pi_model_B_rev2.svg)

手上这台是 Raspberry Pi 1 Model B 只有百兆口，两个 USB 2.0 也没不支持转接千兆网卡，同时不像 3 及以上有 802.11n 无线网卡，因此不适合配置成无线 AP，不过单就游戏加速这一目的算是非常合适了。接着是系统选择，直接刷个 OpenWrt 感觉可玩性不太高，树莓派的性能压榨也不够充分，在 Docker 里跑则更便利，后期的可扩展性更强

## 安装 Raspberry Pi OS

因为过于机型过于古老，因此直接选择了官方的 [Raspberry Pi OS](https://www.raspberrypi.org/software/operating-systems/#raspberry-pi-os-32-bit) （原 Raspbian ），较新的机型也可以选择其他优化过的系统。同样因为较为孱弱的性能，选择了没有桌面环境的 Raspberry Pi OS Lite，~~~HDMI 直连我的 2k 170 过于艰难~~~，直接 headless 初始化。

用官方的 [Raspberry Pi Imaging Utility](https://github.com/raspberrypi/rpi-imager) 进行烧录，完成后在根目录下建立一个 名为`ssh`的空文件以启用 SSH，带无线网卡的也可以在根目录建好 `wpa_supplicant.conf`详情参考 [Setting up a Raspberry Pi headless](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md)

将树莓派接好电源和网线后，在路由器后台查找到对应 IP，使用 SSH 连接上去。默认用户为 pi 密码为 raspberry ，按照提示修改好密码，随后可以进行下一步，也可以先按自己的使用习惯 配置好源，安装 Oh My Zsh 等

![](/images/raspberrypi/raspberry_pi_ssh.png "SSH")

## 安装 Docker

图省事直接使用 [`get.docker.com`](get.docker.com) 提供的脚本安装

```shell
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
```

国内用户可以选用阿里云或者 Azure 的镜像

```shell
$ sudo sh get-docker.sh --mirror Aliyun
$ sudo sh get-docker.sh --mirror AzureChinaCloud
```

### 以非 root 用户管理 Docker （可选）

> The Docker daemon binds to a Unix socket instead of a TCP port. By default that Unix socket is owned by the user `root` and other users can only access it using `sudo`. The Docker daemon always runs as the `root` user.
>
> If you don’t want to preface the `docker` command with `sudo`, create a Unix group called `docker` and add users to it. When the Docker daemon starts, it creates a Unix socket accessible by members of the `docker` group.

创建 `docker` 组，并把 `pi` 用户加入用户组，并切换到 `docker` 登录，再测试是否能无需 `sudo` 运行 `docker`

```shell
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
$ newgrp docker
$ docker run  --rm  hello-world
```

### 启动 Docker daemon

使用 `systemctl` 来手动启动 Docker daemon

```shell
$ sudo systemctl start docker
```

在 Debian 和 Ubuntu 上，Docker 服务已经默认设置为开机启动了，在其他发行版则需执行如下命令

```shell
$ sudo systemctl enable docker.service
$ sudo systemctl enable containerd.service
```

## 安装 OpenWrt

推荐直接采用 [OpenWrt-Rpi-Docker](https://github.com/SuLingGG/OpenWrt-Rpi-Docker) ，对照[在Docker 中运行 OpenWrt 旁路网关](https://mlapp.cn/376.html)安装

### 配置 `macvlan` 网络

打开网卡混杂模式

```shell
$ sudo ip link set eth0 promisc on
```

并在 `/etc/rc.local` 中添加 `ip link set eth0 promisc on` ，以便重启也能生效

创建 `macvlan` ，依据之前查到的树莓派的 IP，修改对应的 `subnet` 和 `gateway` 地址（以 `192.168.1.x` 为例，则两个地址分别修改为 `192.168.1.0/24`和`192.168.1.1`）

```shell
$ docker network create -d macvlan \
  --subnet=172.16.86.0/24 \
  --gateway=172.16.86.1 \
  -o parent=eth0 macnet
```

### 创建容器

从 [Docker 仓库](https://hub.docker.com/r/sulinggg/openwrt)拉取对应版本的 OpenWrt，也可以从阿里云拉取

```shell
$ docker pull sulinggg/openwrt:rpi1
# docker pull registry.cn-shanghai.aliyuncs.com/suling/openwrt:rpi1
```

创建并启动容器（注意镜像名，如果是阿里云拉取的则需修改为 `registry.cn-shanghai.aliyuncs.com/suling/openwrt:rpi1`）

```shell
$ docker run --restart always --name openwrt -d --network macnet --privileged sulinggg/openwrt:rpi1 /sbin/init
```

### 修改配置

进入容器修改网络配置文件

```shell
$ docker exec -it openwrt bash
$ vi /etc/config/network
```

将 Lan 口`ipaddr`修改为树莓派所在网段的任一不重复 IP （即`192.168.1.x`）`gateway`和 `dns` 设置为路由器地址，然后重启网络

```
/etc/init.d/network restart
```

## 配置 OpenWrt

在浏览器中输入之前设置 Lan 口`ipaddr`即可进入控制面板，默认用户为 root，密码 password，登录后先在“系统 - 管理权”中修改密码

在“网络 - 接口 - Lan - 修改”中关闭 DHPC（忽略此接口）

接着在需要连接的设备指定网关即可



接着重点来了，由于电信那个路由不支持 vlan，加速器改造大失败 😭

最后还是拿着 pc 当加速器用了 🤦‍

## 参考资料

[SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/)

[Setting up a Raspberry Pi headless](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md)

[Install Docker Engine on Debian](https://docs.docker.com/engine/install/debian/)

[Post-installation steps for Linux](https://docs.docker.com/engine/install/linux-postinstall/)

[在Docker 中运行 OpenWrt 旁路网关](https://mlapp.cn/376.html) 

[Use macvlan networks](https://docs.docker.com/network/macvlan/)