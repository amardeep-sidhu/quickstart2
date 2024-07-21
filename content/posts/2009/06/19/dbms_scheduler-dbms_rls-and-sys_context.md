---
title: 'DBMS_SCHEDULER, DBMS_RLS and SYS_CONTEXT'
date: '2009-06-19T23:42:40+05:30'
status: publish
permalink: /2009/06/19/dbms_scheduler-dbms_rls-and-sys_context
author: Sidhu
excerpt: ''
type: post
id: 221
category:
    - PL/SQL
    - Troubleshooting
tag:
    - DBMS_RLS
    - DBMS_SCHEDULER
    - SYS_CONTEXT
    - VPD
post_format: []
aktt_notify_twitter:
    - 'yes'
syntaxhighlighter_encoded:
    - '1'
aktt_tweeted:
    - '1'
---
Today one of my colleague was working on development of a screen in Oracle Forms to give the end user an option to schedule a job using dbms\_scheduler. With the hope that i would be able to explain it properly, the whole scenario is like this:

1. User will log in to the application with his username (Lets say USER01) and password (basically every application user is a database user).
2. He is provided with a screen where he can enter details about the job and the code behind the button calls a PL/SQL procedure in the main application schema (lets say APP1) which in turn uses DBMS\_SCHEDULER.CREATE\_JOB to schedule the new job.
3. The ultimate task of the job is to move data from one table in the first database to a table in the second database using a DB Link.
4. There is a VPD policy applied on all the application users to restrict the view of data. Policy function uses SYS\_CONTEXT to fetch some information about the logged in user. The main application user APP1 is exempted from policy and can see the whole data.

Things seem to work fine till the schedule part. But when the job runs it hits ***ORA-02070: database does not support operator SYS\_CONTEXT in this context*** as SYS\_CONTEXT and DB link doesn’t go together.

I did a bit of troubleshooting and came to know that the job gets created with JOB\_CREATOR (a field in DBA\_SCHEDULER\_JOBS) as the user who is logged in (ie USER001). Now when the job runs from USER001, there is a VPD policy which is going to append a where clause to the query and there is a DB link being used, hence **ORA-02070**.

So the way out would be to schedule and run the job from some user that has no VPD policy applied to it. The best choice would obviously be the main application user; APP1 but as the user logs in with his own username so the job would always be created with JOB\_CREATOR as USER001. After a bit of thought provoking an idea hit me:

Create a table in the APP1 schema. Now when the user schedules the job, insert the values of the parameters required to schedule the job in the table. Schedule one master job in APP1 schema which would read this table and in turn call DBMS\_SCHEDULER.CREATE\_JOB to schedule the job required by the user. Now as there is no policy applied on the APP1 database user so the job is not going to hit **ORA-02070**. The frequency of the master job can be set as per the requirements. To identify which entries in the table have been processed either keep a flag which can be updated or delete the record from the table after scheduling.

That is how it clicked in my mind at that time. Suggestions about any other better (or worse 😉 ) methods are welcome 🙂

**PS:** About the title: Nothing really was coming into my mind so i picked up the all three words and titled it DBMS\_SCHEDULER, DBMS\_RLS and SYS\_CONTEXT 🙂