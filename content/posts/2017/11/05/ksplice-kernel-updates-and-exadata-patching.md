---
title: 'ksplice kernel updates and Exadata patching'
date: '2017-11-05T23:02:05+05:30'
status: publish
permalink: /2017/11/05/ksplice-kernel-updates-and-exadata-patching
author: Sidhu
excerpt: ''
type: post
id: 574
category:
    - Exadata
tag:
    - Exadata
    - ksplice
    - Patching
post_format: []
---
If you have installed some one off ksplice fix for kernel on Exadata, remember to uninstall it before you do a kernel upgrade eg regular Exadata patching. As such fixes are kernel version specific so they may not work with the newer version of the kernel.