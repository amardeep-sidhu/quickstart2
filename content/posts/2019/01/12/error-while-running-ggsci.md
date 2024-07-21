---
title: 'Error while running ggsci'
date: '2019-01-12T19:51:49+05:30'
status: publish
permalink: /2019/01/12/error-while-running-ggsci
author: Sidhu
excerpt: ''
type: post
id: 626
category:
    - GoldenGate
tag:
    - ggsci
    - GoldenGate
post_format: []
wpsd_autopost:
    - '1'
---
This was another issue that I faced while trying to configure GoldenGate in HA mode. ggsci was working fine after normal installation but after configuring it in HA mode and trying to run ggsci, it resulted in this:

```
<pre class="wp-block-preformatted">[oragg@node2 product]$ ggsci<br></br> Oracle GoldenGate Command Interpreter for Oracle<br></br> Version 12.3.0.1.4 OGGCORE_12.3.0.1.0_PLATFORMS_180415.0359_FBO<br></br> Linux, x64, 64bit (optimized), Oracle 12c on Apr 16 2018 00:53:30<br></br> Operating system character set identified as UTF-8.<br></br> Copyright (C) 1995, 2018, Oracle and/or its affiliates. All rights reserved.<br></br> 2019-01-08 16:28:37.913<br></br> CLSD: An error occurred while attempting to generate a full name. Logging may not be active for this process<br></br> Additional diagnostics: CLSU-00100: operating system function: sclsdgcwd failed with error data: -1<br></br> CLSU-00103: error location: sclsdgcwd2<br></br> (:CLSD00183:)<br></br> GGSCI (node2) 1>
```

No obvious clues in the error message but little searching revealed that it had something to do with permissions. It was on Exadata so i tried to do a strace of ggsci and see if it could give some clues. There we go:

```
<pre class="wp-block-preformatted">[oragg@node2 product]$ strace ggsci<br></br>.<br></br>.<br></br>mkdir("/u01/app/oracle/product/12.1.0.2/dbhome_4/log/exadatadb02", 01777) = -1 EACCES (Permission denied)
```

That is the Oracle database home, GoldenGate is supposed to use. It is trying to create a directory at the mentioned path and not able to do it. There was another directory called **client** needed inside this. I created both of them and set the needed permissions &amp; the sticky bit and it worked fine. It was working fine on the other node, so i could check the permissions over there and do the same on this node.