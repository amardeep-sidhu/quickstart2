---
title: 'Delete Delete Faster Faster ;)'
date: '2009-10-08T20:53:36+05:30'
status: publish
permalink: /2009/10/08/delete-delete-faster-faster
author: Sidhu
excerpt: ''
type: post
id: 266
category:
    - SQL
tag:
    - Delete
    - SQL
post_format: []
aktt_notify_twitter:
    - 'yes'
syntaxhighlighter_encoded:
    - '1'
aktt_tweeted:
    - '1'
---
2-3 days ago, I came across a code, intended to make delete faster. Just have a look 😉

```
<pre class="brush: sql; title: ; notranslate" title="">.
.
.
LOOP
SELECT COUNT (1)
INTO v_cnt
FROM table1
WHERE ROWNUM < 2;

IF v_cnt = 0
THEN
EXIT;
END IF;

DELETE FROM table1
WHERE ROWNUM < 1000;

COMMIT;
v_cnt := 0;
END LOOP;
.
.
.
```