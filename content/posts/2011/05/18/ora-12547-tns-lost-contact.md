---
title: 'ORA-12547: TNS:lost contact'
date: '2011-05-18T11:13:08+05:30'
status: publish
permalink: /2011/05/18/ora-12547-tns-lost-contact
author: Sidhu
excerpt: ''
type: post
id: 351
category:
    - Troubleshooting
tag:
    - ORA-12547
    - 'TNS:lost contact'
post_format: []
aktt_notify_twitter:
    - 'yes'
aktt_tweeted:
    - '1'
---
Very simple issue but took some amount of time in troubleshooting so thought about posting it here. May be it proves to be useful for someone.

Scenario was: Oracle is installed from “oracle” user and all runs well. There is a new OS user “test1” that also needs to use sqlplus. So granted the necessary permissions on ORACLE\_HOME to test1. Tried to connect sqlplus scott/tiger@DB and yes it works. But while trying sqlplus scott/tiger it throws:

```
<pre class="brush: sql; title: ; notranslate" title="">$ sqlplus scott/tiger

SQL*Plus: Release 10.2.0.5.0 - Production on Wed May 18 09:32:35 2011

Copyright (c) 1982, 2010, Oracle.  All Rights Reserved.

ERROR:
ORA-12547: TNS:lost contact


Enter user-name: ^C
$
```

Did a lot of troubleshooting including checking tnsnames.ora, sqlnet.ora, listener.ora and so on. Nothing was hitting my mind so finally raised an SR. And it has to do with the permissions of the $ORACLE\_HOME/bin/oracle binary. The permissions of oracle executable should be rwsr-s–x or 6751 but they were not. See below:

```
<pre class="brush: sql; title: ; notranslate" title="">$ id
uid=241(test1) gid=202(users) groups=1(staff),13(dba)
$

$ cd $ORACLE_HOME/bin
$ ls -ltr oracle
-rwxr-xr-x    1 oracle   dba       136803483 Mar 16 20:32 oracle
$

$ chmod 6751 oracle
$ ls -ltr oracle
-rwsr-s--x    1 oracle   dba       136803483 Mar 16 20:32 oracle
$

$ sqlplus scott/tiger

SQL*Plus: Release 10.2.0.5.0 - Production on Wed May 18 10:23:27 2011

Copyright (c) 1982, 2010, Oracle.  All Rights Reserved.


Connected to:
Oracle Database 10g Enterprise Edition Release 10.2.0.5.0 - 64bit Production
With the Partitioning, OLAP, Data Mining and Real Application Testing options

SQL> show user
USER is "SCOTT"
SQL>
```