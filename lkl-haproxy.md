# OpenVZ 魔改 BBR - lkl-haproxy 一键脚本

## 概述
这里是 [tcp_nanqinlang](https://github.com/tcp-nanqinlang/wiki/general) 的 `lkl-haproxy` 分支。

本项目 Github 地址： https://github.com/tcp-nanqinlang/lkl-haproxy


## 开始使用
以下适用于 Debian 8+ 环境：
```language-bash
# Debian 8+
# 64 bit
# ldd > = 2.14
# tun/tap enabled
wget https://github.com/tcp-nanqinlang/lkl-haproxy/releases/download/1.1.1/tcp_nanqinlang-haproxy-debian.sh
bash tcp_nanqinlang-haproxy-debian.sh
```

以下适用于 CentOS 7 环境：
```language-bash
# CentOS 7
# 64 bit
# ldd > = 2.14
# tun/tap enabled
wget https://github.com/tcp-nanqinlang/lkl-haproxy/releases/download/1.1.1/tcp_nanqinlang-haproxy-centos.sh
bash tcp_nanqinlang-haproxy-centos.sh
```


## 使用说明
以下进行脚本使用说明：

### 安装 lkl-haproxy
此命令用于安装 lkl-haproxy。

在 `/home/tcp_nanqinlang` 进行安装，所以安装完成后不要动这个文件夹了（除非你想修改端口）。

安装过程中，会提示你选择`单个端口`或`端口段`输入，具体已在运行脚本的提示中有说明，这里不再赘述。

安装完成后，会开启 lkl-haproxy。以后重启机器也会随开机自启。

以后若需要修改转发端口，请将 `/home/tcp_nanqinlang/haproxy.cfg` 中的端口号和 `/home/tcp_nanqinlang/redirect.sh` 中的端口号改为你想要的端口或端口段，修改完成后重启服务器。

使用前请注意自己的 iptables 相关设置。

### 检查 lkl-haproxy 运行状态
此命令用于检查 lkl-haproxy 运行与否，可通过返回的提示判断。

### 卸载 lkl-haproxy
运行此命令会卸载 haproxy、删除 /home/tcp_nanqinlang、移除 rc.local 开机自启项。稍后请自行移除 `iptables` 相关规则。