---
title: 'ORA-30019: Illegal rollback Segment operation in Automatic Undo mode'
date: '2009-07-09T23:26:59+05:30'
status: publish
permalink: /2009/07/09/ora-30019-illegal-rollback-segment-operation-in-automatic-undo-mode
author: Sidhu
excerpt: ''
type: post
id: 245
category:
    - Troubleshooting
tag:
    - ORA-30019
post_format: []
aktt_notify_twitter:
    - 'yes'
syntaxhighlighter_encoded:
    - '1'
aktt_tweeted:
    - '1'
---
Today i was refreshing a MVIEW (Oracle 9.2.0.1.0 on Windows 2000) and instead of writing

```
<pre class="brush: sql; title: ; notranslate" title="">exec dbms_mview.refresh('SCHEMA1.MVIEW1','C'); 
```

i wrote

```
<pre class="brush: sql; title: ; notranslate" title="">exec dbms_mview.refresh('SCHEMA1','MVIEW1','C');
```

And it gave me:

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;">ERROR at line 1:</div><div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;">ORA-30019: Illegal rollback Segment operation in Automatic Undo mode</div><div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;">ORA-06512: at “SYS.DBMS_SNAPSHOT”, line 794</div><div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;">ORA-06512: at “SYS.DBMS_SNAPSHOT”, line 851</div><div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;">ORA-06512: at “SYS.DBMS_SNAPSHOT”, line 832</div><div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;">ORA-06512: at line 1</div>```
<pre class="brush: sql; title: ; notranslate" title="">ERROR at line 1:
ORA-30019: Illegal rollback Segment operation in Automatic Undo mode
ORA-06512: at "SYS.DBMS_SNAPSHOT", line 794
ORA-06512: at "SYS.DBMS_SNAPSHOT", line 851
ORA-06512: at "SYS.DBMS_SNAPSHOT", line 832
ORA-06512: at line 1
```

```
<span style="font-family: Georgia; line-height: 19px; white-space: normal; font-size: 13px;">which has nothing to do with the real error. Take care !</span>
```