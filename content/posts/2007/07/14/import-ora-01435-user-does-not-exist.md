---
title: 'Import ORA-01435: user does not exist&#8230;'
date: '2007-07-14T09:29:00+05:30'
status: publish
permalink: /2007/07/14/import-ora-01435-user-does-not-exist
author: Sidhu
excerpt: ''
type: post
id: 27
category:
    - 'Oracle Tips'
tag: []
post_format: []
blogger_blog:
    - amardeepsidhu.blogspot.com
blogger_author:
    - Sidhu
blogger_permalink:
    - /2007/07/import-ora-01435-user-does-not-exist.html
---
One may encounter this error while importing from a dmp file from older versions of Oracle. Genereally this error is caused by some statement like <span style="font-style: italic;">alter session set current\_schema=scott; </span>And the simple reason is that the user <span style="font-style: italic;">scott</span> doesn’t exist. Yesterday I came across this error. And the reason was that user was not created. As in case of import we generally create tablespaces first (by creating the DDL using option show=Y) but creation of users is done by import itself. In older versions of Oracle, the temp tablespaces were no different from other tablespaces. But in newer versions temp tablespaces are different. So in dmp files from thoese older versions <span style="font-style: italic;">create user </span>statements are written like <span style="font-style: italic;">create user t1 identified by t1 default tablespace temp temporary tablespace temp. </span>This thing worked fine in older versions but in newer versions we cannot specify the TEMP tablespace as the default tablespace for a user. So the statement create <span style="font-style: italic;">user t2 identified by t2 default tablespace temp temporary tablespace temp</span> throws <span style="font-style: italic;">ORA-12910: cannot specify temporary tablespace as default tablespace. </span>In such cases the users (for which default statement is specified as TEMP) have to be created manually by specifying the appropriate tablespace as default tablespace and then the import should be run with ignore=Y.

Sidhu