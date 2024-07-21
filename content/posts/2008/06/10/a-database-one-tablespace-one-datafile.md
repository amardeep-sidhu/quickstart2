---
title: 'A database &#8211; one tablespace &#038; one datafile'
date: '2008-06-10T21:32:24+05:30'
status: publish
permalink: /2008/06/10/a-database-one-tablespace-one-datafile
author: Sidhu
excerpt: ''
type: post
id: 79
category:
    - 'Oracle General'
tag:
    - Database
post_format: []
aktt_tweeted:
    - '1'
---
Today I was checking OTN forums and came across [a thread](http://forums.oracle.com/forums/message.jspa?messageID=2578116). OP’s concern was:

*One of the db i am supporting has about 3.3 Terabytes capacity and the application is using only 1 huge tablespace with one big file.*

*the system is linux 4 , 32 bit.  
oracle version is 10.2.0.4*

*Is there a limit of space for a tablespace when you consider insert/delete/query performance?*

Single datafile of 3.3 TB 🙂

Awesome !

Kudos to the designer 😉