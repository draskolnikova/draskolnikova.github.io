---
layout: post
title: Creating optimized qcow2 files
categories: [work]
tags: [work]
---

Standard and default images create using [virt-manager](https://virt-manager.org/) on linux, automatically full allocating specified disk size to host. To avoid un-usable disk space, you should avoid choosing default images created by virt-manager. 

->![Creating VM's disk using Virt Manager]({{ site.url }}/assets/draskolnikova/2016-10-12-01.png)<-

To create growing images files for VM's, I choose [qcow2](http://www.linux-kvm.org/page/Qcow2). Why growing images? To ensure the host only allocated how much diskspace to harddrive (fill-as-you-go), not how big the disk was allocated. For example :

```
$ sudo qemu-img create -f qcow2 phantom01.xtremenitro.org-lvm-f.qcow2 100G
Formatting 'phantom01.xtremenitro.org-lvm-f.qcow2', fmt=qcow2 size=107374182400 encryption=off cluster_size=65536 lazy_refcounts=off
```

It should be create qcow2 files and have following information:

```
$ sudo qemu-img info phantom01.xtremenitro.org-lvm-f.qcow2
image: phantom01.xtremenitro.org-lvm-f.qcow2
file format: qcow2
virtual size: 100G (107374182400 bytes)
disk size: 196K
cluster_size: 65536
Format specific information:
    compat: 1.1
    lazy refcounts: false
```

See at `disk size`, we have only 196K on first init. For sure, let's check it.

```
$ ls -lah phantom01.xtremenitro.org-lvm-f.qcow2
-rw-r--r--. 1 dmnQ dmnQ 194K Oct 13 03:35 phantom01.xtremenitro.org-lvm-f.qcow2
```

And it will save your disk space as much as you want, and will growth as the data growth. Then after creating the disk file, you need to load it on virt-manager by choosing `Select or create custom storage`.

->![Select or create custom storage]({{ site.url }}/assets/draskolnikova/2016-10-12-02.png)<-

Done, now you have optimized qcow2 files and growth as the data growth.
