---
title: 'dbca doesn&#8217;t list diskgroups'
date: '2018-12-26T21:01:20+05:30'
status: publish
permalink: /2018/12/26/dbca-doesnt-list-diskgroups
author: Sidhu
excerpt: ''
type: post
id: 615
category:
    - Database
    - Exadata
tag:
    - dbca
    - Exadata
post_format: []
wpsd_autopost:
    - '1'
---
This is an Exadata machine running GI version 18.3.0.0.180717 and DB version 12.1.0.2.180717. On one of the DB nodes while running dbca, it doesn’t list the diskgroups. it works fine on the other node.

I cheked the dbca trace and found that the kfod command was failing. I tried to run it manually and got the same error:

```
<pre class="wp-block-code">```
[oracle@exadb01 ~]$ /u01/app/18.0.0.0/grid/bin/kfod op=groups verbose=true
KFOD-00300: OCI error [-1] [OCI error] [Could not fetch details] [-105777048]

KFOD-00105: Could not open pfile 'init@.ora'
[oracle@exadb01 ~]$
```
```

I ran it with strace then:

```
<pre class="wp-block-preformatted">[oracle@exadb01 ~]$ strace /u01/app/18.0.0.0/grid/bin/kfod op=groups verbose=true<br></br> execve("/u01/app/18.0.0.0/grid/bin/kfod", ["/u01/app/18.0.0.0/grid/bin/kfod", "op=groups", "verbose=true"], [/* 18 vars */]) = 0<br></br> brk(0)                                  = 0x2641000<br></br> .<br></br> .<br></br> .<br></br> .<br></br> .<br></br> <strong>open("/u01/app/18.0.0.0/grid/dbs/ab_+ASM1.dat", O_RDONLY) = -1 EACCES (Permission denied)</strong><br></br> geteuid()                               = 1003<br></br> open("/u01/app/18.0.0.0/grid/rdbms/mesg/kfodus.msb", O_RDONLY) = 13<br></br> fcntl(13, F_SETFD, FD_CLOEXEC)          = 0<br></br> lseek(13, 0, SEEK_SET)                  = 0<br></br> read(13, "\25\23\"\1\23\3\t\t\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0"…, 280) = 280<br></br> lseek(13, 512, SEEK_SET)                = 512<br></br> read(13, "\352\3\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0"…, 512) = 512<br></br> lseek(13, 1024, SEEK_SET)               = 1024<br></br> read(13, ".\1=\1E\1M\1X\1\352\3\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0"…, 512) = 512<br></br> lseek(13, 1536, SEEK_SET)               = 1536<br></br> read(13, "\n\0d\0\0\0D\0e\0\1\0e\0f\0\1\0\230\0g\0\1\0\306\0h\0\2\0\325\0"…, 512) = 512<br></br> fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(136, 3), …}) = 0<br></br> mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f43f85f2000<br></br> write(1, "KFOD-00300: OCI error [-1] [OCI "…, 78KFOD-00300: OCI error [-1] [OCI error] [Could not fetch details] [-132605848]<br></br> ) = 78
```

The text in bold just before the kfod error caught my attention. When I checked actually oracle user wasn’t able to read the file. The permissions looked like this:

```
<pre class="wp-block-preformatted">[root@exadb01 dbs]# ls -ltr<br></br> total 20<br></br> -rw-r--r-- 1 oragrid oinstall 3079 May 14  2015 init.ora<br></br> -rw-r--r-- 1 oragrid oinstall  587 Dec 12 15:33 initbackuppfile.ora<br></br><strong> -rw-rw---- 1 oragrid asmadmin 1656 Dec 20 14:26 ab_+ASM1.dat</strong><br></br> -rw-rw---- 1 oragrid oinstall 1544 Dec 20 14:26 hc_+APX1.dat<br></br> -rw-rw---- 1 oragrid oinstall 1544 Dec 21 16:57 hc_+ASM1.dat<br></br> [root@exadb01 dbs]#
```

Whereas on node2 they were like:

```
<pre class="wp-block-preformatted">[oracle@exadb02 dbs]$ ls -ltr <br></br> total 16<br></br> -rwxrwxrwx 1 oragrid oinstall 3079 Dec 12 14:52 init.ora<br></br> -rwxrwxrwx 1 oragrid oinstall 1544 Dec 21 16:57 hc_+ASM2.dat<br></br><strong> -rw-rw---- 1 oragrid oinstall 1720 Dec 21 16:57 ab_+ASM2.dat</strong><br></br> -rwxrwxrwx 1 oragrid oinstall 1544 Dec 21 16:57 hc_+APX2.dat<br></br> [oracle@exadb02 dbs]$
```

Since oracle user isn’t member of asmadmin group, it is not able to read the mentioned file. Changing the owner to oragrid:oinstall fixed the issue.