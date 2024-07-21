---
title: 'MRP process on standby stops with ORA-00600'
date: '2015-08-20T14:33:45+05:30'
status: publish
permalink: /2015/08/20/mrp-process-on-standby-stops-with-ora-00600
author: Sidhu
excerpt: ''
type: post
id: 451
category:
    - Database
    - Troubleshooting
tag:
    - 11g
    - 'Data Guard'
    - Standby
post_format: []
wpsd_autopost:
    - '1'
---
A rather not so great post about an ORA-00600 error i faced on a standby database. Environement was 11.2.0.3 on Sun Super Cluster machine. MRP process was hitting ORA-00600 while trying to apply a specific archive log.

The error message was something like this

```
MRP0: Background Media Recovery terminated with error 600
Errors in file /u01/app/oracle/product/11.2.0.3/diag/diag/rdbms/xxxprd/xxxprd1/trace/xxxprd1_pr00_6342.trc:
ORA-00600: internal error code, arguments: <span style="color: #ff0000;"><strong>[2619]</strong></span>, [539], [], [], [], [], [], [], [], [], [], []
Recovery interrupted!
```

Some googling and MOS searches revealed that the error was due to corrupted archive log file. Recopying the archive file from primary and restarting the recovery resolved the issue. The fist argument of the ORA-600 is actually the sequence no of the archive it is trying to apply.