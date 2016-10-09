---
layout: post
title: Google Brotli Compression [Theory & Implementation]
categories: [work]
tags: [work, personal]
---

Brotli is a generic-purpose lossless compression algorithm that compresses data using a combination of a modern variant of the LZ77 algorithm, Huffman coding and 2nd order context modeling, with a compression ratio comparable to the best currently available general-purpose compression methods. It is similar in speed with deflate but offers more dense compression.

Since September 2015, google was [announced](https://opensource.googleblog.com/2015/09/introducing-brotli-new-compression.html) their new compression algorithm (called Brotli). Even it was a new compression, [most modern browser](https://en.wikipedia.org/wiki/Brotli#Browser_Support) was support it. 

## Why brotli was awesome?

Like gzip (well-known content-encoding), brotli was awesome (at the moment) and [cloudflare](https://blog.cloudflare.com/results-experimenting-brotli/) was experiment with it. Based on personal test, brotli was optimized on dynamic content and specific mime-types (eg. text/html, application/json, and another dynamic content), but brotli still support another types (since they use `brotli_types *`)

Static files (eg. jpeg, jpg, png, webp, gif, etc) aren't optimized (maybe, not yet optimized by brotli?).

## Testbed

### Server Hardware

- 1 x Intel x3430
- 4 x 4GB DDR3 Memory
- 4 x 76GB SCSI/SAS Harddrive (RAID-10)

### Server Software

- Linux Cent OS 7 x86_64
- Nginx 1.11.4 + [ngx_brotli module](https://github.com/google/ngx_brotli)
- [google/brotli](https://github.com/google/brotli) library

### Client Hardware

- Intel(R) Core(TM) i5-4200M
- 2 x 4GB DDR3 Memory
- 1 x 500GB 5400RPM SATA

### Client Software

- Google Chrome Version 53.0.2785.143 (64-bit)

### Test Method

gzip compression level: 9/9 (default 1)
brotli compression level: 9/11 (default 6)

Default Landing Pages Nginx (612bytes)
with gzip:   `518 bytes`
with brotli: `426 bytes`

Dummy json file (9,5KBytes)
with gzip:   `2,9Kbytes` 
with brotli: `2,8Kbytes`

## Result

Brotli compression do better than gzip (on max compression level), but since brotli isn't yet RFC Standards (still draft), I would not recommend deploy it on production state.

Interesting to join the projects? :-)
