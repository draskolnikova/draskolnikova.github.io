---
layout: post
title: KeepAlive menggunakan metode Direct Routing
categories: [work]
tags: [work, linux, loadbalancer]
---

Saat ini saya ada project untuk konfigurasi *Load Balancer* dengan metode *Direct Routing*, atau yang biasa dikenal dengan DSR (*Direct Server Return*). Atau gambaran topologinya seperti ini.

|   IP Address  | Status | Hostname |
|---------------|--------|----------|
|               |  vIP   | LVS + LB |
|192.168.100.20 |  vIP   | WS1      |
|               |  vIP   | WS2      |
|               |  vIP   | WSx      |
|192.168.100.2  |  rIP   | LVS + LB |
|192.168.100.3  |  rIP   | WS1      |
|192.168.100.4  |  rIP   | WS2      |
|192.168.100.5  |  rIP   | WSx      |

<script src="https://gist.github.com/draskolnikova/8ccac12148ca9eea665d.js"></script>

*Testbed* kali ini saya menggunakan Cent OS 7, kebutuhannya *packages*-nya adalah `keepalived`.
```
root> yum install -y keepalived
```

Konfigurasi yang saya gunakan adalah sebagai berikut :

<script src="https://gist.github.com/draskolnikova/cee0e17dcd9bbdc66c6d.js"></script>

Silakan disesuaikan dengan kebutuhan pada saat deployment, terutama pada `<public_ip_vip>`, `<pub_interface_handle_rip_vip>` dan `<public_real_ip_address>`.

Setelah perubahan selesai dilakukan, silakan start `keepalived`.
```
root> systemctl start keepalived
```

Ketika `keepalived` sudah running dengan normal, seharusnya pada log muncul notifikasi seperti ini:
```
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived[15916]: Starting Keepalived v1.2.13 (03/06,2015)
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived[15916]: Remove a zombie pid file /var/run/keepalived.pid
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived[15916]: Remove a zombie pid file /var/run/vrrp.pid
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived[15916]: Remove a zombie pid file /var/run/checkers.pid
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived[15917]: Starting Healthcheck child process, pid=15918
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_healthcheckers[15918]: Initializing ipvs 2.6
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived[15917]: Starting VRRP child process, pid=15919
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_healthcheckers[15918]: Netlink reflector reports IP 192.168.100.20 added
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_healthcheckers[15918]: Netlink reflector reports IP 192.168.100.2 added
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_vrrp[15919]: Netlink reflector reports IP 192.168.100.20 added
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_vrrp[15919]: Netlink reflector reports IP 192.168.100.2 added
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_healthcheckers[15918]: Registering Kernel netlink reflector
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_vrrp[15919]: Registering Kernel netlink reflector
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_healthcheckers[15918]: Registering Kernel netlink command channel
Nov 26 00:21:08 loadbalancer.beritagar.id systemd[1]: Started LVS and VRRP High Availability Monitor.
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_vrrp[15919]: Registering Kernel netlink command channel
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_vrrp[15919]: Registering gratuitous ARP shared channel
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_healthcheckers[15918]: Opening file '/etc/keepalived/keepalived.conf'.
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_healthcheckers[15918]: Configuration is using : 16131 Bytes
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_healthcheckers[15918]: IPVS: Service already exists
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_healthcheckers[15918]: IPVS: Destination already exists
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_healthcheckers[15918]: Using LinkWatch kernel netlink reflector...
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_healthcheckers[15918]: Activating healthchecker for service [192.168.100.3]:80
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_healthcheckers[15918]: Activating healthchecker for service [192.168.100.4]:80
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_vrrp[15919]: Opening file '/etc/keepalived/keepalived.conf'.
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_vrrp[15919]: Configuration is using : 62447 Bytes
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_vrrp[15919]: Using LinkWatch kernel netlink reflector...
Nov 26 00:21:08 loadbalancer.beritagar.id Keepalived_vrrp[15919]: VRRP sockpool: [ifindex(2), proto(112), unicast(0), fd(10,11)]
Nov 26 00:21:09 loadbalancer.beritagar.id Keepalived_vrrp[15919]: VRRP_Instance(LB_1) Transition to MASTER STATE
Nov 26 00:21:10 loadbalancer.beritagar.id Keepalived_vrrp[15919]: VRRP_Instance(LB_1) Entering MASTER STATE
Nov 26 00:21:10 loadbalancer.beritagar.id Keepalived_vrrp[15919]: VRRP_Instance(LB_1) setting protocol VIPs.
Nov 26 00:21:10 loadbalancer.beritagar.id Keepalived_vrrp[15919]: VRRP_Instance(LB_1) Sending gratuitous ARPs on enp5s0f0 for 192.168.100.20
Nov 26 00:21:15 loadbalancer.beritagar.id Keepalived_vrrp[15919]: VRRP_Instance(LB_1) Sending gratuitous ARPs on enp5s0f0 for 192.168.100.20
```

Silakan tambahkan parameter `net.ipv4.ip_nonlocal_bind` dengan nilai `1` pada sysctl.

## Real Server
### Kernel Tuning
Kernel tuning untuk ignore arp di sisi webserver, karena kita akan pakai interface `loopback`, maka kita akan setup individual arp ignore parameter di interface tersebut.
```
root> vim /usr/lib/sysctl.d/90-arp.conf
net.ipv4.conf.lo.arp_ignore = 1
net.ipv4.conf.all.arp_ignore = 1
net.ipv4.conf.lo.arp_announce = 2
net.ipv4.conf.all.arp_announce = 2
```

Tambahkan vIP pada *loopback interface* di masing-masing *real server*.
```
root> ifconfig lo:0 192.168.100.20 netmask 255.255.255.255
```

### Interface Configuration
Supaya interface tersebut berjalan pada saat mesin di *boot-up*, maka tambahkan file pada `/etc/sysconfig/network-scripts/ifcfg-lo:0`, isinya sebagai berikut :
```
DEVICE=lo:0
IPADDR=192.168.100.20
NETMASK=255.255.255.255
ONBOOT=yes
NAME=loopback
```

Restart network service untuk memastikan script tersebut berjalan dengan normal.
```
root> systemctl restart network.service
```

Dari konfigurasi di atas, ada beberapa pro dan kontra mengenai metode Direct Routing atau DSR ini. Seperti yang di tuliskan oleh [Big-IP](https://devcentral.f5.com/articles/the-disadvantages-of-dsr-direct-server-return), akan ada effort lebih untuk melakukan maintenance backend, karena load balancer di sini hanya bertugas untuk melemparkan traffic, bukan melakukan terminasi traffic.

## Pros
- Very low budget 
- No need big / huge load balancer

## Cons
- Security issue will be more complex
- Cache optimization will be more complex since we must optimized all backend individually.