---
title: 'Why Linux cries about &quot;1024 cylinders thing&quot; at the time of installation&#8230;'
date: '2007-08-09T21:29:00+05:30'
status: publish
permalink: /2007/08/09/why-linux-cries-about-1024-cylinders-thing-at-the-time-of-installation
author: Sidhu
excerpt: ''
type: post
id: 32
category:
    - Unix/Linux
tag: []
post_format: []
blogger_blog:
    - amardeepsidhu.blogspot.com
blogger_author:
    - Sidhu
blogger_permalink:
    - /2007/08/why-linux-cries-about-1024-cylinders.html
---
I have been installing Linux for last 6 years and for more than half the number of times, came across a message something like “This partitions is beyond the 1024 cylinder boundary and may not be bootable”. But never cared for it much and understood what exactly it meant to say ?

Yesterday I was reading [System Admin guide to Linux](http://tldp.org/LDP/sag/html/index.html) by Lars Wirzenius (Thanks [Howard](http://www.dizwell.com/prod/node/675) for the link 🙂 From there I came to know what exactly that message meant. Quoting from the guide itself:

<span style="font-style: italic">Unfortunately, the BIOS has a design limitation, which makes it impossible to specify a track number that is larger than 1024 in the CMOS RAM, which is too little for a large hard disk. <span style="color: #3333ff">To overcome this, the hard disk controller lies about the geometry, and translates the addresses given by the computer into something that fits reality.</span> For example, a hard disk might have 8 heads, 2048 tracks, and 35 sectors per track. Its controller could lie to the computer and claim that it has 16 heads, 1024 tracks, and 35 sectors per track, thus not exceeding the limit on tracks, and translates the address that the computer gives it by halving the head number, and doubling the track number. The mathematics can be more complicated in reality, because the numbers are not as nice as here (but again, the details are not relevant for understanding the principle). This translation distorts the operating system’s view of how the disk is organized, thus making it impractical to use the all-data-on-one-cylinder trick to boost performance.</span>  
<span style="font-style: italic">.</span>  
<span style="font-style: italic">.</span>  
<span style="font-style: italic">.</span>  
<span style="font-style: italic">When using IDE disks, the boot partition (the partition with the bootable kernel image files) must be completely within the first 1024 cylinders. This is because the disk is used via the BIOS during boot (before the system goes into protected mode), and BIOS can’t handle more than 1024 cylinders. It is sometimes possible to use a boot partition that is only partly within the first 1024 cylinders. This works as long as all the files that are read with the BIOS are within the first 1024 cylinders. Since this is difficult to arrange, it is a very bad idea to do it; you never know when a kernel update or disk defragmentation will result in an unbootable system. Therefore, make sure your boot partition is completely within the first 1024 cylinders.</span>

Hope it clears the logic why Linux cries about 1024 cylinder issue at the time of installation.

You can read the guide online from the link above and download the pdf [here](http://tldp.org/LDP/sag/sag.pdf). Its simple and concise and just too good. Small thing covering much 🙂

Sidhu  
<span style="font-style: italic">  
</span>