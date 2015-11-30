---
layout: post
title: NGINX Amplify - Monitoring & Config Assistance
categories: [work]
tags: [work, linux, webserver]
---

Beberapa hari yang lalu saya mendapatkan notifikasi dari NGINX mengenai launching product baru yang masih private beta. Apa itu NGINX Amplify? Di ambil dari [halaman resmi](https://github.com/nginxinc/nginx-amplify-doc/blob/master/amplify-faq.md#11-what-is-nginx-amplify)-nya, "**NGINX Amplify is a tool designed to monitor and optimize application delivery infrastructure. With Amplify it becomes possible to proactively analyze and fix problems related to running and scaling modern web applications**".

You can use NGINX Amplify to do the following:
<ul>
<li>Visualize and identify performance bottlenecks, overloaded servers, or potential DDoS attacks</li>
<li>Improve and optimize NGINX performance with intelligent advice and recommendations</li>
<li>Get notified when something is wrong with the application delivery</li>
<li>Plan web application capacity and performance</li>
<li>Keep track of the systems running NGINX</li>
</ul>

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/hashtag/NGINX?src=hash">#NGINX</a> Amplify is coming soon! Request early access to our new monitoring &amp; configuration help tool. Get started: <a href="https://t.co/Rx5eqdfr82">https://t.co/Rx5eqdfr82</a></p>&mdash; NGINX, Inc. (@nginx) <a href="https://twitter.com/nginx/status/661923887014244352">November 4, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

## First Impression

Adalah hal yang besar bagi NGINX untuk memberikan akses gratis *monitoring & configuration assistance platform*. Paling tidak, ini memudahkan pekerjaan saya untuk melakukan *first initialization* dan optimasi dasar *application server*. Bisa terlihat pada halaman *dashboard* berikut ini

![NGINX Amplify]({{ site.url }}/assets/draskolnikova/nginx-amplify-beta.png)

NGINX Amplify melakukan pengujian pada semua konfigurasi yang di-*load* pada file `nginx.conf`

## SSL Certificate Checking

Fitur ini yang belum bisa saya coba, tapi kalau dari dugaan saya, Amplify akan melakukan pengujian `ssl_cipher` (dan merekomendasikan yang harusnya digunakan maupun tidak). 

Saat ini, versi NGINX Amplify yang saya gunakan adalah `amplify-agent-0.24-2`.

## Overview Features

NGINX Amplify 0.24 mempunyai 4 fitur utama yaitu, Graph, Reports, Alerts dan Events. 

### Graph

Sesuai namanya, semua data yang masuk akan di visualisasikan menjadi bentuk grafik, supaya bisa terbaca dengan mudah.

### Reports

Reports meliputi system config dan nginx config seperti penjelasan di atas, akan ada rekomendasi apabila konfigurasi tidak sesuai atau ada yang harus di optimalisasi lagi.

### Events

Halaman ini dibagi menjadi 2 kategori, **local events** dan **global events**. Untuk local events sendiri adalah memastikan bahwa semua files yang di *load* bisa terbaca dan tertulis dengan normal.

Sedangkan global events sendiri berfungsi untuk *tracking update* dari `nginx-amplify-agent`

### Alerts

Yang saya suka adalah bagian ini, kita juga bisa mengetahui beberapa parameter yang selama ini mungkin belum pernah di monitor. Seperti `nginx.http.request.failed`, `nginx.http.conn.dropped`, `nginx.http.status.4xx`, `nginx.http.status.5xx` dan masih banyak parameter lainnya yang tersedia di *dashboard*. 

Ini berguna buat saya, karena untuk melakukan monitoring multi-nodes akan sangat susah kalau *monitoring tools*-nya tidak fleksibel dan well-informed.

Jadi, sudahkah Anda mendapatkan invitation ke NGINX Amplify? :smile: