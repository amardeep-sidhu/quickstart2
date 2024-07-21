---
title: 'Oracle RAC 12.1 &#8211; lsnodes exited with code 9'
date: '2017-03-28T22:01:35+05:30'
status: publish
permalink: /2017/03/28/oracle-rac-12-1-lsnodes-exited-with-code-9
author: Sidhu
excerpt: ''
type: post
id: 536
category:
    - RAC
tag:
    - '12.1'
    - RAC
    - 'Solaris Cluster'
post_format: []
---
I was trying to do a 2 node RAC setup on Solaris 11.3 where Oracle Solaris Cluster 4.3 was already configured. Installed was running but the Cluster Node Information screen was appearing like this

[![error](http://amardeepsidhu.com/blog/wp-content/uploads/2017/03/error_thumb.jpg "error")](http://amardeepsidhu.com/blog/wp-content/uploads/2017/03/error.jpg)

The install log shows this:

<font face="Courier New" size="2">INFO: Checking cluster configuration details</font>

**<font face="Courier New" size="2">INFO: Found Vendor Clusterware. Fetching Cluster Configuration</font>**

<font face="Courier New"><font size="2">**INFO: Executing \[/tmp/OraInstall2017-03-28\_12-50-48PM/ext/bin/lsnodes\]**</font></font>

<font face="Courier New" size="2">with environment variables {TERM=xterm, LC\_COLLATE=, SHLVL=3, JAVA\_HOME=, XFILESEARCHPATH=/usr/dt/app-defaults/%L/Dt, SSH\_CLIENT=172.16.64.55 56370 22, LC\_NUMERIC=, LC\_MESSAGES=, MAIL=/var/mail/oracle, PWD=/export/software/grid/grid, XTERM\_VERSION=XTerm(320), WINDOWID=2097165, LOGNAME=oracle, \_=\*50727\*/export/software/grid/grid/install/.oui, NLSPATH=/usr/dt/lib/nls/msg/%L/%N.cat, SSH\_CONNECTION=172.16.64.55 56370 172.16.72.18 22, OLDPWD=/export/oracle, LC\_CTYPE=, CLASSPATH=, PATH=/usr/bin:/usr/ccs/bin:/usr/bin:/bin:/export/software/grid/grid/install, LC\_ALL=, DISPLAY=localhost:10.0, LC\_MONETARY=, USER=oracle, HOME=/export/oracle, XTERM\_SHELL=/bin/bash, XAUTHORITY=/tmp/ssh-xauth-mlq21a/xauthfile, A\_\_z=”\*SHLVL, XTERM\_LOCALE=en\_US.UTF-8, TZ=localtime, LC\_TIME=, LANG=en\_US.UTF-8}</font>

<font face="Courier New" size="2">INFO: Starting Output Reader Threads for process /tmp/OraInstall2017-03-28\_12-50-48PM/ext/bin/lsnodes</font>

**<font face="Courier New" size="2">INFO: The process /tmp/OraInstall2017-03-28\_12-50-48PM/ext/bin/lsnodes exited with code 9</font>**

So we can see the problem. lsnodes is not able to list the nodes. Let us try to run that command manually.

<font face="Courier New">-bash-4.1$ export PATH=PATH=/usr/bin:/usr/ccs/bin:/usr/bin:/bin:/export/software/grid/grid/install</font>

<font face="Courier New">-bash-4.1$ </font><font face="Courier New">/tmp/OraInstall2017-03-28\_12-50-48PM/ext/bin/lsnodes</font>

<font face="Courier New">ld.so.1: lsnodes: fatal: libskgxn2.so: open failed: No such file or directory</font>

<font face="Courier New">Killed</font>

<font face="Courier New">-bash-4.1$</font>

So looks like it is not able to find this library called libskgxn2.so. If we do a find for this file name we can see that it is present in this directory /usr/cluster/lib/sparcv9/libskgxn2.so .

Some googling and MOS searches revealed that it expects the library to be present at /opt/ORCLcluster/lib. This directory doesn’t exist here. As a workaround we can create this directory manually and create symbolic link to file libskgxn2.so

The lsnodes command worked fine after this workaround and installer also shows both the nodes listed.