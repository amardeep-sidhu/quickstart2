---
title: 'Batch file for ftp&#8217;ing files&#8230;'
date: '2007-07-16T21:54:00+05:30'
status: publish
permalink: /2007/07/16/batch-file-for-ftping-files
author: Sidhu
excerpt: ''
type: post
id: 28
category:
    - Scripting
tag:
    - 'Batch files'
    - Scripting
post_format: []
---
Today I came across a requirement where users needed to ftp files time and again. So ftp’ing again and again is not a very good option. I wrote a small batch file for the same. Just sharing the same over here. I created a folder ftp in C drive and a file get\_file.bat  
Contents of get\_file.bat are:

```
<pre class="brush: css; title: ; notranslate" title="">

set /p file_name=Enter the name of the file you want to ftp:
echo oracle>c:\ftp\param.cfg
echo oracle123>>c:\ftp\param.cfg
echo cd /home/oracle>>c:\ftp\param.cfg
echo lcd c:\ftp>>c:\ftp\param.cfg
bin
get %file_name%
ftp -s:param.cfg 127.0.0.1

```

It will create a file param.cfg having all the things like username, password and command to get the file in the same folder (c:\\ftp). Then we invoke ftp with -s option with specifying the file param.cfg. It will ask the user to enter the file name and ftp the file from server to c:\\ftp

Sidhu