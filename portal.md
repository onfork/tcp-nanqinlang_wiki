# 传送门：魔改 BBR - tcp_nanqinlang 集合

## 概述
本文是作为我的魔改 BBR 的一个集合，因为我的相关主题还算比较多，所以在此集中列举出来。

## wiki 部分
魔改 BBR 一键脚本 for Debian & CentOS  
适用于 `KVM` 环境  
需要更换 Linux 内核  
https://github.com/tcp-nanqinlang/wiki/general

OpenVZ 魔改 BBR - LKL 一键脚本  
适用于 `OpenVZ` 环境  
使用 `Haproxy` 进行转发  
https://github.com/tcp-nanqinlang/wiki/lkl-haproxy

OpenVZ 魔改 BBR - Rinetd 一键脚本  
适用于 `OpenVZ` 环境  
使用 `Rinetd` 进行转发  
https://github.com/tcp-nanqinlang/wiki/lkl-rinetd


## Github 部分
### 项目地址  
https://github.com/tcp-nanqinlang

### no-check-virt
其中 `nocheckvirt` 表示不检查虚拟化，当然最起码你的虚拟化是拥有换内核权限的，也就是给你们例如 hyper-v 或 dedicated-server 或 XEN 等玩的。可能翻车，谨慎操作。
- general:
 - Debian: https://github.com/tcp-nanqinlang/general/releases/tag/3.4.2.1
 - CentOS: https://github.com/tcp-nanqinlang/general/tree/master/General/CentOS/bash
- lkl-haproxy: https://github.com/tcp-nanqinlang/lkl-haproxy/releases/tag/1.1.1
- lkl-rinetd: https://github.com/tcp-nanqinlang/lkl-rinetd/releases/tag/1.1.0-nocheckvirt
