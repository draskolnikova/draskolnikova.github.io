---
layout: post
title: How to deploy and migrating to IPv6
categories: [work]
tags: [work, networking, ipv6] 
---

Since I [(accidentally)](https://log.xtremenitro.org/work/2016/11/12/taking-network-monitoring-to-the-next-level.html) work as Network Administrator, I always curios with IPv6 and how to deliver it to customer (especially my own device). But please keep in mind, you need up-to-date firmware and network device (router, switch, wireless radio(s)) and another equipment to do this.

In my lab and production environment, RouterOS firmware and MikroTik device(s) are still capable to do this transition. Why? It's cheap, easy to configure and "rock-solid". So, my setup actually like this :

* 1x MikroTik RB850Gx2 (tested on RoS 6.39.2)
* 1x HP v1920-24G (Managed)
* 4x HP v1410-2G (Unmanaged)
* 1x TP-Link TL-SF1048 (Unmanaged)
* 2x UAP AC Pro

As you can see, looks like this :
![Deployment Topology]({{ site.url }}/assets/draskolnikova/2017-08-07-ipv6.png)

The goals is to reduce my routing policy because of NAT (with simple routing, I can easily reach client device than using NAT). It's can be solved using IPv4 too, but we already know, how IPv4 were exhausted. 

## How to deploy it?
### Configuration on Router MikroTik RB850Gx2

Assume that you already have `/64` prefix from your ISP to do Router Advertisement (RA).

For example, I use this prefix `2400:ff:ec00:dead::/64` and `ether3` is interface point to `HP v1920`. (please review and change the configuration mathced on your side)

```
[admin@MikroTik] > /ipv6 address add address=2400:ff:ec00:dead:: advertise=no interface=ether3
[admin@MikroTik] > /ipv6 nd
set [ find default=yes ] disabled=yes
add advertise-mac-address=no interface=ether3 managed-address-configuration=yes mtu=1500 other-configuration=yes reachable-time=10s \
    retransmit-interval=5s
/ipv6 nd prefix
add interface=ether3 prefix=2001:df2:cc00:dead::/64
/ipv6 nd prefix default
set autonomous=no
```

Interesting point there, if you are using RouterOS too and have `/127` point-to-point links from your upstream or ISP, then your ISP(s) using MikroTik too, you need to add the default route using local-links. 

```
/ipv6 route
add !bgp-as-path !bgp-atomic-aggregate !bgp-communities !bgp-local-pref !bgp-med !bgp-origin !bgp-prepend check-gateway=ping distance=1 gateway=\
    fe80::e68d:8cff:fe3f:6731%ether5 !route-tag
```

Don't know why, but it works. 

Since now, you can check in your `/ipv6 neigh`, you should get advertised prefix from your client.
Cheers and welcome to IPv6!
