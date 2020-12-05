---
layout: post
title: KADABRA - IDNIC Smart Routing Analyzer
categories: [work]
tags: [bird2, vyos, kadabra, idnic, indonesia] 
---

It's been 2 years ago I've updated my blog :( But thanks for anyone who's still reading this dead blog (lol). This post will tell about my latest project in last 2-3 year that involved many people and I hope useful to other people too.

Today, I'd like to write about my latest projects with [IDNIC](https://idnic.net/) called [KADABRA](https://idnic.net/blog/detail/resmi-kadabra-dapat-diakses-oleh-pengguna-publik/MTIx). KADABRA is smart routing analyzer platform and helps IDNIC or network administrator to analyze internet routing table, especially in Indonesia.

KADABRA v1 built at 2018 in collaboration with RIPE Atlas, [Bukalapak](https://www.bukalapak.com) and [IDNIC-APJII as sponsor](https://atlas.ripe.net/get-involved/community/#!sponsors). You can read the [paper about first KADABRA release](https://www.slideshare.net/draskolnikova/distributed-measurements-as-transparency). It's talk about transparency and simple network mitigation using traceroute and ping only. And we faced so many problems because of RIPE Atlas framework limitation and we didn't have any control access into RIPE backends.

The first development takes 1 year, since end of 2018 until end of 2019 and the result wasn't as expected due the probes limitation. 

## KADABRA v2

In early 2020, we tried to rebuild KADABRA from scratch and collecting several issue that we facing in KADABRA v1, especially at IDNIC. So, we decided to build KADABRA as a Smart Routing Analyzer to help IDNIC validates the data and helps the Indonesian Network Administrator debuging network problem in the internet.

KADABRA v2, born at Feb 2020 with several improvement and adjustment. KADABRA have several core values:
* Collecting and analyze all bgp attributes both ipv4 and ipv6.
* Marking RPKI with ROA Status (Valid, Invalid and Unknown for each IP Blocks)
* RIS (Routing information service) realtime stream with 5 minutes update interval Indonesian domestic routing information base
* It's track the growth of new IPv4, IPv6 and new ASN in daily estimate.
* ... and much more!

KADABRA built on top heterogen ecosystem that makes the application and infrastructure more modular and decentralized. 

## Infrastructure

All infrastructure platform distributed across multi-cloud (agnostics), with the following details :

- JETDINO Hosting - Kubernetes Platforms 
- Alibaba Cloud - Relational Database Management System
- Biznet GIO - Network Object Storage
- Cloudflare - Anycast Domain Name system
- Google Cloud Platform - Cloud-based message queueing

And all infrastructure above are hosted in Indonesia.

## Application

KADABRA Application built using open sources framework, with the following details :

- Go Programming Language (Backend)
- Laravel Framework (Frontend)
- PostgreSQL Database (Database)

## Statistics

Currently, [KADABRA](https://ris.kadabra.id) already record approx. 96K prefixes across 5 exchange in 4 cities. And we have processing power up to 33K request per second to analyze the prefix and create public statistics via [RAGNO](https://stats.kadabra.id).

People behind KADABRA is :
- Ardika Bagus as Senior Cloud & Kubernetes Engineer
- Indra Saputra as Senior Backend Engineer
- Fahri Ahmad Fadil as Application Engineer
- Lufti Rahadian as Senior System & Network Engineer
- Abdul Basit as Senior Database Architect
- Achmad Reyhan as Network Engineer Support
- Adi Nugroho as System Engineer Support

Special thanks to:
- Chief of IDNIC - Mr. Adi Kusuma
- Chief of APJII - Mr. Jamalul Izza
- Chief of PT. APIK Media Inovasi - Mr. Azhari Ahmad
... and all people behind this project, especially it's you! Yes you!

If you have any question or feature request regarding KADABRA, you can send an email to abra [at] kadabra [dot] id or you can directly mention me at twitter [@draskolnikova](https://twitter.com/draskolnikova)