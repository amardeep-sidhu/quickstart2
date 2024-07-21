---
title: 'File system already present at specified mount point /dbfs_direct'
date: '2021-06-05T20:19:37+05:30'
status: publish
permalink: /2021/06/05/file-system-already-present-at-specified-mount-point-dbfs_direct
author: Sidhu
excerpt: ''
type: post
id: 889
category:
    - Exadata
tag:
    - dbfs
    - Exadata
post_format: []
---
It was actually funny. So thought about posting it that sometimes how we can miss the absolute basics. This customer is using a virtualized Exadata with multiple VMs. One VM hosts the database meant to be used for dbfs and another VM connects to this DB over IB to mount dbfs file system using dbfs\_client. One day VMs were rebooted and due to some reason the dbfs filesystem didn’t mount on startup. It went on for few days and they couldn’t mount it. One day I got a chance to look at it and the error they were facing was:

<div class="wp-block-syntaxhighlighter-code ">```
<pre class="brush: bash; title: ; notranslate" title="">
File system already present at specified mount point /dbfs_direct
```

</div>If you are familiar with Unix, this clearly indicates some problem with the directory where it is trying to mount the file system. I checked that and there were some files in /dbfs\_direct. I moved those files and it was able to mount. Issue resolved.

Then after closing the session, I was thinking that what could have happened as this dbfs mount point has been in use for long. Then it struck me. It was being used to take some RMAN backups and the path was hard coded in the scripts. When it didn’t mount after the reboot (i don’t know why), someone ran that script and whatever directories it didn’t find, it probably created that complete directory structure and tried to write to a log file. Once /dbfs\_direct had those files, it anyway was not going to mount.