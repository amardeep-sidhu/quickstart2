---
title: 'IMPORT &amp; EXPORT from/to compressed files directly&#8230;'
date: '2007-06-06T20:57:00+05:30'
status: publish
permalink: /2007/06/06/import-export-fromto-compressed-files-directly
author: Sidhu
excerpt: ''
type: post
id: 19
category:
    - 'Oracle Tips'
tag:
    - Export/Import
post_format: []
blogger_blog:
    - amardeepsidhu.blogspot.com
blogger_author:
    - Sidhu
blogger_permalink:
    - /2007/06/import-export-fromto-compressed-files.html
aktt_tweeted:
    - '1'
---
Well, a simple method to import(export) directly to(from) compressed files using pipes. Its for Unix based systems only, as I am not aware of any pipe type functionality in Windows. The biggest advantage is that you can save lots of space as uncompressing a file makes it almost 5 times or more. (Suppose you are uncompressing a file of 20 GB, it will make 100 GB) As a newbie I faced this problem, so thought about writing a post.

Lets talk about export first. The method used is that create a pipe, write to a pipe(ie the file in exp command is the pipe we created), side by side read the contents of pipe, compress (in the background) and redirect to a file. Here is the script that achieves this:

```
<pre class="brush: css; title: ; notranslate" title="">export ORACLE_SID=MYDB
rm -f ?/myexport.pipe
mkfifo ?/myexport.pipe
cat ?/myexport.pipe |compress > ?/myexport.dmp.Z &amp;amp;
sleep 5
exp file=?/myexport.pipe full=Y log=myexport.log

```

Same way for import, we create a pipe, zcat from the dmp.Z file, redirect it to the pipe and then read from pipe:

```
<pre class="brush: css; title: ; notranslate" title="">export ORACLE_SID=MYDB
rm -f ?/myimport.pipe
mkfifo ?/myimport.pipe
zcat ?/myexport.dmp.Z > ?/myimport.pipe &amp;amp;
sleep 5
imp file=myimport.pipe full=Y show=Y log=?/myimport.log
```

In case there is any issue with the script, do let me know 🙂

<span style="font-weight: bold">Update:</span> If you are on Wintel, you can directly use a compressed folder as an export target. No need to create a pipe as the file system will automatically do it for you. ( Thanks [Noons](http://dbasrus.blogspot.com/) for the tip)

**If you are using gunzip:**

For export:

```
<pre class="brush: css; title: ; notranslate" title="">export ORACLE_SID=MYDB
rm -f exp.pipe
mknod exp.pipe
gzip < exp.pipe > T1.dmp.gz &amp;amp;
exp file=exp.pipe full=Y log=myexport.log

```

For import:

```
<pre class="brush: css; title: ; notranslate" title="">export ORACLE_SID=MYDB
rm -f imp.pipe
mknod imp.pipe
gzip < T1.dmp.gz > imp.pipe &amp;amp;
imp file=imp.pipe full=Y log=myimport.log

```

**Update:** I came across an article that discusses few ways to achieve the same on Windows. Check it [here](http://jcarlossaez.spaces.live.com/blog/cns!B3378F057444B65C!108.entry).