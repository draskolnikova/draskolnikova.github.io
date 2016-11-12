---
layout: post
title: Taking network monitoring to the next level
categories: [work]
tags: [work, networking, atlas probe] 
---

As system engineer and part-time network engineer, I am responsible to make sure all system are running and the network properly connected (24/7/365).Now, I will write about a project came from RIPE NCC, called [RIPE Atlas](https://atlas.ripe.net/about/). RIPE Atlas is a global network of probes that measure Internet connectivity and reachability, providing an unprecedented understanding of the state of the Internet in real time.

Currently, I host 2 probes (the device from RIPE is called probe), 1 public probe [#25259](https://atlas.ripe.net/probes/25259/) and 1 private probe #25577. My public probe have dual stack network configuration, both IPv4 and IPv6 are enabled. You can create custom measurement, for example I have infrastructure(s) on Amsterdam, and have FQDN `speedtest.ams01.softlayer.com`, then I create [public measurement](https://atlas.ripe.net/measurements/4434917/#!probes) to monitoring my network route.

From that measurement, I can track down the history of my network route, whenever it was down or reroute to another link. It's help me so much to maintain the SLA from upstream. The probe have another capability to monitoring, available feature is `PING`, `TRACEROUTE`, `DNS`, `SSL`, `HTTP`, and `NTP`. And you can select specific probe(s) to fulfill your monitoring needs.

This project is FREE\*, you only need to host the probe online. The atlas probe includes: 1xTP-Link MR3020 (Modified F/W), 1xUTP Cable, 1xMicroUSB Cable, 1x8GB USB Disk. Interest to join the project? Feel free to reach me out on twitter, or you can contact directly to Atlas Probe Ambasaddor at Indonesia using [Twitter](https://twitter.com/budiwijaya) or [Facebook](https://www.facebook.com/budiwijaya) (Mr. Budiwijaya).

Happy probing!
