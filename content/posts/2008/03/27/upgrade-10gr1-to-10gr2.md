---
title: 'Upgrade 10gR1 to 10gR2 &#8211; DBUA error'
date: '2008-03-27T22:53:01+05:30'
status: publish
permalink: /2008/03/27/upgrade-10gr1-to-10gr2
author: Sidhu
excerpt: ''
type: post
id: 66
category:
    - Troubleshooting
tag:
    - 10g
    - Database
    - DBUA
    - Upgrade
post_format: []
aktt_tweeted:
    - '1'
---
Today I was upgrading 10gR1 to 10gR2 (10.2.0.1) on Linux x86. The upgrade went almost fine (except that I had to install one package and change few kernel parameters) but while running DBUA to upgrade databases, it gave an error:

```
<pre class="brush: css; title: ; notranslate" title="">Could not get database version from the Oracle Server component. The CEP file rdbmsup.sql does not provide the version directive

and

Start of root element expected. Upgrade Configuration file
'C:\Oracle10g2\cfgtoollogs\dbua\test\upgrade5\upgrade.xml' is not a valid XML file.
```

I searched in the metalink and found that this all happens due to customized glogin.sql file which was there in my case also. And removing that customization made DBUA rock 🙂

You might want to check [here](https://metalink.oracle.com/metalink/plsql/f?p=200:27:2925108627239218163::::p27_id,p27_show_header,p27_show_help:698669.994,1,1), [here](https://metalink.oracle.com/metalink/plsql/f?p=200:27:2925108627239218163::::p27_id,p27_show_header,p27_show_help:640602.993,1,1) and [here](https://metalink.oracle.com/metalink/plsql/f?p=130:14:2925108627239218163::::p14_database_id,p14_docid,p14_show_header,p14_show_help,p14_black_frame,p14_font:NOT,435453.1,1,1,1,helvetica).