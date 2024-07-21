---
title: 'ZDLRA patching'
date: '2021-03-12T22:30:52+05:30'
status: publish
permalink: /2021/03/12/zdlra-patching
author: Sidhu
excerpt: ''
type: post
id: 803
category:
    - ZDLRA
tag:
    - Patching
    - zdlra
post_format: []
---
To be honest, Fernando Simon has already documented all the steps needed in ZDLRA patching . So this post is more like a reference post for me and it points to the links on [his blog](http://www.fernandosimon.com/blog/). One thing he could change though are the post titles. He also agrees 😉

<figure class="wp-block-embed is-type-rich is-provider-twitter wp-block-embed-twitter"><div class="wp-block-embed__wrapper">> Yeap, saw that. Titles are bit confusing 🙂
> 
> — Amardeep Sidhu (@amardeep\_sidhu) [March 12, 2021](https://twitter.com/amardeep_sidhu/status/1370304085245661192?ref_src=twsrc%5Etfw)

<script async="" charset="utf-8" src="https://platform.twitter.com/widgets.js"></script></div></figure>ZDLRA patching is broadly divided into two parts. First part is where you patch the RA library and Grid &amp; DB homes. Second part includes compute node &amp; storage cell image patch and patches for IB/RoCE switches. Second part is exactly similar to Exadata except that it is bit restricted in terms of image versions that you can use. Only the versions that are certified for ZDLRA can be used. Also the RA library version and the Exadata image version should be compatible with each other. So if you are planning to patch only one part; RA library or the image, make sure that both the components stay compatible. The MOS note that has all these details is 1927416.1. This note should be the first place to go when you are planning to patch a ZDLRA. The steps for upgrade/patch, image patching are given in MOS note 2028931.1. There is another note 2639262.1 that discusses some of the known issues that you may face while doing the patching. It is important to review all three notes before you plan to patch.

The RA library patching part can be considered of two different types. This is an important difference. Make sure that you follow the right set of commands. When you are jumping between major versions say going from 12.x to 19.x, it is called an upgrade and the commands are like racli upgrade appliance –step=1. Fernando talks about this in detail in [this post](https://www.fernandosimon.com/blog/zdlra-patch-the-recovery-appliance/).

On the other hand, when you are not jumping between versions; say going from 19.x to 19.x only, it is called patching and the commands are like racli patch appliance –step=1. Fernando has discussed this in detail in [this post](https://www.fernandosimon.com/blog/zdlra-patch-update-the-recovery-appliance/).

The Exadata bit (image &amp; switches patching) of it is exactly the same as we do in Exadata. Fernando talks about this in [this post](https://www.fernandosimon.com/blog/exadata-and-zdlra-patch-exadata-stack/).

The RA library patching bit is pretty much automated and works fine most of the time. If you hit an issue, you may find the solution/workaround documented in one of the MOS notes.

Happy patching !