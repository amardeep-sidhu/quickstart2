---
title: 'Moving datafiles,control files and log files &#8211; Part 1'
date: '2007-07-21T13:44:00+05:30'
status: publish
permalink: /2007/07/21/moving-datafilescontrol-files-and-log-files-1
author: Sidhu
excerpt: ''
type: post
id: 29
category:
    - 'Oracle Tips'
tag:
    - datafiles
    - tablespaces
post_format: []
blogger_blog:
    - amardeepsidhu.blogspot.com
blogger_author:
    - Sidhu
blogger_permalink:
    - /2007/07/moving-data-filescontrol-files-and-log.html
---
Many times you need to move datafiles from one location to another. The simplest approach for this is to take the tablespace offline, copy the datafiles to new location, rename the files with *alter database rename file* (Except that you dont have to move the SYSTEM and UNDO tablespace, as you can’t take SYSTEM tablespace offline)

```
<pre class="brush: css; title: ; notranslate" title="">

SYS@orcl AS SYSDBA> alter tablespace system offline;
alter tablespace system offline
*
ERROR at line 1:
ORA-01541: system tablespace cannot be brought offline; shut down if necessary

SYS@orcl AS SYSDBA>

```

Well lets try moving USERS tablespace.

```
<pre class="brush: css; title: ; notranslate" title="">

SYS@orcl AS SYSDBA> column file_name format a50

SYS@orcl AS SYSDBA> set lines 100
SYS@orcl AS SYSDBA> select file_name,tablespace_name from dba_data_files where tablespace_name='USERS';

FILE_NAME                                          TABLESPACE_NAME
-------------------------------------------------- ------------------------------
C:\ORACLE\ORCL\USERS01.DBF                         USERS

SYS@orcl AS SYSDBA>
```

The current location of the datafile is C:\\ORACLE\\ORCL\\. Suppose I have to move it to c:\\oracle\\oradata. So first lets take the tablespace offline

```
<pre class="brush: css; title: ; notranslate" title="">

SYS@orcl AS SYSDBA> alter tablespace users offline;

Tablespace altered.

SYS@orcl AS SYSDBA>
```

Now copy the datafile to new location \[Note the new directory should be already created\]

```
<pre class="brush: css; title: ; notranslate" title="">

C:\oracle\ORCL>copy USERS01.DBF c:\oracle\oradata
1 file(s) copied.

C:\oracle\ORCL>
```

Now make database aware of the new location of the datafile using <span style="font-style: italic">alter database rename file</span>:

```
<pre class="brush: css; title: ; notranslate" title="">

SYS@orcl AS SYSDBA> alter database rename file 'c:\oracle\orcl\users01.dbf' to 'c:\oracle\oradata\users01.dbf';

Database altered.

SYS@orcl AS SYSDBA>
```

The last thing is to bring the tablespace online. If everything has gone rightly the message like this will appear and you can view the new location of the datafile.

```
<pre class="brush: css; title: ; notranslate" title="">

SYS@orcl AS SYSDBA> alter tablespace users online;

Tablespace altered.

SYS@orcl AS SYSDBA> select file_name,tablespace_name from dba_data_files where tablespace_name='USERS';

FILE_NAME                                          TABLESPACE_NAME
-------------------------------------------------- ------------------------------
C:\ORACLE\ORADATA\USERS01.DBF                      USERS

SYS@orcl AS SYSDBA>
```

In next post we will discuss moving datafiles, controlfiles and logfiles.