---
title: 'AVDF installation error'
date: '2019-12-05T20:18:12+05:30'
status: publish
permalink: /2019/12/05/avdf-installation-error
author: Sidhu
excerpt: ''
type: post
id: 670
category:
    - Database
tag:
    - AVDF
post_format: []
---
I was installing Database Firewall version 12.2.0.11.0 on a Dell x86 machine (with 5 \* 500 GB local HDDs configured in RAID 10) and it got successfully installed. Later on, I came to know that this version [doesn’t support ](https://docs.oracle.com/cd/E69292_01/doc.122/e41705/host_con.htm#SIGAD40830)host monitor functionality on Windows hosts. The latest version that supports that is 12.2.0.10.0. So that was the time to download and install 12.2.0.10.0. The installation started fine but it failed with an error:

```
<pre class="wp-block-code">```
Exception occured

anaconda 13.21.263 exception report

File "/usr/lib/anaconda/storage/devices.py",

OSError: [Errno 2] No such file or directory:
'/dev/sr0'
```
```

<figure class="wp-block-image size-large">![](https://i0.wp.com/amardeepsidhu.com/blog/wp-content/uploads/2019/12/avdf_error1-1.jpg?fit=1024%2C914)</figure>From the script that it is calling i.e. device.py, I guessed it had something to do with the storage. Maybe it was not able to figure out something that was created by the latest version installation. So I removed the RAID configuration and created it again. After this the installation went through without any issues.