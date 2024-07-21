---
title: 'Missing grants'
date: '2008-03-03T20:37:15+05:30'
status: publish
permalink: /2008/03/03/missing-grants
author: Sidhu
excerpt: ''
type: post
id: 53
category:
    - PL/SQL
    - SQL
    - Troubleshooting
tag:
    - PL/SQL
    - SQL
    - Troubleshooting
post_format: []
---
Today one of my colleague was working on a simple PL/SQL procedure. Based on some logic it was returning count(\*) from all\_tab\_columns for few tables. It gave count incorrectly for one table out of around fifty in total. He just hard coded the table name and ran it but again it showed count as zero.

Then he took the code out of procedure and wrote it in DECLARE, BEGIN, END and after running it showed the correct count. But ran as database procedure it always shows incorrectly.

Finally just as hit and trial, he gave SELECT on the TABLE to database user \[Table was in different schema\], used to run the procedure and everything was ok. Isn’t it bit stupid 🙂

**Update:** Well, it happens for a reason. [Nigel Thomas](http://preferisco.blogspot.com/) pointed out in the comment. The reason is that privileges granted to a role are not seen from PL/SQL stored procedures. You need to give direct grant to the user for this or another method is to define the procedure or package with invoker rights.

Thanks Nigel 🙂