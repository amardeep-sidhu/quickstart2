---
title: 'Exadata Virtualized DB node restore'
date: '2020-05-11T21:31:43+05:30'
status: publish
permalink: /2020/05/11/exadata-virtualized-db-node-restore
author: Sidhu
excerpt: ''
type: post
id: 695
category:
    - Exadata
tag:
    - Exadata
    - OneCommand
    - OVS
    - Restore
post_format: []
---
There are two common scenarios when we may need this:

- An existing DB node has crashed and is unrecoverable (due to some failure and non-availability of any backups. Though some of the things may need to be done even if the backups were available).
- We have an existing Exadata rack that is virtualized. Now there is a new DB node and the existing clusters need to be extended to include the VMs on this new node.

I recently faced the first scenario where a virtualized DB node crashed and wasn’t recoverable. A bare metal DB node restore is a relatively simple procedure where we just have to reimage the node, create the needed directories, users etc and add it to the RAC cluster. In case of virtualization, the creation of VMs is an additional step that needs to be done. That makes it slightly more complex.

So the scenario is that we have an Exadata quarter rack where DB node1 has issues and needs to be reimaged and reconfigured. There are multiple VMs (so RAC clusters) created. As one of the DB node has gone down, each RAC cluster is running with one less instance. This failed node will need to be cleaned up from the RAC configuration before adding it back. Here are the steps that we need to follow to restore it back:

1. Reimage the node using an ISO and make it ready for creation of User Domains (aka VMs)
2. Create the required VMs
3. Create the required users, setup ssh with other nodes
4. Clear the failed node configuration from existing RAC clusters
5. Add the newly created VMs back to the respective RAC clusters

Now let’s discuss these steps in detail.

1. **Reimage :** The simplest way to reimage an Exadata node is to connect the ISO (We can download the ISO for the version we need from MOS note 888828.1) using ILOM, set the next boot device to CD-ROM, reboot/reset the node and let it boot from CD-ROM. Most of the installation part is automated and doesn’t ask any questions. Once it is done installing, ipconf starts in interactive mode and asks for all the information like Name servers, NTP servers, IP addresses and hostnames for various network interfaces etc. Once done, it will boot into the Linux partition. Since we need to virtualize the node, we need to switch it to OVS by running a script called `/opt/oracle.SupportTools/switch_to_ovm.sh.` It will reboot the node to OVS partition. Next step is to run reclaim `/opt/opt/oracle.SupportTools/reclaim.sh -free -reclaim` to reclaim the space used for bare metal partition. At this moment we are done with the reimaging part. To use ILOM in a browser and be able to access the console, we need a Java enabled Windows/Linux system. And if there is a firewall between that system and the server, [this link](https://docs.oracle.com/cd/E19860-01/E21549/z40001861019988.html) lists the ports that need to be allowed in the firewall.
2. **VMs creation :** Next step is the creation of VMs. We will use OneCommand to achieve this. In this case, we had the original XML file used for deployment. Now we need to edit that configuration and remove the existing node’s details from it. We can import the XML into OEDA, make the required changes and save the configuration files. This needs to be done carefully as a simple mistake like a duplicate IP may cause issues with the ASM/DBs running on the other node. Once this is done, we can download the OneCommand patch (MOS note 888828.1) and run the create VMs step of OneCommand. As we have only one node in the XML file, so it is not going to touch the existing configuration.
3. **Create users :** Now we need to create the users on the newly created VMs. OneCommand’s create users step can be used here. It will create users on all the VMs. There are some things that we need to do manually here. First thing is to remove binaries from Grid &amp; DB home. As we are going to use addnode.sh to add new nodes to existing RAC clusters, so binaries are going to be copied from an existing node. Then we need to change ownership of Grid &amp; DB home directory tree to oracle:oinstall. Also for each VM, we need to setup passwordless ssh with the respective other VM (&amp; vice versa) that is going to be part of the cluster.
4. **Clear failed node config :** Next we need to clear the failed node’s configuration from each of the RAC clusters. That is pretty much the standard stuff we do in RAC.
5. **Add the new nodes :** This again is just the standard addnode stuff we do in RAC.

I have used the terms VM and Node interchangeably here but the context should make it clear if I am referring to the physical node or a VM. There is another method to do this using OEDACLI and it is documented in Exadata documentation. That automates a lot of these things. Check [this link](https://docs.oracle.com/en/engineered-systems/exadata-database-machine/dbmmn/managing-oracle-vm-domains.html#GUID-0E099CB4-8D4F-4193-8FE3-B91B21CED306) for the details.