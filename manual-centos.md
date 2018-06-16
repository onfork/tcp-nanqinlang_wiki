# 魔改 BBR 手动安装教程（CentOS 篇）

## 概述
因为时不时就会有问脚本怎么怎么样的，为了方便各位的理解和自行调试，我还是决定把一步步的手动教程写出来。这次先出 `CentOS` 篇。

关于本项目的相关脚本，请看这里：[魔改 BBR 脚本 - tcp_nanqinlang](https://github.com/tcp-nanqinlang/wiki/wiki/general)

## 所需环境
1. `CentOS 6/7`
2. `KVM`

可以这样查看你的虚拟化技术：
```bash
yum install -y virt-what
virt-what
```

## 安装步骤
`如果决定手动安装，请务必严格按照本文所述顺序（不可更改命令执行顺序或省略步骤，会出事的），一步步执行并确保执行无误`

### 更换内核
编译魔改算法需要你安装有 `linux-ml-4.12.10` `linux-ml-devel-4.12.10` `linux-ml-headers-4.12.10` 三个内核。本文以 mainline v4.12.10 为例。

1.使用以下命令删除旧内核。删除了旧内核才能安装新内核：
```bash
# 查看当前已安装的所有内核
# 返回的一长串就是下面的“内核名”
# 对每个内核都执行一次删除指令（其中会有一个内核报错删除不了，那个是重启后更换了运行内核后才能删除的重启前的（当前的）运行内核，等你根据后面的步骤重启后再删除那个内核）
rpm -qa | grep kernel | grep -v "4.12.10"

# 删除内核
yum remove -y “内核名”
```

2.使用以下命令安装新内核（linux-devel-4.12.10 和 linux-headers-4.12.10）：
```bash
# CentOS 6
wget https://raw.githubusercontent.com/nanqinlang/CentOS-kernel/master/kernel-ml-4.12.10-1.el6.elrepo.x86_64.rpm && yum install -y kernel-ml-devel-4.12.10-1.el6.elrepo.x86_64.rpm
wget https://raw.githubusercontent.com/nanqinlang/CentOS-kernel/master/kernel-ml-devel-4.12.10-1.el6.elrepo.x86_64.rpm && yum install -y kernel-ml-devel-4.12.10-1.el6.elrepo.x86_64.rpm
wget https://raw.githubusercontent.com/nanqinlang/CentOS-kernel/master/kernel-ml-headers-4.12.10-1.el6.elrepo.x86_64.rpm && yum  install -y kernel-ml-headers-4.12.10-1.el6.elrepo.x86_64.rpm
sed -i '/default=/d' /boot/grub/grub.conf && echo -e "\ndefault=0\c" >> /boot/grub/grub.conf

# CentOS 7
wget https://raw.githubusercontent.com/nanqinlang/CentOS-kernel/master/kernel-ml-4.12.10-1.el7.elrepo.x86_64.rpm && yum install -y kernel-ml-devel-4.12.10-1.el7.elrepo.x86_64.rpm
wget https://raw.githubusercontent.com/nanqinlang/CentOS-kernel/master/kernel-ml-devel-4.12.10-1.el7.elrepo.x86_64.rpm && yum install -y kernel-ml-devel-4.12.10-1.el7.elrepo.x86_64.rpm
wget https://raw.githubusercontent.com/nanqinlang/CentOS-kernel/master/kernel-ml-headers-4.12.10-1.el7.elrepo.x86_64.rpm && yum  install -y kernel-ml-headers-4.12.10-1.el7.elrepo.x86_64.rpm
grub2-mkconfig -o /boot/grub2/grub.cfg && grub2-set-default 0
```

3.以上两步执行完成后再次检查当前已安装内核：
```bash
rpm -qa | grep kernel
```
确认操作无误后，`重启你的 VPS`。

4.重启开机后，删除重启前剩下的那个旧内核（在上文提到的）：
```bash
# 删除重启前剩下的那个旧内核
rpm -qa | grep kernel | grep -v "4.12.10"
yum remove -y “内核名”
```

### 安装算法
5.经过以上步骤后，再次确认你的内核：
```bash
# 指令
rpm -qa | grep kernel

# 返回值（大概是这样）
linux-kernel-ml-4.12.10-1
linux-kernel-ml-devel-4.12.10-1
linux-kernel-ml-headers-4.12.10-1
```
确认返回值相同后，就可以执行以下了：
```bash
yum groupinstall -y "Development Tools"

wget https://raw.githubusercontent.com/nanqinlang-tcp/tcp_nanqinlang/General/CentOS/source/tcp_nanqinlang.c
wget -O Makefile https://raw.githubusercontent.com/nanqinlang-tcp/tcp_nanqinlang/master/Makefile/Makefile-CentOS
make && make install

sed -i '/net\.ipv4\.tcp_congestion_control/d' /etc/sysctl.conf
echo -e "\nnet.ipv4.tcp_congestion_control=nanqinlang\c" >> /etc/sysctl.conf && sysctl -p
```

以上均执行无误后，返回例如如下即安装成功：
```bash
root@sometimesnaive:# lsmod | grep nanqinlang
tcp_nanqinlang  7014  23
```

## 相关 issue
https://github.com/nanqinlang/sometimesnaive.org/issues/50