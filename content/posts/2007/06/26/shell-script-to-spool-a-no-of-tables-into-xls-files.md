---
title: 'Shell script to spool a no of tables into .xls files&#8230;'
date: '2007-06-26T22:38:00+05:30'
status: publish
permalink: /2007/06/26/shell-script-to-spool-a-no-of-tables-into-xls-files
author: Sidhu
excerpt: ''
type: post
id: 25
category:
    - 'Oracle Tips'
    - SQL
tag: []
post_format: []
blogger_blog:
    - amardeepsidhu.blogspot.com
blogger_author:
    - Sidhu
blogger_permalink:
    - /2007/06/shell-script-to-spool-no-of-tables-into_26.html
---
On OTN someone asked [a question](http://forums.oracle.com/forums/thread.jspa?threadID=523835&tstart=50) that how to spool data from a table into a xls file. Spooling a single table I discussed in one of the [previous posts](http://amardeepsidhu.com/blog/2007/06/16/spool-to-a-xls-excel-file). We can use the same approach to spool data from more than 1 table also. Well here I will do it through a shell script and assume that you have a text file having list of tables to be spooled (Even if you don’t have one, it can be easily made by spooling the names of tables into a simple text file) Here is the shell script that you can use to spool data to various xls files, table wise.

```
<pre class="brush: css; title: ; notranslate" title="">cat list.txt | while read a
do
echo "spooling $a"
sqlplus username/password@string <<EOF
set feed off markup html on spool on
spool /home/oracle/$a.xls
select * from $a;
spool off
set markup html off spool off
EOF
done
```

I didn’t see any work around for Windoze as SQLPLUS &lt;&lt; EOF thing doesn’t seem to work in Windows. Will try to find some alternative. If you come across something, do let me know.

Sidhu