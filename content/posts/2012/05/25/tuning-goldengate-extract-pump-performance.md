---
title: 'Tuning GoldenGate Extract Pump performance'
date: '2012-05-25T20:38:18+05:30'
status: publish
permalink: /2012/05/25/tuning-goldengate-extract-pump-performance
author: Sidhu
excerpt: ''
type: post
id: 387
category:
    - GoldenGate
tag:
    - 'Extract Pump'
    - GoldenGate
    - Performance
post_format: []
aktt_notify_twitter:
    - 'yes'
wpsd_autopost:
    - '1'
aktt_tweeted:
    - '1'
---
Just a quick note/post about the significance of COMPRESS and TCPBUFSIZE parameter in performance of a GoldenGate Extract Pump process. COMPRESS helps in compressing the outgoing blocks hence helping in better utilization of the bandwidth from source to target. GG is going to uncompress the blocks before writing them to the remote trail file on the target. Compression ratios of 4:1 or better can be achieved. Of course, use of COMPRESS may result in increased CPU usage on both the sides.

TCPBUFSIZE controls the size of the TCP buffer socket that is going to be used by the Extract. If the bandwidth allows, it will be a good idea to send larger packets. So depending upon the available bandwidth one can experiment with the values of TCPBUFSIZE. At one of the client sites, I saw a great increase in the performance after setting TCPBUFSIZE. The trail file (10 MB size) that was taking almost a minute to transfer started getting through in few seconds after setting this parameter. Documentation ([http://docs.oracle.com/cd/E35209\_01/doc.1121/e29399.pdf](http://docs.oracle.com/cd/E35209_01/doc.1121/e29399.pdf) page 313) provides the method to calculate the optimum value for TCPBUFSIZE for your environment.

While using TCPBUFSIZE value for TCPFLUSHBYTES (at least equal to the value of TCPBUFSIZE) also needs to be set. It is the buffer that collects the data that is going to be transferred to the target.

These parameters can be used like following:

```
rmthost, mgrport, compress, tcpbufsize 10000, tcpflushbytes 10000
```

Also see the metalink note **1071892.1**.