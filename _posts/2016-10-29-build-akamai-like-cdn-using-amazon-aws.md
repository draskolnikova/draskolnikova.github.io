---
layout: post
title: Building AKAMAI-like CDN using Amazon AWS with minimal budget
categories: [work]
tags: [work, networking, amazon, cdn, akamai] 
---

Based on [Gartner Magic Quadrant (Aug) 2016](https://aws.amazon.com/resources/gartner-2016-mq-learn-more/), AWS still leader on cloud IaaS provider. And I've been using Amazon AWS since 2013 (S3 Bucket, Cloudfront, and Route53). The reason why I use that service only on Amazon AWS are, to optimize existing on premise infrastructure. The big challenge is to manage big assets (images) without downtime and data loss and of course using optimized resource.

According to [S3 SLA](https://aws.amazon.com/s3/sla/), I believe that at least I have only 0.01% downtime, it means only [approximately 43minutes](http://uptime.is/99.9) in a month. And what happen if the S3 bucket can't accessed in period time? I combine with Amazon CloudFront. CloudFront will cached anything that hosted by AmazonS3.

Actually, the design is looks like.

![AKAMAI-Like CDN]({{ site.url }}/assets/draskolnikova/2016-10-29-01.png)

I use S3 bucket to store all assets as origin, then distribute them through CloudFront. And I use Route53 as my DNS Servers, since route53 support geolocation and failover support, it will be nice. Cloudwatch? It's help me to monitor performance of cache server on Indonesia, if the cache server goes down or unreachable, it will be trigger route53 to remove it from dns record and switch to AWS CloudFront distribution as default images server (`static.xtremenitro.org`).

## Cost Calculation

It's the most important to do setup using Amazon AWS, miss configuration, **you'll be charged more expensive**. From the existing installation, I have.

### Amazon Simple Storage Service (S3)

I thought it's still the best storage service ever, because I have a lot of asset(s) there, approx 400GB++, it's only charged me $14.xx.

![Amazon Simple Storage Service]({{ site.url }}/assets/draskolnikova/2016-10-29-02.png)

### Amazon CloudFront 

Since CloudFront didn't have any node in Indonesia, build cache server in Indonesia (our visitor is 99% in Indonesia) will cut off your Amazon CloudFront billing usage.

![Amazon CloudFront]({{ site.url }}/assets/draskolnikova/2016-10-29-03.png)

### Amazon Route53

It's the key to save your resource, using Amazon Route53, and the feature of Geolocation and Failover combine with CloudWatch.

![Amazon Route53]({{ site.url }}/assets/draskolnikova/2016-10-29-04.png)

### VPS on Indonesia

The last one, to serve our visitor came from Indonesia, we need at least two VPS with 2 vCPU & 2 GB of RAM. Why we need this? We need lower latency to serve Indonesian Visitor. You can go through [JetdinoVM using Professional package](https://portal.jetdino.com/cart.php?gid=8) (this is non-paid article, seriously).

## Total Cost

- Amazon S3: $14.61
- Amazon CloudFront: $43.80
- Amazon Route53: $44.87
- VPS: $60

Total Cost: $163.28 (round up $165)

What if we use all of AWS Infrastructure? I've tried it couple months ago, it will charged you $450 / month. And still having some latency issue regarding internet access in Indonesia.

With this algorithm and specification, I can reduce the monthly cost and increase speed access to user upto 1-3 times. 

What about akamai? The title as I mentioned above. Since now, only Akamai that penetrate CDN in Indonesia, most CDN company nearby nodes is on Singapore. And what about the price? I have the price, but it's NDA, you can directly contact Akamai's Sales for more information. 
