---
title: 'Using NTLDR to boot Linux&#8230;'
date: '2007-05-06T09:26:00+05:30'
status: publish
permalink: /2007/05/06/using-ntldr-to-boot-linux
author: Sidhu
excerpt: ''
type: post
id: 15
category:
    - Unix/Linux
tag:
    - ntldr
    - Unix/Linux
post_format: []
blogger_blog:
    - amardeepsidhu.blogspot.com
blogger_author:
    - Sidhu
blogger_permalink:
    - /2007/05/using-ntldr-to-boot-linux.html
aktt_tweeted:
    - '1'
---
Sometimes, while installing Linux, installing LILO/GRUB to MBR makes you run into loads of issues, one of the most popular being that after reboot you are not able to boot into either of the OS 😉 There is a way to use NTLDR to boot Linux, this way the MBR remains untouched and if you don’t want to see the Linux option, what you need to do is, edit your boot.ini and your are done.

For this while installing GRUB, install it to the partition where you are installing Linux, instead of MBR. Now after rebooting, Linux will not boot as the partition on which you installed GRUB is not active. What actually we will do is, copy first 512 bytes of the partition where GRUB is installed, make a bin file, copy it to C drive and add the path of the same to boot.ini. So now, when you select Linux from the list of options displayed, NTLDR will call GRUB and then GRUB will boot Linux, just like normal.

There is a small utility called bootpart that does this all for us. [Here](http://www.winimage.com/bootpa26.zip) is the direct link. Extract it and go to command prompt.

```
<pre class="brush: css; title: ; notranslate" title="">C:\bootpart>dir

Volume in drive C has no label.
Volume Serial Number is 08E8-E412

Directory of C:\bootpart

05/05/2007  04:19 PM              .
05/05/2007  04:19 PM              ..
08/01/2005  02:26 AM              32bits
08/01/2005  02:06 AM            44,544 bootpart.exe
08/01/2005  02:06 AM            12,055 bootpart.txt
08/01/2005  02:06 AM               119 bootpart.url
08/01/2005  02:06 AM               383 file_id.diz
4 File(s)         57,101 bytes
3 Dir(s)   4,188,106,752 bytes free

C:\bootpart>
```

Run <span style="font-weight: bold">bootpart</span>, it will show all the partitions on the disk like

```
<pre class="brush: css; title: ; notranslate" title="">
C:\bootpart>bootpart
Boot Partition 2.60 for WinNT/2K/XP (c)1995-2005 G. Vollant (info@winimage.com)
WEB : http://www.winimage.com and http://www.winimage.com/bootpart.htm
Add partition in the Windows NT/2000/XP Multi-boot loader
Run "bootpart /?" for more information

Physical number of disk 0 : 282d282d
0 : C:* type=7  (HPFS/NTFS), size= 25599546 KB, Lba Pos=63
1 : C:  type=c  (Win95 Fat32 LBA), size= 11751547 KB, Lba Pos=208828935
2 : C:  type=d7 , size= 1052257 KB, Lba Pos=232332030
3 : C:  type=f  (Win95 XInt 13 extended), size= 78814890 KB, Lba Pos=51199155
4 : C:  type=7   (HPFS/NTFS), size= 25607578 KB, Lba Pos=51199218
5 : C:  type=5   (Extended), size= 35736592 KB, Lba Pos=102414375
6 : C:  type=7    (HPFS/NTFS), size= 35736561 KB, Lba Pos=102414438
7 : C:  type=5    (Extended), size= 15366172 KB, Lba Pos=173887560
8 : C:  type=83     (Linux native), size= 15366141 KB, Lba Pos=173887623
9 : C:  type=5     (Extended), size= 2104515 KB, Lba Pos=204619905
10 : C:  type=82      (Linux swap), size= 2104483 KB, Lba Pos=204619968
```

Now the one at 8th number is my native Linux partition. Now run <span style="font-weight: bold">bootpart 8 c:\\linux.bin</span> (Here 8 is my Linux partition number and linux.bin is the name of the file which it will create in C drive) It automatically adds the entry to boot.ini So now you are ready to go. Just reboot and you will see 2 options there. Windows &amp; Linux 🙂

Happy NTLDR’ing…

Sidhu