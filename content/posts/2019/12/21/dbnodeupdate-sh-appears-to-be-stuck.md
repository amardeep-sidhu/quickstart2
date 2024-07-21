---
title: 'dbnodeupdate.sh appears to be stuck'
date: '2019-12-21T06:37:33+05:30'
status: publish
permalink: /2019/12/21/dbnodeupdate-sh-appears-to-be-stuck
author: Sidhu
excerpt: ''
type: post
id: 679
category:
    - Exadata
tag:
    - Exadata
    - Patching
post_format: []
---
I was patching an Exadata db node from 18.1.5.0.0.180506 to 19.3.2.0.0.191119. It had been more than an hour and dbnodeupdate.sh appeared to be stuck. Trying to ssh to the node was giving “connection refused” and the console had this output (some output removed for brevity):

```
<pre class="wp-block-code">```
[  458.006444] upgrade[8876]: [642/676] (72%) installing exadata-sun-computenode-19.3.2.0.0.191119-1...
<>
[  459.991449] upgrade[8876]: Created symlink /etc/systemd/system/multi-user.target.wants/exadata-iscsi-reconcile.service, pointing to /etc/systemd/system/exadata-iscsi-reconcile.service.
[  460.011466] upgrade[8876]: Looking for unit files in (higher priority first):
[  460.021436] upgrade[8876]: /etc/systemd/system
[  460.028479] upgrade[8876]: /run/systemd/system
[  460.035431] upgrade[8876]: /usr/local/lib/systemd/system
[  460.042429] upgrade[8876]: /usr/lib/systemd/system
[  460.049457] upgrade[8876]: Looking for SysV init scripts in:
[  460.057474] upgrade[8876]: /etc/rc.d/init.d
[  460.064430] upgrade[8876]: Looking for SysV rcN.d links in:
[  460.071445] upgrade[8876]: /etc/rc.d
[  460.076454] upgrade[8876]: Looking for unit files in (higher priority first):
[  460.086461] upgrade[8876]: /etc/systemd/system
[  460.093435] upgrade[8876]: /run/systemd/system
[  460.100433] upgrade[8876]: /usr/local/lib/systemd/system
[  460.107474] upgrade[8876]: /usr/lib/systemd/system
[  460.114432] upgrade[8876]: Looking for SysV init scripts in:
[  460.122455] upgrade[8876]: /etc/rc.d/init.d
[  460.129458] upgrade[8876]: Looking for SysV rcN.d links in:
[  460.136468] upgrade[8876]: /etc/rc.d
[  460.141451] upgrade[8876]: Created symlink /etc/systemd/system/multi-user.target.wants/exadata-multipathmon.service, pointing to /etc/systemd/system/exadata-multipathmon.service.
```
```

There was not much that I could do so just waited. Also created an SR with Oracle Support and they also suggested to wait. It started moving after some time and completed successfully. Finally when the node came up, i checked that there was an NFS mount entry in /etc/rc.local and that was what created the problem. For the second node, we commented this out and it was all smooth. Important to comment out all NFS entries during patching to avoid all such issues. I had commented the ones in /etc/fstab but the one in rc.local was an unexpected one.