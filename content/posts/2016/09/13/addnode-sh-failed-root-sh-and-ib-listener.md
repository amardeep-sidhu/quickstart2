---
title: 'addNode.sh, failed root.sh and IB listener'
date: '2016-09-13T19:44:56+05:30'
status: publish
permalink: /2016/09/13/addnode-sh-failed-root-sh-and-ib-listener
author: Sidhu
excerpt: ''
type: post
id: 501
category:
    - Exadata
    - RAC
tag:
    - addNode
    - Exadata
    - RAC
post_format: []
---
So this customer has an Exadata quarter rack and they have an IB listener configured on both DB nodes (for DB connections from a multi-racked Exalogic system). We were adding a new DB node to this rack. So just followed the standard procedure of creating users, directories etc on the new node, setting up ssh equivalence and running addNode.sh. All went fine but root.sh failed. Little looking into the logs revealed that it failed while running **srvctl start listener –n &lt;node\_name&gt;**

If we manually run this command, it will immediately reveal what the problem is. It is not able to start IB listener on the new node as the IB VIP doesn’t yet exist. It could happen for any of the additional networks added.

There is a MOS note that describes this exact situation but the solution that it gives is to remove the additional listener, complete addNode.sh &amp; root.sh and add the additional listener back. That wasn’t possible in this case. After little bit of googling I stumbled upon [this post](https://ardentperf.com/2013/11/04/listener-error-from-addnode-sh-with-second-network/) by [Jeremy Schneider](https://ardentperf.com/). His colleague solved this problem with a very simple and clever workaround. Before root.sh prepares to run **srvctl start listener** command, run the add VIP command from another Window ![Winking smile](http://amardeepsidhu.com/blog/wp-content/uploads/2016/09/wlEmoticon-winkingsmile.png). Additional network would have already got added when root.sh runs on the new node.

To be able to perform this trick, you have to have the hosts file updated with the new VIP name and IP and be ready with the command to add the VIP. While root.sh is running, it will show a message like “there is already an active cluster, restarting to join”, immediately start trying to run **srvctl add vip** command in another window. The moment CRS, comes up the command will succeed. Immediately after that root.sh is going to run **srvctl start listener** command, and this time it shouldn’t fail as the VIP is already added.

Another small mistake we made was not updating the cellip.ora on the new node before running root.sh. That caused the root.sh to fail as it couldn’t talk to ASM running on existing cell nodes. Updating cellip.ora with the existing storage node IPs fixed the problem.