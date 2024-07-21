---
title: 'wrap&#8217;ed code and SQL trace'
date: '2009-09-18T21:42:00+05:30'
status: publish
permalink: /2009/09/18/wraped-code-and-sql-trace
author: Sidhu
excerpt: ''
type: post
id: 257
category:
    - PL/SQL
tag:
    - 'SQL Trace'
    - Wrap
post_format: []
aktt_notify_twitter:
    - 'yes'
syntaxhighlighter_encoded:
    - '1'
aktt_tweeted:
    - '1'
---
Yesterday, one of my colleague asked that if he traced a wrap’ed PL/SQL procedure, would the SQL statements show up in the trace ? Very simple thing but at that moment i got, sort of into doubt. So i ran a simple test and yes they do show up 😉

```
<pre class="brush: sql; title: ; notranslate" title="">CREATE OR REPLACE PROCEDURE wrap1
AS
v_today   DATE;
BEGIN
SELECT SYSDATE
INTO v_today
FROM DUAL;
END;
/

C:\>wrap iname=wrap1.sql

PL/SQL Wrapper: Release 10.2.0.1.0- Production on Fri Sep 18 21:07:49 2009

Copyright (c) 1993, 2004, Oracle.  All rights reserved.

Processing wrap1.sql to wrap1.plb

C:\>more wrap1.plb
CREATE OR REPLACE PROCEDURE wrap1 wrapped
a000000
b2
abcd
abcd
abcd
abcd
abcd
abcd
abcd
abcd
abcd
abcd
abcd
abcd
abcd
abcd
abcd
7
65 96
uu93le0yJCtORZedJgcWflZ1Jacwg5nnm7+fMr2ywFwWlvJWfF3AdIsJaWnnbSgIv1JfNsJx
doRxO75ucVUAc2fTr+Ii4v+onq/3r8q9yOOsrLAP4yRZW6LbYoWa6q9sd7PG7Nk9cpXs+6Y5
tQR4

/
```

And here is the output from the trace file, showing the SQL statement:

```
<pre class="brush: sql; title: ; notranslate" title="">BEGIN wrap1; END;

call     count       cpu    elapsed       disk      query    current        rows
------- ------  -------- ---------- ---------- ---------- ----------  ----------
Parse        1      0.00       0.00          0          0          0           0
Execute      1      0.03       0.01          0          0          0           1
Fetch        0      0.00       0.00          0          0          0           0
------- ------  -------- ---------- ---------- ---------- ----------  ----------
total        2      0.03       0.02          0          0          0           1

Misses in library cache during parse: 1
Optimizer mode: ALL_ROWS
Parsing user id: 54
********************************************************************************

SELECT SYSDATE
FROM
DUAL
```