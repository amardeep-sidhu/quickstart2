---
title: 'TNS-12543: TNS:destination host unreachable'
date: '2017-07-15T10:23:36+05:30'
status: publish
permalink: /2017/07/15/tns-12543-tns-destination-host-unreachable
author: Sidhu
excerpt: ''
type: post
id: 556
category:
    - Database
    - Troubleshooting
    - Unix/Linux
tag:
    - TNS-12543
post_format: []
wpsd_autopost:
    - '1'
---
Scenario : Setting up a physical standby from Exadata to a non-Exadata single instance. tnsping from standby to primary works fine but tnsping from primary to standby fails with:

```
<pre class="brush: sql; title: ; notranslate" title="">TNS-12543: TNS:destination host unreachable
```

I am able to ssh standby from primary, can ping as well but tnsping doesn’t work. From the error description we can figure out that something is blocking the access. In this case it was iptables that was enabled on the standby server.

Stopping the service resolved the issue.

```
<pre class="brush: bash; title: ; notranslate" title="">service iptables stop
chkconfig iptables off
```

The error is an obvious one but sometimes it just doesn’t strike you that it could be something simple like that.