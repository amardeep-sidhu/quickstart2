---
title: 'ORA-03113 Refresh of a mview in Oracle 10g'
date: '2008-01-03T07:33:00+05:30'
status: publish
permalink: /2008/01/03/ora-03113-refresh-of-a-mview-in-oracle-10g
author: Sidhu
excerpt: ''
type: post
id: 46
category:
    - 'Oracle Tips'
    - Troubleshooting
tag:
    - bug
    - 'mview refresh'
    - ora-03113
    - Troubleshooting
post_format: []
blogger_blog:
    - amardeepsidhu.blogspot.com
blogger_author:
    - Sidhu
blogger_permalink:
    - /2008/01/ora-03113-refresh-of-mview-in-oracle.html
aktt_tweeted:
    - '1'
---
At my workplace we were facing a problem with refresh of a mview. Say it was created in schema of user1 but when I tried to refresh it from user2 it would give <span style="font-style: italic">ORA-03113: end-of-file on communication channel. </span>Then we raised a SR and have been following up with Oracle support for long but it was not getting anywhere. Yesterday that guy seemed to have reached some point. The mviews that we have created and are having problem with refresh are created on top of both local &amp; remote objects and he said that up to 11gr2 there is no possibility of creating mviews on both local and remote objects. I did validate this thing. All the mviews failing to refresh are created on top of both local &amp; remote objects. But again from the owner the refresh is fine but from another user it gives problem. By the way that guy hinted at [bug 4084125](https://metalink.oracle.com/metalink/plsql/f?p=130:15:3284197721413823598::::p15_database_id,p15_docid,p15_show_header,p15_show_help,p15_black_frame,p15_font:BUG,4084125,1,1,1,helvetica) and also suggested a work around. I haven’t tried that yet. Will try and update about the results.

Sidhu