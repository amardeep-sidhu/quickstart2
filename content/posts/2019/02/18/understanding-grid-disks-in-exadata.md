---
title: 'Understanding grid disks in Exadata'
date: '2019-02-18T18:37:25+05:30'
status: publish
permalink: /2019/02/18/understanding-grid-disks-in-exadata
author: Sidhu
excerpt: ''
type: post
id: 638
category:
    - Exadata
tag:
    - 'cell disks'
    - Exadata
    - 'grid disks'
post_format: []
wpsd_autopost:
    - '1'
---
Use of Exadata storage cells seems to be a very poorly understood concept. A lot of people have confusions about how exactly ASM makes uses of disks from storage cells. Many folks assume there is some sort of RAID configured in the storage layer whereas there is nothing like that. I will try to explain some of the concepts in this post.

Let’s take an example of an Exadata quarter rack that has 2 db and 3 storage nodes (node means a server here). Few things to note:

- The space for binaries installation on db nodes comes from the local disks installed in db nodes (600GB \* 4 (expandable to 8) configured in RAID5). In case you are using OVM, same disks are used for keeping configuration files, Virtual disks for VMs etc.
- All of the ASM space comes from storage cells. The minimum configuration is 3 storage cells.

So let’s try to understand what makes a storage cell. There are 12 disks in each storage cell (latest X7 cells are coming with 10 TB disks). As I mentioned above that there are 3 storage cells in a minimum configuraiton. So we have a total of 36 disks. There is no RAID configured in the storage layer. All the redundancy is handled at ASM level. So to create a disk group:

- First of all cell disks are created on each storage cell. 1 physical disk makes 1 cell disk. So a quarter rack has 36 cell disks.
- To divide the space in various disk groups (by default only two disk groups are created : DATA &amp; RECO; you can choose how much space to give to each of them) grid disks are created. grid disk is a partition on the cell disk. slice of a disk in other words. Slice from each cell disk must be part of both the disk groups. We can’t have something like say DATA has 18 disks out of 36 and the RECO has another 18. That is not supported. Let’s say you decide to allocate 5 TB to DATA grid disks and 4 TB to RECO grid disks (out of 10 TB on each disk, approx 9 TB is what you get as usable). So you will divide each cell disk into 2 parts – 5 TB and 4 TB and you would have 36 slices of 5 TB each and 36 slices of 4 TB each.
- DATA disk group will be created using the 36 5 TB slices where grid disks from each storage cell constitute one failgroup.
- Similarly RECO disk group will be created using the 36 4 TB slices.

What we have discussed above is a quarter rack scenario with High Capacity (HC) disks. There can be somewhat different configurations too:

- Instead of HC disks, you can have the Extreme Flash (EF) configuration which uses flash cards in place of disks. Everything remains the same except the number. Instead of 12 HC disks there will be 8 flash cards.
- With X3 I think, Oracle introduced an eighth rack configuration. In an eighth rack configuration db nodes come with half the cores (of quarter rack db nodes) and storage cells come with 6 disks in each of the cell. So here you would have only 18 disks in total. Everything else works in the same way.

Hope it clarified some of the doubts about grid disks.