---
title: 'waiting for resmgr:become active &#8211; can&#8217;t login'
date: '2011-03-04T17:19:33+05:30'
status: publish
permalink: /2011/03/04/waiting-for-resmgr-become-active-cant-login
author: Sidhu
excerpt: ''
type: post
id: 342
category:
    - Troubleshooting
tag:
    - 'resmgr:become active'
post_format: []
aktt_notify_twitter:
    - 'yes'
aktt_tweeted:
    - '1'
---
Some time back, I was at a client where the customer complained that no one was able to log in to the database. It was Oracle 10.2.0.4 running on HP-Ux. I logged in to the database and checked the wait events:

```
<pre class="brush: sql; title: ; notranslate" title="">SQL> @wait

EVENT                                                              COUNT(*)
---------------------------------------------------------------- ----------
wait for possible quiesce finish                                          1
Streams AQ: qmn coordinator idle wait                                     1
Streams AQ: qmn slave idle wait                                           1
Streams AQ: waiting for time management or cleanup tasks                  1
SQL*Net message to client                                                 1
smon timer                                                                1
pmon timer                                                                1
jobq slave wait                                                           4
rdbms ipc message                                                        11
SQL*Net message from client                                              27
resmgr:become active                                                    322

11 rows selected.

SQL>
```

Tanel’s snapper showed something like:

```
<pre class="brush: sql; title: ; notranslate" title="">SQL> @snapper ash 5 1 all
Sampling with interval 5 seconds, 1 times...

-- Session Snapper v3.11 by Tanel Poder @ E2SN ( http://tech.e2sn.com )

-----------------------------------------------------------------------
Active% | SQL_ID          | EVENT                     | WAIT_CLASS
-----------------------------------------------------------------------
 26322% | 4ffu7nb93c2c9   | resmgr:become active      | Scheduler
  1900% | 2wn958z7gzh57   | resmgr:become active      | Scheduler
  1400% | 9d9bg2r538nd2   | resmgr:become active      | Scheduler
   600% | 4d3k70q6y344k   | resmgr:become active      | Scheduler
   500% | d6vwqbw6r2ffk   | resmgr:become active      | Scheduler
   500% | 4tsrz92mmshbw   | resmgr:become active      | Scheduler
   200% | 37td1bbvc1a69   | resmgr:become active      | Scheduler
   100% | ftdjfxws0s8q9   | resmgr:become active      | Scheduler
   100% | 41apc1bjqrfbv   | resmgr:become active      | Scheduler
   100% | af9d8aqtkvn02   | resmgr:become active      | Scheduler

--  End of ASH snap 1, end=2011-02-10 11:06:40, seconds=5, samples_taken=23

PL/SQL procedure successfully completed.

SQL>
```

If we check the [description of the wait event](http://download.oracle.com/docs/cd/B19306_01/server.102/b14237/waitevents003.htm#sthref3161), it says:

> The session is waiting for a resource manager active session slot. This event occurs when the resource manager is enabled and the number of active sessions in the session’s current consumer group exceeds the current resource plan’s active session limit for the consumer group. To reduce the occurrence of this wait event, increase the active session limit for the session’s current consumer group.

But if we check the resource\_limit settings:

```
<pre class="brush: sql; title: ; notranslate" title="">SQL> show parameter resource

NAME_COL_PLUS_SHOW_PARAM   TYPE		VALUE_COL_PLUS_SHOW_PARAM
----------------------- -----------     --------------------------
resource_limit            boolean		FALSE
resource_manager_plan     string

SQL>
```

What ? Resource manager is not enabled. But why all the sessions are waiting for **resmgr:become active** and nobody is able to login ?

A bit of googling lead me to [this page](http://www.saptechies.com/oracle-wait-events) from where I got the clue.

> Generally, this wait situation occurs when you execute certain EMCA operations such as the operation for creating the EM repository. As a result of these operations, the systems implicity switches to QUIESCE mode. Therefore, all database connections (except users SYS and SYSTEM) must wait for “resmgr:become active”. In this case, refer to Note 1044758 and execute the following command if necessary:
> 
> ALTER SYSTEM UNQUIESCE;

I asked around in the DBA team and one of the guys was trying to configure EM for the database due to which system switched tto QUIESCE mode and all the sessions were waiting on **resmgr:become active**.

After canceling the operation, the wait event was gone and everything was working normally.