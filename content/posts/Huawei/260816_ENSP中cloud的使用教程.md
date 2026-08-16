+++
date = '2026-08-16'
draft = false
title = 'FTP渗透测试'
tags= ["ENSP","华为"] 
categories=["Huawei"]
featured_image =  '/images/260816/0.png'
+++

在学习网络工程师的过程中，ensp中的cloud一直让人疑惑，他是云服务器吗？怎么用？怎么设置接口？

但实际cloud并不是连接云端，而是**打通实体网络的通道，而不局限于ensp内部的设备**

得知此功能后，我尝试通过ensp连接我的Ubuntu Linux虚拟机，下面是操作过程：

*本次的环境中，安装ensp的设备是虚拟机Windows 10（为了确保纯净的环境能正常启动）于是网卡均连接到VM中的NAT模式，确保在同一网段*

cloud的配置如图所示

![](/images/260816/1.png)

先创建udp端口，类型默认为Eth或GE，第二个创建本地网卡，绑定信息为ethernet0

*不同设备可能不同，如果需要在visualbox配置的虚拟机需要将绑定信息设置为VirtualBox Host-Only Network #X*

入端口编号1，出端口编号2，勾选双向通道，**端口类型需与创建端口的一致**

随后配置PC的IP **（需与nat网段一致同时确保不冲突，可以提前查看Linux、主机的IP）**，启动设备即可

可以先尝试ping安装ensp的主机，再到网关，最后再pingLinux或者其他设备

![](/images/260816/2.png)

#### 无法连接的排查：

1. 使用 `display interface brief` 检查接口是否全开 **（可能有拓扑显示已连接但是接口是down的情况）**

2. 使用抓包或 `arp -a` 检测Windows设备是否有看见ensp的设备的ip

3. 尝试关闭Windows防火墙 *注意内网环境安全关闭*，或者其他杀毒软件

4. 尝试重启设备或ensp ~~可能是ensp的bug或visualbox异常~~

----

### 补充笔记

`display current-configuration` 

显示比较详细的信息，如默认的设置（设备名、cluster集群 ntdp路由交换 ndp邻居发现、aaa用户配置、接口情况）

`display this` 

能简化一些，或者看当前配置下的操作配置

`display interface ` 

可以看接口状态，和该设备的mac地址（hardware address）

`display interface brief` 

可以看接口是否开启关闭
