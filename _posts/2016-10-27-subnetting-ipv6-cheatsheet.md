---
layout: post
title: Subnetting IPv6 Cheatsheet
categories: [work]
tags: [work, networking] 
---

Since IPv6 hype was launch several years ago, many people (incl. me) were struggling how to split IPv6 subnet correctly. Honestly, I wasn't network engineer, but learning network fundamental is not a big deal (especially IPv6, nowadays, it's a new protocol!). Since it was launch at 1990 (former IPNG), based on APNIC Labs Survey, the adoption of native IPv6 [still low](https://stats.labs.apnic.net/ipv6/).

The fundamental of TCP/IP is how to correctly split the subnet and know the routing works. At July 25th, I was attend the [Advance IPv6 Routing Workshop by APNIC](https://www.idnog.or.id/en/workshop/3) and learn with many senior engineers right there. The most interesting part is, I know how to split IPv6 subnet correctly without using any IPv6 calculator. I'll write down how to do it for self notes, and I hope it is usefull for you too.


## Subnetting

Prefix IPv6: `2404:6800::/32` (Google IPv6 Block Asia Pacific)

We must know that IPv6 have 128-bit address, separated by colon (`:`) and have 8 (eight) groups, then IPv4 only have 32-bit address, separated by dots (`.`) and only have 4 (four) groups.

So, if the rest IPv6 have 128-bit address, then each group should have 16-bit.

Shortest IPv6 Prefix: `2404:6800::/32` 128-bit addresses

Longest IPv6 Prefix: `2404:6800:0000:0000:0000:0000:0000:0000` 128-bit addresses

Then if I want split it into small subnets (eg. `/48`), how? And which first prefix should be?

For the example, we will split IPv6 Prefix `2404:6800::/32` into `/48`, then found the 48-bit.

``` 
2404:6800:0000:0000:0000:0000:0000:0000  -> 128-bit total
 16   16   16   16   16   16   16   16   -> 128-bit total (from 16-bit x 8 block)
```

> Please keep in mind, IPv6 is a hexadecimal. So it will start counting from `0` to `f`
>
> Then, in every member in a group(s) have 4-bit address [`0`,`1`,`2`,`3`,`4`,`5`,`6`,`7`,`8`,`9`,`a`,`b`,`c`,`d`,`e`,`f`]

How we found that 48-bit address from `2404:6800::/32` ?

So we must find the 48th bit from `2404:6800::/32`, then we need 16-bit left.

#### Diagram
```
  [4]
0 0 0 0 -> each number represent 1-bit operation(s)
| | | |
| | | +---- 0 1 2 3 etc (2^0)
| | +------ 0 2 4 6 etc (2^1)
| +-------- 0 4 8 c etc (2^2)
+---------- 0 8 0 8 etc (2^3)
```

`2404` on first group, have first 16-bit address.

`6800` on second group, have second 16-bit address, so total bit are 32-bit right now.

`0000` on third group, have third 16-bit address, so total bit are 48-bit right now.

Voila! We've found the 48th bit. The 48th bit are on third group, and first IPv6 is `2404:6800:0::/48` on `/48` prefix.

Then another case, how we found `/33` prefix? You can use the referrence [diagram](#diagram) above and let's find out.

IPv6 on /32 Subnet: 2404:6800:0000:0000:0000:0000:0000:0000

IPv6 on /33 Subnet: 2404:6800:`0`000:0000:0000:0000:0000:0000

See the marked zero `0` on `/33` subnet, it's the hint to find the subnet, and crosscheck with the [diagram](#diagram). So the `/33` prefix should be `2404:6800:0::/33` and `2404:6800:8::/33`.

Hope it helps!
