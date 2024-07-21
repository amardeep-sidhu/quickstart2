---
title: 'OneCommand Step 1 error'
date: '2017-04-10T22:20:41+05:30'
status: publish
permalink: /2017/04/10/onecommand-step-1-error
author: Sidhu
excerpt: ''
type: post
id: 538
category:
    - Exadata
tag:
    - Exadata
    - OneCommand
post_format: []
---
Hit this silly issue while doing an Exadata deployment for a customer. Step 1 was giving the following error:

<font face="Courier New">ERROR: 192.168.99.102 configured on dm01celadm01.example.com as dm01dbadm02 does not match expected value dm01dbadm02.example.com</font>

I wasn’t able to make sense of it for quite some time until a colleague pointed out that the reverse lookup entries should be done for FQDN only. As it is clear in the above message reverse lookup of the IP 192.168.99.102 returns dm01dbadm02 instead of dm01dbadm02.example.com. Fixing this in DNS resolved the issue.

Actually the customer had done reverse lookup entries for both the hostname and FQDN. As the DNS can return the results in any order, so the error message was bit random. Whenever the the hostname was returned first, Step 1 gave an error. But when the FQDN was the first thing returned, there was no error in Step 1 for that IP.