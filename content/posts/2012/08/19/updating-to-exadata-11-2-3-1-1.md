---
title: 'Updating to Exadata 11.2.3.1.1'
date: '2012-08-19T22:37:41+05:30'
status: publish
permalink: /2012/08/19/updating-to-exadata-11-2-3-1-1
author: Sidhu
excerpt: ''
type: post
id: 420
category:
    - Exadata
tag:
    - 11.2.3.1.1
    - Exadata
    - Patching
post_format: []
aktt_notify_twitter:
    - 'yes'
aktt_tweeted:
    - '1'
---
Just a quick note about change in the way the compute nodes are patched starting from version 11.2.3.1.1. For earlier versions Oracle provided the minimal pack for patching the compute nodes. Starting with version 11.2.3.1.1 Oracle has discontinued the minimal pack and the updates to compute nodes are done via Unbreakable Linux Network (ULN).

Now there are three ways to update the compute nodes:

1\) You have internet access on the Compute nodes. In this case you can download patch **13741363**, complete the one time setup and start the update.

2\) In case you don’t have internet access on the Compute nodes you can choose some intermediate system (that has internet access) to create a local repository and then point the Compute nodes to this system to install the updates.

3\) Oracle will also provide all the future updates via an downloadable ISO image file (patch 14245540 for 11.2.3.1.1). You can download that ISO image file, mount it on some local system and point the compute nodes to this system for updating the rpms (the readme has all the details on how to do this).

Some useful links:

[https://blogs.oracle.com/XPSONHA/entry/updating\_exadata\_compute\_nodes\_using](https://blogs.oracle.com/XPSONHA/entry/updating_exadata_compute_nodes_using)

[https://blogs.oracle.com/XPSONHA/entry/new\_channels\_for\_exadata\_11](https://blogs.oracle.com/XPSONHA/entry/new_channels_for_exadata_11)

Metalink note **1466459.1**