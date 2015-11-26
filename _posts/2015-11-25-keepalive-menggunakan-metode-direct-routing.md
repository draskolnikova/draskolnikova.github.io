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
