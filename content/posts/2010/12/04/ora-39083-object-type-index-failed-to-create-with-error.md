---
title: 'ORA-39083: Object type INDEX failed to create with error'
date: '2010-12-04T17:22:56+05:30'
status: publish
permalink: /2010/12/04/ora-39083-object-type-index-failed-to-create-with-error
author: Sidhu
excerpt: ''
type: post
id: 290
category:
    - Troubleshooting
tag:
    - ORA-14102
    - ORA-39083
post_format: []
aktt_notify_twitter:
    - 'yes'
aktt_tweeted:
    - '1'
---
Another let-us-help-Google post ;).

While running impdp import in 11g, you hit:

```
<pre class="brush: sql; title: ; notranslate" title="">ORA-39083: Object type INDEX failed to create with error:
ORA-14102: only one LOGGING or NOLOGGING clause may be specified
```

It is related to bug 9015411 where DBMS\_METADATA.GET\_DDL creates incorrect create index statement by dumping both LOGGING and NO LOGGING clauses. Due to this the CREATE INDEX statement, while running impdp fails with the above error. It applies to 11.2.0.1 (Metalink doc id 1066635.1)

Fix is to install the patch, if it is available for your platform. Another workaround is given in [this OTN thread](http://forums.oracle.com/forums/thread.jspa?messageID=4512555) i.e. strip the create index statement of storage related information by using TRANSFORM=SEGMENT\_ATTRIBUTES:N:INDEX &amp; TRANSFORM=SEGMENT\_ATTRIBUTES:N:CONSTRAINT