---
title: 'Take care of a slash in a SQL script'
date: '2009-06-14T17:31:49+05:30'
status: publish
permalink: /2009/06/14/take-care-of-slash-sql-script
author: Sidhu
excerpt: ''
type: post
id: 202
category:
    - Troubleshooting
tag:
    - 'SQL script'
    - 'SQL* Plus'
post_format: []
aktt_notify_twitter:
    - 'yes'
syntaxhighlighter_encoded:
    - '1'
aktt_tweeted:
    - '1'
---
Since long time i have almost been writing useless posts only. Now, i guess my blog doesn’t even look like an Oracle blog. So thought about posting something related to Oracle 😉

Day before yesterday a colleague at my workplace asked that she was running an SQL script (which contained a simple DBMS\_MVIEW.REFRESH() statement to refresh an MVIEW), it ran successfully but after completion re-ran the last command run in the session. I was also puzzled and checked the SQL script but it contained simple DBMS\_MVIEW.REFRESH() statement. Next try revealed that the script actually had a / (slash) in the second line (with no semi-colon at the end of the first line). Something like this (I used dbms\_stats instead of dbms\_mview):

```
<pre class="brush: sql; title: ; notranslate" title="">exec dbms_stats.gather_table_stats(user,'EMP')
/

```

Now this thing, when run in SQL\* Plus session can be confusing:

```
<pre class="brush: sql; title: ; notranslate" title="">SCOTT@TESTING >
SCOTT@TESTING >delete emp1;
delete emp1
*
ERROR at line 1:
ORA-00942: table or view does not exist

SCOTT@TESTING >@c:\test

PL/SQL procedure successfully completed.

delete emp1
*
ERROR at line 1:
ORA-00942: table or view does not exist

SCOTT@TESTING >
```

There is no semicolon at the end of the first statement but it executes without that also. So the slash in the 2nd line simply re-executes the last SQL, as expected 🙂 . But it does get confusing !