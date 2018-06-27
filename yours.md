# 维护一个自己的 bbr 魔改
如果你也想自己调参数玩了，或者你想自己做新内核的适配，那么你可能需要该文。

要维护一个属于你自己的 bbr 魔改，你需要做以下三件事：
1. 换内核
2. 改源码
3. make install


## 换内核
你需要换内核的话，本文列出三种方式，自己选一种你喜欢的：
- 下载 deb 安装
- 找 ubuntu repo 安装
- 用我的 bbr 脚本安装，第一个选项就是

当然，其中需要注意的是，image、headers_all、headers_generic（或者 lowlatency） 都是不可少的，少一个你就等着 make 爆炸吧


## 改源码
当然，换内核后，你还需要改对应内核版本的 bbr 源码，源码自己去 linux kernel 找，然后找到 ~/net/ipv4/bbr.c（大概是这个路径，具体不记得了），想怎么改就随你喜欢了，只要能用。

想说不会改或者懒得改的，这里有一直到 v4.16.x 版本为止的各版本线的魔改 bbr 源码适配：  
https://github.com/tcp-nanqinlang/general/tree/master/General/Debian/source


## make install
然后就是编译 bbr 模块并安装，过程不多说了，参考这个 readme：  
https://github.com/tcp-nanqinlang/tested/blob/master/readme.md


## 其它资料
你可以在以下 wiki 集中找到实现本文所述目的的所有资料：  
https://github.com/tcp-nanqinlang/wiki/wiki/

以及，实在不懂的，自己要会先去用搜索引擎，这也是对被你提问的人的最基本尊重


## 不会再有更新了
[tcp-nanqinlang](https://github.com/tcp-nanqinlang) 这个魔改 bbr 项目我维护快满一周年了（不信的自己去翻 commit 记录。first commit 2017.07.12），没有收获到什么，且时间长了都是会腻的。没有催生出什么技术（别说探讨了，感兴趣的人都没有），倒是说不定被拿来干了一堆坏事。

不会再有更新了。以后不会再有更新，也不会再有 bug 修复，新功能增加也是不可能的。

最后一次做一位好人。

以上。