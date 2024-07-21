---
title: 'Adding a new cell to Exadata with asm scoped security enabled'
date: '2022-05-09T13:26:00+05:30'
status: publish
permalink: /2022/05/09/adding-a-new-cell-to-exadata-with-asm-scoped-security-enabled
author: Sidhu
excerpt: ''
type: post
id: 1258
category:
    - Exadata
tag:
    - 'asm scoped security'
    - Exadata
    - 'Storage Cell'
post_format: []
---
A customer is using an Exadata X8M-2 machine with multiple VMs (hence multiple clusters). I was working on adding a new storage cell to the configuration. After creating griddisks on the new cell and updating cellip.ora on all the VMs, I noticed that none of the clusters was able to see the new griddisks. I checked the usual suspects like if asm\_diskstring was set properly, private network subnet mask on new cell was same as the old ones. All looked good. I started searching about the issue and stumbled upon some references mentioning [ASM scoped security](https://mudasirhakakblog.wordpress.com/2019/03/01/asm-scoped-security/). I checked on one of the existing cells and that actually was the issue. The existing nodes had it enabled while the new one hadn’t. Running this command on an existing cell

```
<pre class="wp-block-code">```
cellcli -e list key detail

name:
key:		c25a62472a160e28bf15a29c162f1d74
type:		CELL

name:		cluster1
key:		fa292e11b31b210c4b7a24c5f1bb4d32
type:		ASMCLUSTER

name:		cluster2
key:		b67d5587fe728118af47c57ab8da650a	
type:		ASMCLUSTER 
```
```

We need to enable ASM scoped security on the new cell as well. There are three things that need to be done. We need to copy /etc/oracle/cell/network-config/cellkey.ora from an existing cell to the new cell, assign the key to the cell and then assign keys to the different ASM clusters. We can use these commands to do it

```
<pre class="wp-block-code">```
cellcli -e  ASSIGN KEY FOR CELL 'c25a62472a160e28bf15a29c162f1d74'
cellcli -e  ASSIGN KEY FOR ASMCLUSTER 'cluster1'='fa292e11b31b210c4b7a24c5f1bb4d32';
cellcli -e  ASSIGN KEY FOR ASMCLUSTER 'cluster2'='b67d5587fe728118af47c57ab8da650a';
```
```

Once this is done, we need to tag the griddisks for appropriate ASM clusters. If the griddisks aren’t created yet, we can use this command to do it

```
<pre class="wp-block-code">```
cellcli -e CREATE GRIDDISK ALL HARDDISK PREFIX=sales, size=75G, availableTo='cluster1'
```
```

If the griddisks are already created, we can use the alter command to make this change

```
<pre class="wp-block-code">```
cellcli -e alter griddisk griddisk0,gridisk1,.....griddisk11 availableTo='cluster1';
```
```

Once this is done, we should be able to see new griddisks as CANDIDATE in v$asm\_disk