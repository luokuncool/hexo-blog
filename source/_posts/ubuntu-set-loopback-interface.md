---
title: ubuntu16设置环回网络
tags:
  - Linux
  - Ubuntu
  - Loopback
date: 2018-05-10 16:01:39
---


系统默认带有一个`127.0.0.1`的换回网络，想添加更多有如下两种方法。

## 1. 第一种直接通过ifconfig命令添加  

添加以前先使用`ifconfig -a`查看目前都有哪些网络，可以看到分别有`eth0`,`eth1`,`lo`。

```
eth0      Link encap:Ethernet  HWaddr 08:00:27:ac:ea:bc  
          inet addr:10.0.2.15  Bcast:10.0.2.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:feac:eabc/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:1037 errors:0 dropped:0 overruns:0 frame:0
          TX packets:690 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:93176 (93.1 KB)  TX bytes:78768 (78.7 KB)

eth1      Link encap:Ethernet  HWaddr 08:00:27:49:50:84  
          inet addr:192.168.10.12  Bcast:192.168.10.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe49:5084/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:18 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:1596 (1.5 KB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:22 errors:0 dropped:0 overruns:0 frame:0
          TX packets:22 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:2373 (2.3 KB)  TX bytes:2373 (2.3 KB)
```
然后执行如下命令添加`lo:0`。
```bash
ifconfig lo:0 192.168.10.1 netmask 255.255.255.255 up
```
再使用`ifconfig -a`查看就回发现多了一个`lo:0`了, 说明配置成功了。

```
eth0      Link encap:Ethernet  HWaddr 08:00:27:ac:ea:bc  
          inet addr:10.0.2.15  Bcast:10.0.2.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:feac:eabc/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:1037 errors:0 dropped:0 overruns:0 frame:0
          TX packets:690 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:93176 (93.1 KB)  TX bytes:78768 (78.7 KB)

eth1      Link encap:Ethernet  HWaddr 08:00:27:49:50:84  
          inet addr:192.168.10.12  Bcast:192.168.10.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe49:5084/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:18 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:1596 (1.5 KB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:22 errors:0 dropped:0 overruns:0 frame:0
          TX packets:22 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:2373 (2.3 KB)  TX bytes:2373 (2.3 KB)

lo:0      Link encap:Local Loopback  
          inet addr:192.168.10.11  Mask:255.255.255.255
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
```

通过`ifconfig`在系统重启之后会丢失，可以尝试第二种方式。

## 2. 编辑`/etc/network/interfaces`配置文件

用vi打开该配置文件找到如下几行

```
# The loopback network interface
auto lo
iface lo inet loopback
```

进行编辑后保存退出，然后重启`service networking restart`服务，再用`ifconfig -a`查看，将会看到和第一步配置以后一样的内容
```
auto lo lo:0
iface lo inet loopback

iface lo:10 inet static
        address 192.168.10.11
        netmask 255.255.255.255
```
