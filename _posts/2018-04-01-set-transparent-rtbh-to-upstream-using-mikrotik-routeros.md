---
layout: post
title: Set transparent RTBH to upstream using MikroTik RouterOS 
categories: [work]
tags: [mikrotik, routeros, bgp, rtbh] 
---

Couple weeks ago, there was [memcrashed exist in the wild](https://blog.cloudflare.com/memcrashed-major-amplification-attacks-from-port-11211/) and it's hurt my router so hard. It was attack my router up to 1GBps, fortunately my router doesn't hung. LoL.

![Memcrashed]({{ site.url }}/assets/draskolnikova/memcrashed.png)

## How to mitigate this issue from client side?

There's several options to resolve this issue, you can drop it using firewall or send your ddos'ed IP Address to blackhole. To save your router(s) resource, it's simply send your ddos'ed IP to upstream blackhole.

Then what happen if you're a service provider and using MikroTik RouterOS? You need to pass your internal bgp communities to upstream bgp communities.

## How to passing internal blackhole communities to upstream blackhole communities?

For example, my client has ddos'ed IP `192.168.10.10`, my blackhole community is `[AS:6969]`, my upstream blackhole community is `[UPSTREAM:999]` and client ASN is `AS65666`. 

*TL;DR*

(`AS65666`) => (`AS65555`) => (`AS65444`)

`AS65666` = Client ASN
`AS65555` = Service Provider
`AS65444` = Upstream Provider

From client point of view (`AS65666`), they need to blackhole and mark IP `192.168.10.10` disappear from internet, so they need send blackhole community to (`AS65555`) and pass it to (`AS65444`)

From `AS65666` point of view:
```
/rou filter
chain=transit-out-AS65555 prefix=192.168.10.10 prefix-length=32 invert-match=no action=accept set-bgp-prepend-path="" \
     set-bgp-communities=65555:6969 append-bgp-communities=""
     
/rou bgp network
add network=192.168.10.10/32 synchronize=no
```

`AS65666` Advertisement
```
[client@as65666] > /routing bgp advertisements print peer-as65555
PEER         PREFIX               NEXTHOP          AS-PATH    ORIGIN     LOCAL-PREF
peer-as65555 192.168.10.10/32     172.16.66.2                 igp
peer-as65555 192.168.10.0/24      172.16.66.2                 igp
```

From `AS65555` point of view:
```
/rou filter
add action=accept bgp-communities=65555:6969 chain=transit-out-65444 comment=RTBH set-bgp-communities=65444:9999
add action=accept bgp-communities=65555:6969 chain=transit-in-65666 prefix-length=29-32
```

Prefix length in AS65555 is how long prefix we accept for blackhole community.

`AS65555` Advertisement
```
[dewangga@as65555] > /routing bgp advertisements print peer-as65444
PEER         PREFIX               NEXTHOP          AS-PATH    ORIGIN     LOCAL-PREF
peer-as65444 192.168.10.10/32     172.16.20.2      AS65666    igp
peer-as65444 192.168.10.0/24      172.16.20.2      AS65666    igp
```

We can see advertised prefix above, we found `192.168.10.10` should be blackholed and sent to `AS65444` as upstream provider. It's just a simple case, and you can improve using sflow or any opensource tools. 
