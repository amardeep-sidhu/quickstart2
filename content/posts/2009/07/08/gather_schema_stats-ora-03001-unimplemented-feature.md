---
title: 'GATHER_SCHEMA_STATS &#038; ORA-03001: unimplemented feature'
date: '2009-07-08T23:33:47+05:30'
status: publish
permalink: /2009/07/08/gather_schema_stats-ora-03001-unimplemented-feature
author: Sidhu
excerpt: ''
type: post
id: 233
category:
    - Troubleshooting
tag:
    - 'Bug 5767661'
    - DBMS_STATS
    - GATHER_SCHEMA_STATS
post_format: []
aktt_notify_twitter:
    - 'yes'
syntaxhighlighter_encoded:
    - '1'
aktt_tweeted:
    - '1'
---
Today i was gathering stats on one schema (10.2.0.3 on AIX 5.3, 64 bit) and it said:

```
<pre class="brush: sql; title: ; notranslate" title="">ERROR at line 1:
ORA-03001: unimplemented feature
ORA-06512: at "SYS.DBMS_STATS", line 13336
ORA-06512: at "SYS.DBMS_STATS", line 13682
ORA-06512: at "SYS.DBMS_STATS", line 13760
ORA-06512: at "SYS.DBMS_STATS", line 13719
ORA-06512: at line 1
```

Little bit of searching on Metalink revealed that i had hit Bug no 6011068 which points to the base [Bug 576661](https://metalink2.oracle.com/metalink/plsql/f?p=130:14:5335018637904207860::::p14_database_id,p14_docid,p14_show_header,p14_show_help,p14_black_frame,p14_font:NOT,559389.1,1,1,1,helvetica) which is related to function based indexes. There were 2 function based indexes in the schema. Before talking about the workaround let us re-produce the test case. Here i am doing it on my laptop (10.2.0.1 on Windows XP 32 bit)

```
<pre class="brush: sql; title: ; notranslate" title="">SCOTT@TESTING >create table test1 as select * from emp;

Table created.

SCOTT@TESTING >create index ind1 on test1(comm,1);

Index created.

SCOTT@TESTING >

SYSTEM@TESTING>exec dbms_stats.gather_schema_stats('SCOTT');
BEGIN dbms_stats.gather_schema_stats('SCOTT'); END;

*
ERROR at line 1:
ORA-03001: unimplemented feature
ORA-06512: at "SYS.DBMS_STATS", line 13210
ORA-06512: at "SYS.DBMS_STATS", line 13556
ORA-06512: at "SYS.DBMS_STATS", line 13634
ORA-06512: at "SYS.DBMS_STATS", line 13593
ORA-06512: at line 1

SYSTEM@TESTING>
```

As suggested in the metalink article let us set event 3001 before running the GATHER\_SCHEMA\_STATS command.

```
<pre class="brush: sql; title: ; notranslate" title="">SYSTEM@TESTING >alter session set tracefile_identifier=stats1;

Session altered.

SYSTEM@TESTING >alter session set events '3001 trace name ERRORSTACK level 3';

Session altered.

SYSTEM@TESTING >exec dbms_stats.gather_schema_stats('SCOTT');
BEGIN dbms_stats.gather_schema_stats('SCOTT'); END;

*
ERROR at line 1:
ORA-03001: unimplemented feature
ORA-06512: at "SYS.DBMS_STATS", line 13210
ORA-06512: at "SYS.DBMS_STATS", line 13556
ORA-06512: at "SYS.DBMS_STATS", line 13634
ORA-06512: at "SYS.DBMS_STATS", line 13593
ORA-06512: at line 1

SYSTEM@TESTING >
```

Part of the trace file reads:

```
<pre class="brush: sql; title: ; notranslate" title="">ksedmp: internal or fatal error
ORA-03001: unimplemented feature
Current SQL statement for this session:
select /*+ no_parallel_index(t,IND1) dbms_stats cursor_sharing_exact use_weak_name_resl dynamic_sampling(0) no_monitoring no_expand index(t,"IND1") */ count(*) as nrw,count(distinct sys_op_lbid(51966,'L',t.rowid)) as nlb,count(distinct hextoraw(sys_op_descend("COMM")||sys_op_descend(1))) as ndk,sys_op_countchg(substrb(t.rowid,1,15),1) as clf from "SCOTT"."TEST1" t where "COMM" is not null or 1 is not null
----- PL/SQL Call Stack -----
object      line  object
handle    number  name
65AA77D4      9406  package body SYS.DBMS_STATS
65AA77D4      9919  package body SYS.DBMS_STATS
```

So the problem is being caused by the index ind1 we created on (comm,1). This bug has been fixed in 10.2.0.5 and 11.1.0.7. The available workaround for other versions is to create index using 1 as character instead of number.

```
<pre class="brush: sql; title: ; notranslate" title="">SCOTT@TESTING >drop index ind1;

Index dropped.

SCOTT@TESTING >create index ind1 on test1(comm,'1');

Index created.

SCOTT@TESTING >
```

And now running GATHER\_SCHEMA\_STATS:

```
<pre class="brush: sql; title: ; notranslate" title="">SYSTEM@TESTING >exec dbms_stats.gather_schema_stats('SCOTT');

PL/SQL procedure successfully completed.

SYSTEM@TESTING >
```