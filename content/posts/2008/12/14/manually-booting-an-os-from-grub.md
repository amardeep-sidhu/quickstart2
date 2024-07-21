---
title: 'Manually booting an OS from GRUB'
date: '2008-12-14T19:28:25+05:30'
status: publish
permalink: /2008/12/14/manually-booting-an-os-from-grub
author: Sidhu
excerpt: ''
type: post
id: 143
category:
    - Unix/Linux
    - Windows
tag:
    - GRUB
    - Linux
    - Windows
post_format: []
aktt_notify_twitter:
    - 'yes'
aktt_tweeted:
    - '1'
---
One of my friend today asked me about removing Linux partitions &amp; GRUB (from a dual boot system) and return back to windows alone. Removing Linux involves just formatting/removing the partitions. Now to remove GRUB either do **fdisk /mbr** from a Windows 98 bootable CD or do **fixmbr** after booting into repair mode with Windows XP CD. But if you have none then to remove GRUB you will need some utility like [this one](http://www.ambience.sk/fdisk-master-boot-record-windows-linux-lilo-fixmbr.php) and if you reboot before doing that it might make GRUB unable to boot into Windows. It will get stuck at GRUB&gt; prompt only. So there is an option: to manually boot the OS you want (ie Windows). A quick search gave link to [this thread](http://www.ntcompatible.com/How_to_remove_GRUB_loader_t28242.html#150012). It involves few commands on the GRUB prompt:

```
<pre class="brush: css; title: ; notranslate" title="">grub> rootnoverify (hd0,0)
grub> makeactive
grub> chainloader +1
grub> boot
```

It will load the NTLDR where your Windows is installed in Partition 1 on HDD 1.