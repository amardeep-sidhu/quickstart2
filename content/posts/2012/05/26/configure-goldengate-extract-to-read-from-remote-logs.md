---
title: 'Configure GoldenGate Extract to read from remote logs'
date: '2012-05-26T18:13:02+05:30'
status: publish
permalink: /2012/05/26/configure-goldengate-extract-to-read-from-remote-logs
author: Sidhu
excerpt: ''
type: post
id: 398
category:
    - GoldenGate
tag:
    - Extract
    - GoldenGate
    - 'Remote logs'
post_format: []
aktt_notify_twitter:
    - 'yes'
wpsd_autopost:
    - '1'
aktt_tweeted:
    - '1'
---
Sometimes you may need to run GoldenGate on some different machine than the one that hosts the database. It is very much possible but some kind of restrictions apply. First is that the Endian order of both the systems should be same and the second is the bit width has to be same. For example it is not possible to run GoldenGate on a 32 bit system to read from a database that runs on some 64 bit platform. Assuming that the environemnt satisfies the above two conditions; we can use the LOGSOURCE option of TRANSLOGOPTIONS to achieve this.

Here we run GG on host goldengate1 (192.168.0.109) and the database from which we want to capture the changes runs on the host goldengate3 (192.168.0.111). Both the systems run 11.2.0.2 on RHEL 5.5. On goldengate3 redo logs are in the mount point /home which has been NFS mounted on goldengate1 as /home\_gg3

```
Filesystem           1K-blocks      Used Available Use% Mounted on

192.168.0.111:/home   12184800   7962496   3593376  69% /home_gg3
```

 The Extract parameters are as follows:

```
EXTRACT ERMT01

USERID ggadmin@orcl3, PASSWORD ggadmin

EXTTRAIL ./dirdat/er

TRANLOGOPTIONS LOGSOURCE LINUX, PATHMAP /home/oracle/app/oracle/oradata/orcl /home_gg3/oracle/app/oracle/oradata/or
cl, PATHMAP /home/oracle/app/oracle/flash_recovery_area/ORCL/archivelog /home_gg3/oracle/app/oracle/flash_recovery_
area/ORCL/archivelog

TABLE HR.*;

(The text in the line starting with TRANLOGOPTIONS is a single line)
```

So using PATHMAP we can make GG aware about the actual location of the red logs &amp; archive logs on the remote server and the mapped location on the system where GG is running (It is somewhat like db\_file\_name\_convert option for Data Guards).

We fire some DMLs on the source database and then run stats command for the Extract

```
GGSCI (goldengate1) 93> stats ermt01 totalsonly *

Sending STATS request to EXTRACT ERMT01 ...

Start of Statistics at 2012-05-26 05:17:05.

Output to ./dirdat/er:

Cumulative totals for specified table(s):

*** Total statistics since 2012-05-26 04:51:10 ***
        Total inserts                                1.00
        Total updates                                0.00
        Total deletes                                1.00
        Total discards                               0.00
        Total operations                             2.00
.
.
.

End of Statistics.

GGSCI (goldengate1) 94>
```

For more details have a look at the [GG reference guide (Page 402)](http://docs.oracle.com/cd/E35209_01/doc.1121/e29399.pdf).