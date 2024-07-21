---
title: 'Implementing ZDLRA &#8211; Part 2'
date: '2020-10-06T18:06:53+05:30'
status: publish
permalink: /2020/10/06/implementing-zdlra-part-2
author: Sidhu
excerpt: ''
type: post
id: 755
category:
    - ZDLRA
tag:
    - backups
    - Oracle
    - zdlra
post_format: []
---
In [part 1](https://amardeepsidhu.com/blog/2020/09/09/implementing-zdlra-part-1/), we discussed few things that you should take care before implementation of a ZDLRA. In this post, we will discuss few more things that you should review before or at the time of implementation:

1. If you are getting two ZDLRAs (one each for primary and standby sites), there are two ways they can be deployed. One scenario is where all the primary databases (or the database that have no standby) backup to RA at the primary site and then the data is replicated from primary RA to RA at the standby site. This works well for the DBs that have no standby database. For the DBs where there is a standby database, there is a better architecture that can be deployed. In that scenario, primary databases backup to primary RA and the standby databases backup to standby RA. That saves you all the traffic over replication network. Oracle has published a whitepaper on how to do this configuration. Few of the instructions in this paper are a bit dated but it gives a good overall idea of how to do the implementation.
2. Keep an eye on the features supported for different DB versions. An interesting one is that real-time redo shipping from standby databases is supported on 12c+ databases only. It is not supported for 11g. There could be other similar things. MOS note 1995866.1 has these details.
3. Depending upon the ZDLRA software version being deployed, it may need a minimum version of EM and the ZDLRA plugin. MOS note 2542836.1 has these details.
4. Make sure after discovering the the primary and standby databases in EM, their primary-standby relationship is reflected.
5. Real-time redo sent to ZDLRA is compressed but the archive logs backup will be compressed only if you use compression in the RMAN command. It is always good to include backup archivelog command with daily incremental job to make sure that no archive log is missed.
6. Many of the environments have separate networks for backup traffic. Make sure the backup traffic to ZDLRA uses DB server’s backup network. If that is not the case, you may need to add an explicit route on DB server for ZDLRA client/VIP/scan IPs.
7. There are going to be different users that you will need to use: one OS user for deploying the EM agent, one DB user that will be used to run the backups. Depending upon your environment, it may oracle OS user, SYS DB user or could be some other named user created for this purpose.

In next few posts, we will discuss some of the issues I have faced while doing ZDLRA implementation for some customers.

PS: Fernando Simon has written some brilliant [posts related to ZDLRA](https://www.fernandosimon.com/blog/category/engineeredsystems/zdlra/) on his [blog](https://www.fernandosimon.com/blog/). I highly recommend to review all of them. Brilliant stuff.