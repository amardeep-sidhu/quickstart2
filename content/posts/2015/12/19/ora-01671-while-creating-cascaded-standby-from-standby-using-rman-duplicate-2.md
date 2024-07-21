---
title: 'ORA-01671 while creating cascaded standby from standby using RMAN DUPLICATE'
date: '2015-12-19T20:50:17+05:30'
status: publish
permalink: /2015/12/19/ora-01671-while-creating-cascaded-standby-from-standby-using-rman-duplicate-2
author: Sidhu
excerpt: ''
type: post
id: 467
category:
    - RMAN
tag:
    - RMAN
    - Standby
post_format: []
wpsd_autopost:
    - '1'
---
On a T5 Super Cluster (running 11.2.0.3) I was creating a cascaded standby from an already functional standby using RMAN DUPLICATE and it errored out with

> ```
> ORA-01671: control file is a backup, cannot make a standby control file
> ```

A quick search reveals that it is bug 11715084 that affects most of the 11.x versions except 11.2.0.4. There is a one off patch available for most of the versions or one can install the bundle patch that includes the fix for this patch. I applied BP26 and it worked fine after that.