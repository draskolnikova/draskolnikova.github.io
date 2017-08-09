---
layout: post
title: Fixing VLC flickering on AMD Graphic Cards
categories: [personal]
tags: [linux, fedora, vlc, amd radeon] 
---

Couple days ago, I've been update my Fedora Core 25 to latest Fedora Core 26. Got some adjustment and improvement there, also got new (bugs?) errors regarding my dedicated graphic cards. From `lspci` reports, I am using `Radeon HD 8670A/8670M/8690M / R5 M330 / M430` graphic cards on Lenovo G40-80 notebook.

```
$ sudo lspci -v | grep AMD
04:00.0 Display controller: Advanced Micro Devices, Inc. [AMD/ATI] Sun XT [Radeon HD 8670A/8670M/8690M / R5 M330 / M430] (rev 83)
```

When I launch VLC, the video was flickering like this.

![Flickering while using AMD Dedicated Graphic Cards]({{ site.url }}/assets/draskolnikova/2017-08-09-flicker-1.png)

And I search for the clue, why this VLC flicker when I start it using dedicated graphic cards, I found it [there](https://www.phoronix.com/forums/forum/linux-graphics-x-org-drivers/amd-linux/5374-trick-to-prevent-flickering-in-vlc).

Trying to check default VLC Configuration, it was set to automatic.

![Automatic preferences]({{ site.url }}/assets/draskolnikova/2017-08-09-preferences-1.png)

Then, I was trying set it to `VA-API video decoder via DRM`.

![VA-API video decord via DRM]({{ site.url }}/assets/draskolnikova/2017-08-09-preferences-2.png)

Don't forget to save, apply and restart your VLC. 

Test again my latest configuration, and voila!

![VLC was fixed from flickered screen]({{ site.url }}/assets/draskolnikova/2017-08-09-flicker-2.png).

I hope it helps if you got same problem like me.
