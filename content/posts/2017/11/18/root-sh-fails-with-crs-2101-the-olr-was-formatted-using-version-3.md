---
title: 'root.sh fails with CRS-2101:The OLR was formatted using version 3'
date: '2017-11-18T16:03:33+05:30'
status: publish
permalink: /2017/11/18/root-sh-fails-with-crs-2101-the-olr-was-formatted-using-version-3
author: Sidhu
excerpt: ''
type: post
id: 586
category:
    - RAC
    - Troubleshooting
tag:
    - RAC
    - Troubleshooting
post_format: []
wpsd_autopost:
    - '1'
---
Got this while trying to install 11.2.0.4 RAC on Redhat Linux 7.2. root.sh fails with a message like

```
<pre class="brush: sql; title: ; notranslate" title="">ohasd failed to start
Failed to start the Clusterware. Last 20 lines of the alert log follow:
2017-11-09 15:43:37.883:
[client(37246)]CRS-2101:The OLR was formatted using version 3.
```

This is bug 18370031. Need to apply the patch before running root.sh.