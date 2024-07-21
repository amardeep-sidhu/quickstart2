---
title: 'expdp not consistent'
date: '2011-11-04T15:10:26+05:30'
status: publish
permalink: /2011/11/04/expdp-not-consistent
author: Sidhu
excerpt: ''
type: post
id: 368
category:
    - Troubleshooting
tag:
    - expdp
post_format: []
aktt_notify_twitter:
    - 'yes'
wpsd_autopost:
    - '1'
aktt_tweeted:
    - '1'
---
Came across this small oddity that documentation of 10.2 and 11.2 states that expdp by default takes consistent image of the database. But actually it is not so. You need to use flashback\_scn/flashback\_time for that. Metalink doc 377218.1 explains the scenario.