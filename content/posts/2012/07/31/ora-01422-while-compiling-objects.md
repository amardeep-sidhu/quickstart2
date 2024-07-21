---
title: 'ORA-01422 while compiling objects'
date: '2012-07-31T22:04:28+05:30'
status: publish
permalink: /2012/07/31/ora-01422-while-compiling-objects
author: Sidhu
excerpt: ''
type: post
id: 415
category:
    - Troubleshooting
tag:
    - 'Compile objects'
    - Troubleshooting
post_format: []
aktt_notify_twitter:
    - 'yes'
aktt_tweeted:
    - '1'
---
There was an interesting issue at one of the customer sites. Few tables in the database were altered and the dependent objects became invalid. But the attempts to compile the objects using utlrp.sql or manually were failing. In all the cases it was giving the same error:

```
SQL> alter function SCOTT.SOME_FUNCTION compile;
 alter function SCOTT.SOME_FUNCTION compile
*
ERROR at line 1:
ORA-00604: error occurred at recursive SQL level 1
ORA-01422: exact fetch returns more than requested number of rows
ORA-06512: at line 27

SQL>
```

At first look it sounded like some issue with the dictionary as the error in case of every object (be it a view, function or package) was the same.

Everybody was trying to compile the invalid objects and surprisingly few VIEWs (that were not getting compiled from SQL\*Plus) got compiled from Toad ! But that didn’t explain anything. In fact it was more confusing.

Finally I enabled errorstack for event 1422 and tried to compile a view. Here is the relevant content from the trace file

```
----- Error Stack Dump -----
ORA-01422: exact fetch returns more than requested number of rows
----- Current SQL Statement for this session (sql_id=7kb01v7t6s054) -----
SELECT SQL_TEXT FROM V$OPEN_CURSOR VOC, V$SESSION VS WHERE VOC.SADDR = VS.SADDR AND AUDSID=USERENV('sessionid') AND UPPER(SQL_TEXT) LIKE 'ALTER%'
```

I took it to be some system SQL and started searching in that direction and obviously that was of no use.

In the mean time another guy almost shouted…”oh there is a trigger to capture DDL operations in the database; it must be that”. And indeed it was. Here is the code that was creating the problem:

```
 select sql_text into vsql_text
           from v$open_cursor voc, v$session vs
           where voc.saddr = vs.saddr
           and audsid=userenv('sessionid')
           and upper(sql_text) like 'ALTER%';
```

```
 
```

As v$open\_cursor was returning multiple rows, hence the problem !

Moral is that the errorstack traces do tell a lot (of course if you listen carefully) 😉