---
title: 'Smokescreen detects traffic from an Exadata VM'
date: '2022-03-21T19:30:16+05:30'
status: publish
permalink: /2022/03/21/smokescreen-detects-traffic-from-an-exadata-vm
author: Sidhu
excerpt: ''
type: post
id: 1206
category:
    - Exadata
    - Unix/Linux
tag:
    - Database
    - Exadata
    - KVM
    - Smokescreen
post_format: []
---
A customer who is using an Exadata X8M-2 with multiple VMs had Smokescreen deployed in their company recently and they reported an issue that one of the Smokescreen decoy servers in their DC was seeing traffic from one of the Exadata VMs on a certain port. That was rather confusing as that port was the database listener port on that VM and why would a VM with Oracle RAC deployed try to access any random IP on the listener port. Also it was happening only for this VM. Nothing for so many other VMs.

We were just looking at the things and my colleague said that he had seen this IP somewhere and he started looking through the emails. In a minute, we found the issue as he found this IP mentioned in one of the emails. This was the VIP of this VM from where the traffic was reported to be originating. While reserving IPs for Smokescreen decoy servers, someone made the mess and re-used the IP that was already used for one of the VIPs of this RAC system !