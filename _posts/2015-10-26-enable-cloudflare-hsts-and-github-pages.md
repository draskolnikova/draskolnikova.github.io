---
layout: post
title: Enable Cloudflare HSTS and Github Pages
categories: [technology]
tags: [work, technology, tls]
draft: true
---

Dewasa ini, sudah banyak layanan SSL Termination yang bisa digunakan, salah satunya adalah [Cloudflare Universal SSL](https://blog.cloudflare.com/introducing-universal-ssl/). Saat ini, `log.xtremenitro.org` menggunakan layanan tersebut secara cuma-cuma. Saat artikel ini di tulis, layanan tersebut masih disediakan gratis bagi pengguna Cloudflare <i>(free/paid)</i>.

Dan, untuk meningkatkan fleksibilitas menulis, saya sedang dalam proses memindahkan semua tulisan saya dari `xtremenitro.org` ke `log.xtremenitro.org` dengan beberapa filter <strike>(engga semua tulisan saya pindahkan)</strike>. Halaman ini di host langsung menggunakan [Github Pages](https://pages.github.com/) with Custom Domain.

Karena Github Pages sudah menggunakan SSL, jadi sekalian saya menggunakan konfigurasi [HSTS (HTTP Strict Transport Security)](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security). 

Berikut ini hasil uji coba menggunakan Cloudflare Universal SSL with HSTS + Github Pages.
![Quallys SSL Labs]({{ site.url }}/assets/draskolnikova/log.xtremenitro.org-ssl.jpg)

Happy blogging with Github :+1: