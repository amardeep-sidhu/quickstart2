---
title: 'Unable to connect to the database with SQLPlus'
date: '2022-01-06T18:41:09+05:30'
status: publish
permalink: /2022/01/06/unable-to-connect-to-the-database-with-sqlplus
author: Sidhu
excerpt: ''
type: post
id: 1095
category:
    - EM
    - ZDLRA
tag:
    - EM
    - 'Enterprise Manager'
    - RMAN
    - zdlra
post_format: []
---
I was working on configuring a new database for backup to ZDLRA and hit this issue while testing a controlfile backup via Enterprise Manager -&gt; Schedule backup. It could happen in any environment.

```
<pre class="wp-block-code">```
Unable to connect to the database with SQLPlus, either because the database is down or due to an environment issue such as incorrectly specified...
If the database is up, check the database target monitoring properties and verify that the Oracle Home value is correct.
```
```

 The 2nd line clearly tells the problem but since the Cluster Database status in EM was green, so it took me a while to figure it out. Issue turned out to be a missing / in the end of ORACLE\_HOME specified in monitoring configuration of the cluster database. The DB home specified was /u01/app/oracle/product/11.2.0.4/dbhome\_1 instead of /u01/app/oracle/product/11.2.0.4/dbhome\_1**/**.

On the server the bash\_profile has the home set as /u01/app/oracle/product/11.2.0.4/dbhome\_1. When I tried to connect as sysdba there, it gave an error TNS : lost contact. Then I set the environment with .oraenv and I was able to connect. /etc/oratab had correct home specified as /u01/app/oracle/product/11.2.0.4/dbhome\_1**/**. After comparing the value of ORACLE\_HOME in these two cases, the issue was identified. Then I updated the ORACLE\_HOME value in the target monitoring configuration in Enterprise Manager and it worked as expected.