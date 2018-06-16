# 魔改 BBR 手动安装教程（Debian 篇）

## 概述
之前有写过 [CentOS 的手动安装教程](https://github.com/tcp-nanqinlang/wiki/wiki/manual-centos)，这次来写 `Debian` 篇。

关于本项目的相关脚本，请看这里：[魔改 BBR 脚本 - tcp_nanqinlang](https://github.com/tcp-nanqinlang/wiki/wiki/general)

本文所述教程，对应脚本为 `pro` 版本。

## 所需环境
1. `Debian 7+`
2. `KVM`

可以这样查看你的虚拟化技术：
```bash
apt-get install -y virt-what
virt-what
```

## 安装步骤
`如果决定手动安装，请务必严格按照本文所述顺序（不可更改命令执行顺序或省略步骤，会出事的），一步步执行并确保执行无误`

### 更换内核
编译魔改算法需要你安装有 `linux-image` `linux-headers-all` `linux-headers-$bit` 三个内核。
#### 安装新内核
以 mainline v4.10.10 为例。
```bash
# 确认你的系统是 32 or 64 位
bit=`uname -m`

# 64 位
wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.10.10/linux-image-4.10.10-041010-lowlatency_4.10.10-041010.201704120813_amd64.deb
dpkg -i linux-image-4.10.10-041010-lowlatency_4.10.10-041010.201704120813_amd64.deb
wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.10.10/linux-headers-4.10.10-041010_4.10.10-041010.201704120813_all.deb
dpkg -i linux-headers-4.10.10-041010_4.10.10-041010.201704120813_all.deb
wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.10.10/linux-headers-4.10.10-041010-lowlatency_4.10.10-041010.201704120813_amd64.deb
dpkg -i linux-headers-4.10.10-041010-lowlatency_4.10.10-041010.201704120813_amd64.deb

# 32 位
wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.10.10/linux-image-4.10.10-041010-lowlatency_4.10.10-041010.201704120813_i386.deb
dpkg -i linux-image-4.10.10-041010-lowlatency_4.10.10-041010.201704120813_i386.deb
wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.10.10/linux-headers-4.10.10-041010_4.10.10-041010.201704120813_all.deb
dpkg -i linux-headers-4.10.10-041010_4.10.10-041010.201704120813_all.deb
wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.10.10/linux-headers-4.10.10-041010-lowlatency_4.10.10-041010.201704120813_i386.deb
dpkg -i linux-headers-4.10.10-041010-lowlatency_4.10.10-041010.201704120813_i386.deb
```
#### 删除旧内核
删除除了 v4.10.10 版本以外的所有内核。
```bash
# 列出所有多余内核
# 返回的一长串就是下面的“内核名”
dpkg -l|grep linux-image   | awk '{print $2}' | grep -v "4.10.10"
dpkg -l|grep linux-headers | awk '{print $2}' | grep -v "4.10.10"

# 对上面列出的每个内核都执行一次删除指令
apt-get purge -y “内核名”
```
#### 检查内核
先运行以下命令：
```bash
update-grub
```
执行完毕后，再次检查当前内核是否已安装无误：
```bash
dpkg -l|grep linux-image   | awk '{print $2}'
dpkg -l|grep linux-headers | awk '{print $2}'

# 返回如下值
linux-image-4.10.10-041010-lowlatency
linux-headers-4.10.10-041010
linux-headers-4.10.10-041010-lowlatency
```
然后就可以重启你的 VPS 了。开机后进入第二步。

### 启用算法
经过第一步，已经成功的更换了系统内核。接下来，要启用魔改 BBR，按照顺序执行以下：
```bash
# dependences
apt-get update && apt-get install -y build-essential

# Makefile
## Debian 7/8
wget -O Makefile https://raw.githubusercontent.com/nanqinlang-tcp/tcp_nanqinlang/master/Makefile/Makefile-Debian7or8
## Debian 9
wget -O Makefile https://raw.githubusercontent.com/nanqinlang-tcp/tcp_nanqinlang/master/Makefile/Makefile-Debian9

# source
wget https://raw.githubusercontent.com/nanqinlang-tcp/tcp_nanqinlang/master/General/Debian/source/kernel-v4.12andbelow/tcp_nanqinlang.c

# make + install
make && make install

# sysctl
echo -e "\nnet.core.default_qdisc=fq" >> /etc/sysctl.conf
echo -e "net.ipv4.tcp_congestion_control=nanqinlang\c" >> /etc/sysctl.conf
```

## 检查状态
以上过程均完成后，使用以下命令检查是否已成功启用魔改 BBR：
```bash
sysctl net.ipv4.tcp_available_congestion_control | awk '{print $3}'
# 返回值：
nanqinlang

lsmod | grep nanqinlang
# 返回值（类似于）：
tcp_nanqinlang    6053  18
```