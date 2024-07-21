---
title: 'DML and HCC &#8211; Exadata'
date: '2011-12-22T00:01:11+05:30'
status: publish
permalink: /2011/12/22/dml-and-hcc-exadata
author: Sidhu
excerpt: ''
type: post
id: 379
category:
    - Exadata
tag:
    - Exadata
    - HCC
post_format: []
aktt_notify_twitter:
    - 'yes'
wpsd_autopost:
    - '1'
aktt_tweeted:
    - '1'
---
Hybrid Columnar Compression (HCC) is a new awesome feature in Exadata that helps in saving a lot of storage space in your environment. [This whitepaper](http://www.oracle.com/technetwork/middleware/bi-foundation/ehcc-twp-131254.pdf) on Oracle website explains this feature in detail. Also Uwe Hesse has an excellent *how to use all this* [post on his blog](http://uhesse.wordpress.com/2011/01/21/exadata-part-iii-compression/). You can see the compression levels one can achive by making use of HCC. It is very simple to use feature but one needs to be aware of few things before using HCC extensively as otherwise all your storage calculations may go weird. Here are few of the things to keep in mind:

- HCC works with direct path loads only that includes: CTAS, running impdp with ACCESS\_METHOD=DIRECT or direct path inserts. If you insert data using a normal insert, it will not be HCC compressed.


- It is most suited for tables that aren’t going to be updated once loaded. There are some complications (next point) that arise if some DML is going to be run on HCC compressed data.


- At block level HCC stores data as compression units. A compression unit can be defined as a set of blocks. Now if some rows (stored with HCC) are updated, they need to be decompressed first. Also in that case the database needs to read the compression unit, not a single block. So once you do some update on the data stored in HCC, it will be moved out of HCC compression. To HCC compress it again you will need to do *alter table table\_name move compress for* (Also see Metalink note 1332853.1)*.* So if the tables you are planning to use HCC upon, undergo frequent DML, HCC may not be best suited for that scenario. Not only it will add the additional overhead of running *alter table move* statement every time some updates happen, it may screw up the storage space calculations as well.