---
title: 'Accessing outside DocumentRoot files in Apache HTTP server'
date: '2008-07-22T21:51:10+05:30'
status: publish
permalink: /2008/07/22/accessing-outside-documentroot-files-in-apache-http-server-2
author: Sidhu
excerpt: ''
type: post
id: 95
category:
    - Others
tag:
    - 'apache web server'
    - DocumentRoot
post_format: []
aktt_tweeted:
    - '1'
---
I don’t know a bit about Apache HTTP server but faced one issue in office…so thought about writing it here 😉

We are having a 3 tier setup where Oracle Application Server 10g was there on AIX 5.3. It runs Apache HTTP server and we needed to access the files outside DocumentRoot. A bit of googling revealed that we could use [Alias](http://httpd.apache.org/docs/2.2/mod/mod_alias.html#alias) for that. Basically we need to add the following small piece of text to httpd.conf file:

```
<pre class="brush: css; title: ; notranslate" title="">
Alias /test1/ "/home/sidhu/test1/"
<Directory "/home/sidhu/test1">
Options Indexes FollowSymLinks
AllowOverride None
Order allow,deny
Allow from all
</Directory>
```

Now we should be able to access the files in /home/sidhu/test1 by hitting the following URL

http://server:port/test1/filename.html

PS: I couldn’t find the reason but it didn’t allow me to use word “reports” in the directory name.