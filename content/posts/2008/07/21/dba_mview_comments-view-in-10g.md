---
title: 'DBA_MVIEW_COMMENTS view in 10g'
date: '2008-07-21T07:28:32+05:30'
status: publish
permalink: /2008/07/21/dba_mview_comments-view-in-10g
author: Sidhu
excerpt: ''
type: post
id: 92
category:
    - 'Oracle Tips'
tag:
    - dba_mview_comments
    - 'mviews comments'
post_format: []
aktt_tweeted:
    - '1'
---
Our application (3 tier, Front end Forms10g and back end 10gR2) provides user with a front end to refresh the mviews. That form has 2 columns showing mview name and the comment against it. Recently i saw that while opening this front end ORA-01403 NO DATA FOUND was being raised.

I opened the fmb and found that it was populating comments from DBA\_TAB\_COMMENTS. In 10g the comments against mviews are stored in DBA\_MVIEW\_COMMENTS unlike till 9i where it was in ALL\_TAB\_COMMENTS. So there was a little modification required.

BTW if you try to comment on the table (which is created with MVIEW) it won’t allow you to do so and instead raise ORA-12098: cannot comment on the materialized view.

So may be that little change needs to be done !