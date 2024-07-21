---
title: 'Issues in upgrading from 9i to 10g'
date: '2011-01-29T22:30:29+05:30'
status: publish
permalink: /2011/01/29/issues-in-upgrading-from-9i-to-10g
author: Sidhu
excerpt: ''
type: post
id: 334
category:
    - Troubleshooting
tag:
    - 10g
    - 9i
    - Upgrade
post_format: []
aktt_notify_twitter:
    - 'yes'
aktt_tweeted:
    - '1'
---
Last week I had a chance to upgrade a 9.2.0.7 database to 10.2.0.5. The size of the database was around 800 GB. The major applications connecting to the database were developed in Pro\*C and Oracle Forms. The upgrade itself pretty smooth but there were few glitches around that needed to be handled. Just thought about documenting all the issues:

- <div align="justify">Few users in the database were assigned the CREATE SESSION privilege through a password protected role (That role was the default role for that user). 10.2.0.5 onwards, password protected roles can’t be set as default roles. The alternate is to either disable the password for the role or assign CREATE SESSION directly to the user, not through a role.</div>
- <div align="justify">After the upgrade, few procedures became invalid and while compiling started giving **ORA-00918: COLUMN AMBIGUOUSLY DEFINED**. The issue was bug 2846640 which is fixed in 10.2. Actually, in few of the queries using ANSI syntax, the developer didn’t qualify the column names with table names. It worked fine in 9i but due to the bug getting fixed in 10g, it started giving ORA-00918. The simple solution is to prefix the column name with the table name.</div>
- <div align="justify">Few of the application schema owner users complained that they were not able to modify the procedures/packages in their own schemas. The schemas were not assigned CREATE PROCEDURE privilege but as per [documentation](http://download.oracle.com/docs/cd/B19306_01/network.102/b14266/authoriz.htm#sthref453), they should be able to modify the existing procedures/packages owned by them. This again is a documentation bug. It worked fine in 9i but in 10g onwards you need to have either a CREATE PROCEDURE or ALTER ANY PROCEDURE privilege (a risky one) to be able to edit the PL/SQL units in your own schema. </div>

These were few of the issues encountered, rest of the upgrade was super smooth !

Happy upgrading !