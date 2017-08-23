---
layout: post
title: Creating bridge interface using nmcli 
categories: [work]
tags: [linux, centos, networkmanager, nmcli] 
---

Recenly I got confused setting up VLAN and deliver it to my virtualization server(s). I pretty sure my VLAN configuration was working fine and double check the logic is working. I have 10 (Ten) KVM Host, 9 nine of them was properly connected to networks (bridged interface(s)) and the other is having problem because of the bridge interface won't up. 

## Problem

The interface can't connect to existing networks, it can be caused by manual bridge interface creation.

Since the network configuration is a bit differents between EL6 and EL7, [nmcli](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Networking_Guide/sec-Network_Bridging_Using_the_NetworkManager_Command_Line_Tool_nmcli.html) is the key here. Trying to hook up manual configuration by adding `brX` interface on `/etc/sysconfig/networks-script`, but no luck. 

Have you setup your bridge interface using this `old` configuration ?

```
TYPE=Bridge
BOOTPROTO=none
DEFROUTE=no
IPV4_FAILURE_FATAL=no
IPV6INIT=no
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
NAME=br0
DEVICE=br0
ONBOOT=yes
IPADDR=ip.ad.dr.es
PREFIX=24
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
```

If yes, trying to avoid that, and you should (must!) using `nmcli`.

To create bridge interface, you just only type this in your servers.

```
[root@linux ~]# nmcli con add type bridge ifname br0 stp yes priority 36864
[root@linux ~]# nmcli con add type ethernet ifname enp1s0f0 master bridge-br0
```

Where `enp1s0f0` is the interface and change with your real interface name, then `br0` is new bridge interface, you can named it, and it's not always br0 (can be br1, br2, etc). And if you want to see script files inside `/etc/sysconfig/networks-script/`, it should be like this :

```
DEVICE=br0
STP=no
TYPE=Bridge
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
NAME=bridge-br0
UUID=411a4a13-caab-41e1-83f5-4d1c31ab0fb4
ONBOOT=yes
IPADDR=192.168.69.25
PREFIX=24
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
```

## Conclusion

If you're using NetworkManager on EL7, you should avoid creating or editing network configuration located under `/etc/sysconfig/networks-script`. Use NetworkManager (available on `nmcli` or `nmtui`) instead of manual.
