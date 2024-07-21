---
title: 'Spool to a .xls (excel) file&#8230;'
date: '2007-06-16T00:14:00+05:30'
status: publish
permalink: /2007/06/16/spool-to-a-xls-excel-file
author: Sidhu
excerpt: ''
type: post
id: 22
category:
    - 'Oracle Tips'
    - SQL
tag:
    - 'spool to excel'
    - SQL
post_format: []
blogger_blog:
    - amardeepsidhu.blogspot.com
blogger_author:
    - Sidhu
blogger_permalink:
    - /2007/06/spool-to-xls-file.html
aktt_tweeted:
    - '1'
---
A small tip, I read on [OTN](http://forums.oracle.com/forums/thread.jspa?messageID=1849526) about spooling to a .xls (excel) file:

It goes like this

```
<pre class="brush: css; title: ; notranslate" title="">set feed off markup html on
spool onspool c:\salgrade.xls
select * from salgrade;
spool offset markup html off
spool off
```

And the xls it makes shows up like:

```
<a href="http://bp2.blogger.com/_cX21i_8-MT4/RnLaSNXaVrI/AAAAAAAAACM/nxm6jQbYTak/s1600-h/xls.JPG" onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}"><img alt="" border="0" decoding="async" id="BLOGGER_PHOTO_ID_5076359736360326834" src="http://bp2.blogger.com/_cX21i_8-MT4/RnLaSNXaVrI/AAAAAAAAACM/nxm6jQbYTak/s320/xls.JPG" style="margin: 0px auto 10px; display: block; text-align: center; cursor: pointer"></img></a>
```

![](file:///C:/DOCUME%7E1/Amardeep/LOCALS%7E1/Temp/moz-screenshot.jpg)Sidhu