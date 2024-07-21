---
title: 'VirtualBox and Windows driver verifier'
date: '2014-12-03T16:04:56+05:30'
status: publish
permalink: /2014/12/03/virtualbox-and-windows-driver-verifier
author: Sidhu
excerpt: ''
type: post
id: 436
category:
    - Virtualization
tag:
    - VirtualBox
    - Virtualization
    - Windows
post_format: []
wpsd_autopost:
    - '1'
---
I was troubleshooting some Windows hangs on my Desktop system running Windows 8 and enabled driver verifier. Today when I tried to start VirtualBox it failed with this error message.

> Failed to load VMMR0.r0 (VERR\_LDR\_MISMATCH\_NATIVE)

Most of the online forums were asking to reinstall VirtualBox to fix the issue. But [one of the thread](http://stackoverflow.com/questions/21654596/suddenly-getting-failed-to-load-vmmr0-r0-verr-ldr-mismatch-native-in-virtual) mentioned that it was being caused by Windows Driver Verifier. I disabled it, restarted Windows and VirtualBox worked like a charm. Didn’t have time to do more research as i quickly wanted to test something. May be we can skip some particular stuff from Driver Verifier and VirutalBox can then work.