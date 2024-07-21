---
title: 'dbc_min_pct and dbc_max_pct in HP-UX'
date: '2011-05-25T16:45:56+05:30'
status: publish
permalink: /2011/05/25/dbc_min_pct-and-dbc_max_pct-in-hp-ux
author: Sidhu
excerpt: ''
type: post
id: 357
category:
    - 'Oracle Tuning'
    - Troubleshooting
tag:
    - dbc_max_pct
    - dbc_min_pct
    - HP-UX
post_format: []
aktt_notify_twitter:
    - 'yes'
aktt_tweeted:
    - '1'
---
It was a 10g (10.2.0.5 on HP-UX 11.23 RISC) database which was recently upgraded from 9.2.0.8. The CPU and memory utilization was going really high. After tuning few of the queries coming in top, CPU usage was coming within accetable limits but the memory usage was still high. There was a total of 16 GB of RAM on the server and the usage was above 90%, constantly. One of the reasons behind high usage was increase in the SGA size. It was increased from 2.5 GB (in 9i) to around 5 GB (in 10g). Another major chunk was being eaten by OS buffer cache. While looking at the memory usage with kmeminfo:

```
<pre class="brush: bash; title: ; notranslate" title="">Buffer cache        =  1048448    4.0g  25%  details with -bufcache
```

In HP-UX, The memory allocated to (dynamic) buffer cache is controlled by two parameters **dbc\_min\_pct** and **dbc\_max\_pct**. It can vary between dbc\_min\_pct and dbc\_max\_pct percent of the total RAM. They default to 5 and 50 respectively. For a system that is running an Oracle database value of 50 for dbc\_max\_pct is way too high. That means half of the memory is going to be allocated to OS buffer cache. As Oracle has got its own buffer cache so the OS cache is not of much use. As mentioned in the metalink note 726652.1, the value of dbc\_max\_pct can be safely lowered without impacting the Oracle database performance. In many of the threads (on HP website) people have suggested the value of 10 for db\_max\_pct. Not sure if it is more like a thumb rule but in the same metalink note (726652.1) it is mentioned that if %rcache in sar -b is above 90, that means your OS buffer cache is adequately sized.

After setting the value of dbc\_max\_pct to 15 (It will be changed to 10, finally), around 1.6 GB more memory was freed. Also there was no impact on the database or OS performance. Here are few of the metalink notes and threads on HP-UX website that talk about these parameters in detail:

[Oracle Shadow Processes Are Taking Too Much Memory (Doc ID 434535.1)](https://supporthtml.oracle.com/ep/faces/secure/km/DocumentDisplay.jspx?id=434535.1)

[How OS Buffer Cache Size Affects Db Performance (Doc ID 726652.1) ](https://supporthtml.oracle.com/ep/faces/secure/km/DocumentDisplay.jspx?id=726652.1)

[Commonly Misconfigured HP-UX Kernel Parameters (Doc ID 68105.1) ](https://supporthtml.oracle.com/ep/faces/secure/km/DocumentDisplay.jspx?id=68105.1)

[http://forums11.itrc.hp.com/service/forums/questionanswer.do?admit=109447626+1306231311459+28353475&amp;threadId=1266914](http://forums11.itrc.hp.com/service/forums/questionanswer.do?admit=109447626+1306231311459+28353475&threadId=1266914)

<http://forums11.itrc.hp.com/service/forums/questionanswer.do?threadId=727618>

<http://forums11.itrc.hp.com/service/forums/questionanswer.do?threadId=467288>

<http://forums11.itrc.hp.com/service/forums/questionanswer.do?threadId=750342>