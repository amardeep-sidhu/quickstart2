---
title: 'Command line history in SQL (for Linux)&#8230;'
date: '2007-05-04T20:35:00+05:30'
status: publish
permalink: /2007/05/04/command-line-history-in-sql-for-linux
author: Sidhu
excerpt: ''
type: post
id: 14
category:
    - SQL
    - Unix/Linux
tag:
    - rlwrap
    - SQL
post_format: []
blogger_blog:
    - amardeepsidhu.blogspot.com
blogger_author:
    - Sidhu
blogger_permalink:
    - /2007/05/command-line-history-in-sql-for-linux.html
aktt_tweeted:
    - '1'
---
Found a very interesting article on Dizwell’s blog. It was about keeping history of the SQL commands in SQL Plus on Linux. It is almost very simple. Just need to download a small utility called rlwrap from [here](http://utopia.knoware.nl/%7Ehlub/rlwrap/). Its a tar.gz file. Download it, un-tar using

```
<pre class="brush: css; title: ; notranslate" title="">
tar -xvf rlwrap-0.28.tar.gz
```

It will create a directory with the same name. <span style="font-weight: bold">cd</span> to the directory and run

```
<pre class="brush: css; title: ; notranslate" title="">
./configure
```

Now do

```
<pre class="brush: css; title: ; notranslate" title="">
make install
```

(I was logged in as <span style="font-weight: bold">oracle </span>user, then did <span style="font-weight: bold">su, </span>but it gave some errors, finally I logged in as root and it worked fine)

Now what is left to be done is make an alias for sqlplus as

```
<pre class="brush: css; title: ; notranslate" title="">
alias sqlplus='rlwrap sqlplus'
```

Using up/down arrows, commands can be scrolled up and down just like windows. Have a look at full article [here](http://www.dizwell.com/prod/node/56).

Cheers

Sidhu