---
title: 'EXP-00056: ORACLE error 28002 encountered'
date: '2010-11-14T00:24:30+05:30'
status: publish
permalink: /2010/11/14/exp-00056-oracle-error-28002-encountered
author: Sidhu
excerpt: ''
type: post
id: 281
category:
    - Troubleshooting
tag:
    - 'Bug 1654141'
    - EXP-00056
    - ORA-24309
    - ORA-28002
post_format: []
aktt_notify_twitter:
    - 'yes'
aktt_tweeted:
    - '1'
---
Yesterday, a friend of mine asked me about an error he was getting while running a schema level export in Oracle 8i:

```
<pre class="brush: sql; title: ; notranslate" title="">exp system/manager@DB owner=ABC file=ABC.dmp log =ABC.log

Export: Release 8.1.7.1.0 - Production on Fri Nov 12 04:21:05 2010

(c) Copyright 2000 Oracle Corporation.  All rights reserved.

EXP-00056: ORACLE error 28002 encountered
ORA-28002: the password will expire within 11 days
EXP-00056: ORACLE error 24309 encountered
ORA-24309: already connected to a server
EXP-00000: Export terminated unsuccessfully
```

I googled and metalink’ed (oops…is there anything like that ? 😉 ) a bit and found that it was bug 1654141 where user accounts in grace period cannot perform export. It is fixed in Oracle 9i (version 9.0.0.0 as per metalink). The obvious work around is to change the password and then try again. Thought about posting it here so that Google can give little better results if someone in trouble comes searching for it ;).