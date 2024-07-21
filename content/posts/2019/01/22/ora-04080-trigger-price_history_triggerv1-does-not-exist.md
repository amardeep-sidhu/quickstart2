---
title: 'ORA-04080: trigger &#8216;PRICE_HISTORY_TRIGGERV1&#8217; does not exist'
date: '2019-01-22T19:15:51+05:30'
status: publish
permalink: /2019/01/22/ora-04080-trigger-price_history_triggerv1-does-not-exist
author: Sidhu
excerpt: ''
type: post
id: 633
category:
    - Database
tag: []
post_format: []
wpsd_autopost:
    - '1'
---
It is actually a dumb one. I was disabling triggers in a schema and ran this SQL to generate the disable statements. (Example from [here](http://plsql-tutorial.com/plsql-triggers.htm))

<div class="wp-block-syntaxhighlighter-code ">```
<pre class="brush: sql; title: ; notranslate" title="">
HR@test> select 'alter trigger '||trigger_name|| ' disable;' from user_triggers where table_name='PRODUCT';

'ALTERTRIGGER'||TRIGGER_NAME||'DISABLE;'
--------------------------------------------------------------------------------
alter trigger PRICE_HISTORY_TRIGGERv1 disable;

HR@test> alter trigger PRICE_HISTORY_TRIGGERv1 disable;
alter trigger PRICE_HISTORY_TRIGGERv1 disable
*
ERROR at line 1:
ORA-04080: trigger 'PRICE_HISTORY_TRIGGERV1' does not exist


HR@test>
```

</div>WTF ? It is there but the disable didn’t work. I was in hurry, tried to connect through SQL developer and disable and it worked ! Double WTF ! Then i spotted the problem. Someone created it with one letter in the name in small. So to make it work, we need to use double quotes.

<div class="wp-block-syntaxhighlighter-code ">```
<pre class="brush: sql; title: ; notranslate" title="">
HR@test> alter trigger "PRICE_HISTORY_TRIGGERv1" disable;

Trigger altered.

HR@test>

```

</div>One of the reasons why you shouldn’t use case sensitive names in Oracle. That is stupid.