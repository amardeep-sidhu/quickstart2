---
title: 'ORA-12154 in Data Guard environment'
date: '2017-05-31T21:24:54+05:30'
status: publish
permalink: /2017/05/31/ora-12154-in-data-guard-environment
author: Sidhu
excerpt: ''
type: post
id: 548
category:
    - Database
tag:
    - 'Data Guard'
    - ORA-12154
post_format: []
wpsd_autopost:
    - '1'
---
Hit this silly issue in one of the data guard environments today. Primary is a 2 node RAC running 11.2.0.4 and standby is also a 2 node RAC. Archive logs from node2 aren’t shipping and the error being reported is

```
<pre class="brush: sql; title: ; notranslate" title="">ORA-12154: TNS:could not resolve the connect identifier specified
```

We tried usual things like going to $TNS\_ADMIN, checking the entry in tnsnames.ora and then also trying to connect using sqlplus sys@target as sysdba. Everything seemed to be good but logs were not shipping and the same problem was being reported repeatedly. As everything on node1 was working fine so it looked even more weird.

From the error it is clear that the issue is with tnsnames entry. Finally found the issue after some 30 mins. It was an Oracle EBS environment so the TNS\_ADMIN was set to the standard $ORACLE\_HOME/network/admin/\*hostname\* path (on both the nodes). On node1 there was no tnsnames.ora file in $ORACLE\_HOME/network/admin so it was connecting to the standby using the Apps tnsnames.ora which was having the correct entry for standby. On node2 there was a file called tnsnames.ora in $ORACLE\_HOME/network/admin but it was not having any entry for standby. It was trying to connect using that file (the default tns path) and failing with ORA-12154. Once we removed that file, it started using the Apps tnsnames.ora and logs started shipping.