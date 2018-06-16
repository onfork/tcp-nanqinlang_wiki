# 魔改 BBR 一键脚本 for Debian & CentOS

## 概述
魔改 BBR 一键脚本，包含 Debian && CentOS 两个版本。

此文所述版本，仅适用于 `KVM` 的 Debian 7+ (32/64 bit) 或 CentOS 6+ (64 bit)。

OpenVZ 请右转 [lkl-haproxy](https://github.com/tcp-nanqinlang/wiki/lkl-haproxy) 或 [lkl-rinetd](https://github.com/tcp-nanqinlang/wiki/lkl-rinetd) 分支。

BBR 是出自谷歌员工之手的应用于 Linux 内核中的拥塞控制技术。关于拥塞控制技术，可以参看我之前的一篇帖子： [也谈TCP拥塞控制技术 与BBR的加速原理](https://sometimesnaive.org/article/8)

本项目 Github 地址：https://github.com/tcp-nanqinlang/general


## 开始使用
这个是 `新手简装` 版本，只需 `运行脚本第一项+重启+运行脚本第二项`。一般用户只需使用此版本，并建议使用该版本。此版本不需要编译的过程，直接安装 v4.10.2 内核。
```language-bash
# Debian 7+
# fool
wget https://github.com/tcp-nanqinlang/general/releases/download/3.4.2.1/tcp_nanqinlang-fool-1.3.0.sh
bash tcp_nanqinlang-fool-1.3.0.sh
```

这个是 `进阶` 版本。提供自定义内核版本功能，只建议有`用户自己指定安装的内核的版本`需求的用户使用，例如你想安装 v4.12.10 版本的内核，就需要使用这个版本。目前此版本脚本提供 linux kernel `v4.9.3 ~ v4.14.32` 支持。
```language-bash
# Debian 7+
# pro
wget https://github.com/tcp-nanqinlang/general/releases/download/3.4.2.1/tcp_nanqinlang-pro-3.4.2.1.sh
bash tcp_nanqinlang-pro-3.4.2.1.sh
```

这个是 `CentOS` 平台的版本，仍未确定为正式版，请勿在重要环境使用。
```language-bash
# CentOS 6/7
# only 64 bit
wget https://raw.githubusercontent.com/tcp-nanqinlang/general/master/General/CentOS/bash/tcp_nanqinlang-1.3.2.sh
bash tcp_nanqinlang-1.3.2.sh
```


## 使用说明
出现四个选项供以选择：

### 安装内核
`必须使用此命令安装内核并重启！`

安装内核时，请注意区别：

- Debian
 - 下载内核安装包至 /home/tcp_nanqinlang，脚本第二项运行完成后移除该文件夹
 - 系统中只会留下新安装的内核，原有的所有内核都会被移除
 - 对于 pro 版本，安装的内核版本由你指定，若不确定应输入哪个版本号，直接回车即可，会安装 v4.10.10 版本内核
 - 指定安装内核版本为 v4.13.x 时，会使用新版本内核适配的源码
 - 本魔改项目暂不支持 v4.14 及以上版本内核
 - 此命令执行完毕后，请根据脚本内提示确认内核是否已安装完毕

- CentOS：套路和上面 Debian 的大致相当，主要在于以下区别：
 - 不会询问安装版本号，直接安装内核版本 v4.12.10
 - 内核安装完成后，系统中会装有 `linux-ml-4.12.10` `linux-ml-devel-4.12.10` `linux-ml-headers-4.12.10` 三个内核
 - 内核安装完成后，系统中依旧会留有旧版本的 linux-x.xx.xx-ml 内核，这些残留的内核，会在执行第二个选项 “安装并启用算法” 后被移除

确认内核更换完成后，`重启你的 vps`。

重启开机后，`再次运行该脚本，选择第二项: 安装并开启算法`。

### 开启算法
用于编译并启用魔改 BBR 算法。

### 检查运行状态
用于检查 tcp_nanqinlang 是否已被 加载 (installed) 和 启用 (running)。

### 卸载
不会删除已安装的内核，仅移除 sysctl.conf 中的相关设置项。然后重启机器后，魔改 BBR 才会停止运作。


## 注意事项
1. 一定要在执行完成 `安装内核` 并重启 vps 后，才能执行 `安装并启用算法`
2. 卸载命令不会改动内核
3. 若 pro 版本的编译过程报错，请留言

## 手动教程
以下是 Debian or CentOS 的手动安装的教程：
- https://github.com/tcp-nanqinlang/wiki/manual-debian
- https://github.com/tcp-nanqinlang/wiki/manual-centos
