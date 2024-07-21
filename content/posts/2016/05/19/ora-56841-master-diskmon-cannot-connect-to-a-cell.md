---
title: 'ORA-56841: Master Diskmon cannot connect to a CELL'
date: '2016-05-19T22:15:05+05:30'
status: publish
permalink: /2016/05/19/ora-56841-master-diskmon-cannot-connect-to-a-cell
author: Sidhu
excerpt: ''
type: post
id: 471
category:
    - Exadata
tag:
    - Exadata
    - ORA-56841
    - 'Storage Cell'
post_format: []
wpsd_autopost:
    - '1'
---
Faced this error while querying v$asm\_disk after adding new storage cell IPs to cellip.ora on DB nodes of an existing cluster on Exadata. Query ends with *ORA-03113 end-of-file on communication channel* and *ORA-56841* is reported in *$ORA\_CRS\_HOME/log/&lt;hostname&gt;/diskmon/diskmon.log*. Reason in my case was that the new cell was using different subnet for IB. It was pingable from the db nodes but querying v$asm\_disk wasn’t working. Changing the subnet for IB on new cell to the one on existing cells fixed the issue.