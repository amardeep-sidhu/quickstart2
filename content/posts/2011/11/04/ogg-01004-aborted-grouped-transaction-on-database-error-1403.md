---
title: 'OGG-01004  Aborted grouped transaction on <table_name>&#8216;, Database error 1403 ()'
date: '2011-11-04T16:38:39+05:30'
status: publish
permalink: /2011/11/04/ogg-01004-aborted-grouped-transaction-on-database-error-1403
author: Sidhu
excerpt: ''
type: post
id: 371
category:
    - GoldenGate
tag:
    - GoldenGate
post_format: []
aktt_notify_twitter:
    - 'yes'
wpsd_autopost:
    - '1'
aktt_tweeted:
    - '1'
---
The [last post](http://amardeepsidhu.com/blog/2011/11/04/expdp-not-consistent/) was just like that. It was this GoldenGate issue that woke me up from the deep sleep to do a post after a long time 😛 .

Well it was a simple schema to schema replication setup using GoldenGate. We were using the SCN method (Metalink Doc ID 1276058.1 &amp; 1347191.1) to do the intial load so that there is no overlvapping of transactions and the replicat runs with minimum issues. Even after following this method, the replicat was hitting

```
<pre class="brush: plain; title: ; notranslate" title="">2011-10-31 19:25:17  WARNING OGG-01004  Aborted grouped transaction on 'SCHEMA.TABLE', Database error 1403 ().

2011-10-31 19:25:17  WARNING OGG-01003  Repositioning to rba 3202590 in seqno 1.

2011-10-31 19:25:18  WARNING OGG-01154  SQL error 1403 mapping SCHEMA.TABLE TO SCHEMA.TABLE.

2011-10-31 19:25:18  WARNING OGG-01003  Repositioning to rba 3468713 in seqno 1.
```

If we managed to bypass this error somehow, it hit:

```
<pre class="brush: plain; title: ; notranslate" title="">2011-10-24 19:58:15  WARNING OGG-00869  OCI Error ORA-00001: unique constraint (SCHEMA.UK) violated (status = 1), SQL <INSERT INTO "SCHEMA"."TABLE" (<INSERT HERE>

2011-10-24 19:58:15  WARNING OGG-01004  Aborted grouped transaction on 'SCHEMA.TABLE', Database error 1 (OCI Error ORA-00001: unique constraint (SCHEMA.UK) violated (status = 1), SQL <INSERT HERE>).

2011-10-24 19:58:15  WARNING OGG-01003  Repositioning to rba 1502788 in seqno 3.

2011-10-24 19:58:15  WARNING OGG-01154  SQL error 1 mapping SCHEMA.TABLE to SCHEMA.TABLE OCI Error ORA-00001: unique constraint (SCHEMA.UK) violated (status = 1), SQL <INSERT HERE>.

2011-10-24 19:58:15  WARNING OGG-01003  Repositioning to rba 1502788 in seqno 3.
```

1403 means that GoldenGate couldn’t find the record it wanted to update.

00001 would mean that the record GoldenGate tried to insert was already there.

In our case, as we used SCN method so none of them was expected. So these weird errors left us totally confused. Some guys suggested that expdp was not taking a consistent image and some transactions were getting overlapped (picked up by both expdp &amp; GG extract trail). We took the database down and repeated the exercise but oops ! it hit almost the same errors again. So it was not about consistency for sure.

Till now we haven’t been examining the contents of discard file very seriously. As the errors were pretty simple so we always suspected that some transactions were getting overlapped. Now it was high time to take some help from discard file as well 😉 . We took the before/after image of the record from the discard file and checked it in the target database &amp; values in one or two columns were different (that is why GG couldn’t find that record). The new values were the actual hint towards the solution \[It was a table storing the mail requests and their statuses. This update that GG was trying to run was updating the status from NOT-SENT TO SENT but here on the target the status was already set to ‘ORA-something……’\]. We got the clue that something must have run on the target itself that spoiled this record and now GG is not able to find it and abending with 1403. select \* from dba\_jobs cleared it all. While doing the initial load with expdp/impdp, job also got imported and some of them were in not broken state. They were firing according to their schedule and making changes to data in the target. So before GG came to update/insert record the job had already done its game and the replicat was hitting different errors. We did the initial load again (this time by using flashback\_scn in the running database), disabled all the jobs and ran the replicat. It went through without any errors.

So things to take care of, in such cases:

1\) Disable all the triggers on the target side (or exclude triggers while running expdp)

2\) Look for and disable any scheduled jobs (could be dba\_jobs, dba\_scheduler\_jobs or cron)

Happy GoldenGate’ing !