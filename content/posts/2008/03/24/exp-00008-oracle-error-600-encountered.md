---
title: 'EXP-00008: ORACLE error 600 encountered'
date: '2008-03-24T23:06:50+05:30'
status: publish
permalink: /2008/03/24/exp-00008-oracle-error-600-encountered
author: Sidhu
excerpt: ''
type: post
id: 63
category:
    - Troubleshooting
tag:
    - Export/Import
    - ORA-600
    - Troubleshooting
post_format: []
---
Today I was running export of an Oracle 9.2.0.1 database. The export completed but with an ORA-600 error:

```
<pre class="brush: css; title: ; notranslate" title="">

EXP-00008: ORACLE error 600 encountered
ORA-00600: internal error code, arguments: [xsoptloc2], [4], [4], [0], [], [], [], []
ORA-06512: in "SYS.DBMS_AW", line 347
ORA-06512: in "SYS.DBMS_AW", line 470
ORA-06512: in "SYS.DBMS_AW_EXP", line 270
ORA-06512: in line 1
EXP-00083: The previous problem occurred when calling SYS.DBMS_AW_EXP.schema_info_exp
```

I googled a bit and found that the problem is with applying some patchset. Then metalink confirmed the same. Somebody tried applying a patch to upgrade it to 9.2.0.5 but didn’t perform all the steps (missed post installation steps, to be precise). Metalink [Note 300849.1 ](https://metalink.oracle.com/metalink/plsql/f?p=130:14:3653505393918947609::::p14_database_id,p14_docid,p14_show_header,p14_show_help,p14_black_frame,p14_font:NOT,300849.1,1,1,1,helvetica)covers the issue and also gives the solution. In nutshell startup the database with **startup migrate** and **run catpatch.sql**.