---
layout: post
title: Cent OS 7, Working with nmcli
categories: [work]
tags: [work, personal]
---

Bagi yang sudah terbiasa menggunakan *operating system* Cent OS 6, untuk migrasi ke Cent OS 7 rata-rata masih harus mikir 2x, karena [perbedaan SysV dan SystemD](http://www.pcworld.com/article/2841873/meet-systemd-the-controversial-project-taking-over-a-linux-distro-near-you.html).

Biasanya, untuk melakukan konfigurasi standar post instalasi linux server, hal yang pertama kali dilakukan adalah *network configuration*.

Network configuration, hanya melakukan perubahan pada file yang berlokasi di `/etc/sysconfig/network-script/ifcfg-&lt;iface_name&gt;`

Admin diwajibkan untuk menghafal beberapa syntax yang ada di dalamnya, seperti `IPADDR`, `PREFIX` dan beberapa parameter lainnya yang harus di rubah.

Tapi, di [EL 7](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Networking_Guide/sec-Using_the_NetworkManager_Command_Line_Tool_nmcli.html), `nmcli` mempermudah tanpa harus melakukan editing pada file nya. 

## Konfigurasi Static IP Address

```
$ nmcli c m &lt;iface_name&gt; ipv4.method manual ipv4.addr "192.168.1.2/24" 
ipv4.dns "8.8.8.8,8.8.4.4" ipv4.gateway "192.168.1.2" connection.autoconnect "yes" 
```

## Konfigurasi DHCP IP Address

```
$ nmcli c m &lt;iface_name&gt; ipv4.method auto connection.autoconnect "yes"
```

Kalau diperiksa lebih lanjut, file yang ada di `/etc/sysconfig/network-scripts/ifcfg-&lt;iface_name&gt;` hasilnya sudah pasti sama dengan konfigurasi di atas.

Welcome to Cent OS 7 :smile: