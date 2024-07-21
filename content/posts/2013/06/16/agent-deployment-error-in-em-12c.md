---
title: 'agent deployment error in EM 12c'
date: '2013-06-16T22:34:56+05:30'
status: publish
permalink: /2013/06/16/agent-deployment-error-in-em-12c
author: Sidhu
excerpt: ''
type: post
id: 423
category:
    - EM
tag:
    - 12c
    - EM
post_format: []
---
Yesterday I was configuring EM 12c for a Sun Super Cluster system. There were a total of 4 LDOMs where I needed to deploy the agent (Setup –&gt; Add targets –&gt; Add targets manually). Out of these 4 everything went fine for 2 LDOMs but for the other two it failed with an error message. It didn’t give much details on the EM screen but rather gave a message to try to secure/start the agent manually. When I tried to do that manually the secure agent part worked fine but the start agent command failed with the following error message:

<font face="Courier New">oracle@app1:~$emctl start agent   
Oracle Enterprise Manager Cloud Control 12c Release 2   
Copyright (c) 1996, 2012 Oracle Corporation. All rights reserved.   
Starting agent ………………………………………………………. failed.   
HTTP Listener failed at Startup   
Possible port conflict on port(3872): Retrying the operation…   
Failed to start the agent after 1 attempts. Please check that the port(3872) is available.</font>

I thought that there was something wrong with the port thing so I cleaned the agent installation, made sure that the port wasn’t being used and did the agent deployment again. This time it again failed with the same message but it reported a different port number ie 1830 agent port no:

<font face="Courier New">oracle@app1:~$emctl start agent   
Oracle Enterprise Manager Cloud Control 12c Release 2   
Copyright (c) 1996, 2012 Oracle Corporation. All rights reserved.   
Starting agent ……………………………………………. failed.   
HTTP Listener failed at Startup   
Possible port conflict on port(1830): Retrying the operation…   
Failed to start the agent after 1 attempts. Please check that the port(1830) is available.</font>

Again checked few things but found nothing wrong. All the LDOMs had similar configuration so what worked for the other two should have worked for these two also.

Before starting with the installation I had noted the LDOM hostnames and IPs in a notepad file and had swapped the IPs of two LDOMs (actually these two only ![Smile with tongue out](http://amardeepsidhu.com/blog/wp-content/uploads/2013/06/wlEmoticon-smilewithtongueout.png) ). But later on I found that and corrected. While looking at the notepad file it occurred to me that the same stuff could be wrong in /etc/hosts of the server where EM is deployed. Oh boy that is what it was. While making the entries in /etc/hosts of EM server, I copied it from the notepad and the wrong entries got copied. The IPs for these two LDOMs got swapped with each other and that was causing the whole problem.

deinstalled the agent, correct the /etc/hosts and tried to deploy again…all worked well !