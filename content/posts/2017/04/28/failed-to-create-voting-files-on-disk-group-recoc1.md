---
title: 'Failed to create voting files on disk group RECOC1'
date: '2017-04-28T14:31:20+05:30'
status: publish
permalink: /2017/04/28/failed-to-create-voting-files-on-disk-group-recoc1
author: Sidhu
excerpt: ''
type: post
id: 541
category:
    - Exadata
    - RAC
tag:
    - Exadata
    - RAC
    - Voting
post_format: []
wpsd_autopost:
    - '1'
---
Long story short, faced this issue while running OneCommand for one Exadata system. The root.sh step (Initialize Cluster Software) was failing with the following error on the screen

<span style="font-family: 'Courier New';">Checking file root\_dm01dbadm02.in.oracle.com\_2017-04-27\_18-13-27.log on node dm01dbadm02.somedomain.com  
Error: Error running root scripts, please investigate…  
Collecting diagnostics…  
Errors occurred. Send /u01/onecommand/linux-x64/WorkDir/Diag-170427\_181710.zip to Oracle to receive assistance.</span>

Doesn’t make much sense. So let us check the log file of this step

<span style="font-family: 'Courier New';">2017-04-27 18:17:10,463 \[INFO\]\[ OCMDThread\]\[ ClusterUtils:413\] Checking file root\_dm01dbadm02.somedomain.com\_2017-04-27\_18-13-27.log on node inx321dbadm02.somedomain.com  
2017-04-27 18:17:10,464 \[INFO\]\[ OCMDThread\]\[ OcmdException:62\] Error: Error running root scripts, please investigate…  
2017-04-27 18:17:10,464 \[FINE\]\[ OCMDThread\]\[ OcmdException:63\] Throwing OcmdException… message:Error running root scripts, please investigate…</span>

So we need to go to root.sh log file now. That shows

<span style="font-family: 'Courier New';">Failed to create voting files on disk group RECOC1.  
Change to configuration failed, but was successfully rolled back.  
CRS-4000: Command Replace failed, or completed with errors.  
Voting file add failed  
2017/04/27 18:16:37 CLSRSC-261: Failed to add voting disks</span>

<span style="font-family: 'Courier New';">Died at /u01/app/12.1.0.2/grid/crs/install/crsinstall.pm line 2068.  
The command ‘/u01/app/12.1.0.2/grid/perl/bin/perl -I/u01/app/12.1.0.2/grid/perl/lib -I/u01/app/12.1.0.2/grid/crs/install /u01/app/12.1.0.2/grid/crs/install/root  
crs.pl ‘ execution failed</span>

Makes some senses but we can’t understand what happened while creating Voting files on RECOC1. Let us check ASM alert log also

<span style="font-family: 'Courier New';">NOTE: Creating voting files in diskgroup RECOC1  
Thu Apr 27 18:16:36 2017  
NOTE: Voting File refresh pending for group 1/0x39368071 (RECOC1)  
Thu Apr 27 18:16:36 2017  
NOTE: Attempting voting file creation in diskgroup RECOC1  
NOTE: voting file allocation (replicated) on grp 1 disk RECOC1\_CD\_00\_DM01CELADM01  
NOTE: voting file allocation on grp 1 disk RECOC1\_CD\_00\_DM01CELADM01  
NOTE: voting file allocation (replicated) on grp 1 disk RECOC1\_CD\_00\_DM01CELADM02  
NOTE: voting file allocation on grp 1 disk RECOC1\_CD\_00\_DM01CELADM02  
NOTE: voting file allocation (replicated) on grp 1 disk RECOC1\_CD\_00\_DM01CELADM03  
NOTE: voting file allocation on grp 1 disk RECOC1\_CD\_00\_DM01CELADM03  
ERROR: Voting file allocation failed for group RECOC1  
Thu Apr 27 18:16:36 2017  
Errors in file /u01/app/oracle/diag/asm/+asm/+ASM1/trace/+ASM1\_ora\_228588.trc:  
**<span style="background-color: #ffff00;">ORA-15274: Not enough failgroups (5) to create voting files</span>**</span>

So we can see the issue here. We can look at the above trace file also for more detail.

Now to why did this happen ?

The RECOC1 is a HIGH redundancy disk group which means that if we want to place Voting files there, it should have at least 5 failure groups. In this configuration there are only 3 cells and that doesn’t meet the minimum failure groups condition (1 cell = 1 failgroup in Exadata).

Now to how did it happen ?

This one was an Exadata X3 half rack and we planned to deploy it (for testing purpose) as 2 quarter racks : 1st cluster with db1, db2 + cell1, cell2, cell3 and 2nd cluster with db3, db4 + cell4, cell5, cell6, cell7. All the disk groups to be in High redundancy.

Before a certain 12.x Exadata software version it was not even possible to have all disk groups in High redundancy in a quarter rack as to have Voting disk in a High redundancy disk group you need to have a minimum of 5 failure groups (as mentioned above). In a quarter rack you have only 3 fail groups. With a certain 12.x Exadata software version a new feature quorum disks was introduced which made is possible to have that configuration. Read [this link](http://asmsupportguy.blogspot.in/2016/03/quorum-disks-in-exadata.html) for more details. Basically we take a slice of disk from each DB node and add it to the disk group where you want to have the Voting file. 3 cells + 2 disks from DB nodes makes it 5 so all is good.

Now while starting with the deployment we noticed that db node1 was having some hardware issues. As we needed the machine for testing so we decided to build the first cluster with 1 db node only. So the final configuration of 1st cluster had 1 db node + 3 cells. We imported the XML back in OEDA, modified the cluster 1 configuration to 1 db node and generated the configuration files. That is where the problem started. The RECO disk group still was High redundancy but as we had only 1 db node at this stage so the configuration was not even a candidate for quorum disks. Hence the above error. Changing DBFS\_DG to Normal redundancy fixed the issue as when DBFS\_DG is Normal redundancy, OneCommand will place the Voting files there.

Ideally it shouldn’t happened as OEDA shouldn’t allow a configuration that is not doable. The case here is that as originally the configuration was having 2 db nodes + 3 cells so High redundancy for all disk groups was allowed in OEDA. While modifying the configuration when one db node was removed from the cluster, OEDA probably didn’t run the redundancy check on disk groups and it allowed the go past that screen. If you try to create a new configuration with 1 db node + 3 cells, it will not allow you to choose High redundancy for all disk groups. DBFS will remain in Normal redundancy. You can’t change that.