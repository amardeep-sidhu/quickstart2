---
title: 'Implementing ZDLRA &#8211; Part 1'
date: '2020-09-09T18:33:39+05:30'
status: publish
permalink: /2020/09/09/implementing-zdlra-part-1
author: Sidhu
excerpt: ''
type: post
id: 735
category:
    - ZDLRA
tag:
    - backups
    - Oracle
    - zdlra
post_format: []
---
Zero Data Loss Recovery Appliance (ZDLRA) is Oracle’s solution for database backups. It has many advantages over other backup solutions that are available in the market. [This post](https://blogs.oracle.com/frankwickham/zero-data-loss-recovery-appliance-zdlra) has a brief introduction to ZDLRA and few links for further reading. This is a quick post about few of things that you should keep in mind if you are planning to get a ZDLRA (RA in short). Of course, there is a lot more that is needed while executing the whole plan, but these are some of the basics:

1. The very first thing is capacity planning. Depending upon the number &amp; sizes of the DBs that you plan to backup, you need to choose the required configuration. In most cases, an Oracle guy would be doing this for you but you should actively participate in the exercise by providing all the necessary information so that the calculations can be as accurate as possible.
2. Another things that plays an important role in deciding the capacity needed is the retention period i.e. period for which you would like to keep the backups in RA. More the number of days, more is the space that you will need.
3. Another important thing to consider is whether you are getting only one RA (for primary or standby site) or getting two of them i.e. one each for primary and standby site. Both scenarios need different type of configurations (including the bandwidth requirements between primary and standby sites) so it needs to be planned accordingly.
4. One more aspect you need to consider is long term retention. It could be Oracle Cloud object storage or some tape solution.
5. Once you have enabled DB backups to ZDLRA, you will need to stop all other backups. Plan that accordingly. Oracle provides way to run the legacy and ZDLRA backups together but that is for short duration i.e. when you are migrating from legacy backups to ZDLRA. That is not really a way to run 2 backup strategies together for long term.

In the [next post](https://amardeepsidhu.com/blog/2020/10/06/implementing-zdlra-part-2/), will talk about few more things that are important at the time of actual implementation.