---
title: 'PRVF-4657 : Name resolution setup check for &#8220;db-scan&#8221; (IP address: x.x.x.101) failed'
date: '2020-09-25T19:41:28+05:30'
status: publish
permalink: /2020/09/25/prvf-4657-name-resolution-setup-check-for-db-scan-ip-address-x-x-x-101-failed
author: Sidhu
excerpt: ''
type: post
id: 747
category:
    - Exadata
    - RAC
tag:
    - Exadata
    - RAC
    - root.sh
post_format: []
---
A quick note about an error I faced while running root.sh on an Exadata machine. The configuration tools failed with the following error:

<div class="wp-block-syntaxhighlighter-code ">```
<pre class="brush: sql; title: ; notranslate" title="">
Error is PRVF-4657 : Name resolution setup check for "db-scan" (IP address: x.x.x.101) failed
```

</div>I did nslookup on the scan name and it all seemed good. So why the error ? After spending another 5 minutes, I looked at /etc/hosts and there was it. Someone had populated /etc/hosts of DB nodes with all the hostnames entries including the scan name. Something like:

<div class="wp-block-syntaxhighlighter-code ">```
<pre class="brush: bash; title: ; notranslate" title="">
x.x.x.101	db-scan.example.com	db-scan
x.x.x.102	db-scan.example.com	db-scan
x.x.x.103	db-scan.example.com	db-scan
```

</div>As /etc/hosts can return only one IP against a hostname whereas for scan, DNS is supposed to return 3 IPs, hence the problem. The solution is to comment out the scan name entries in /etc/hosts on all the db nodes and let the system do the name resolution via the DNS.