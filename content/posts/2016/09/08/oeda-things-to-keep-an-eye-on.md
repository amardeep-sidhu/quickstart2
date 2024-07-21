---
title: 'OEDA&ndash;Things to keep an eye on'
date: '2016-09-08T15:44:15+05:30'
status: publish
permalink: /2016/09/08/oeda-things-to-keep-an-eye-on
author: Sidhu
excerpt: ''
type: post
id: 485
category:
    - Exadata
tag:
    - Exadata
    - OEDA
post_format: []
---
So if you are filling an **OEDA** for Exadata deployment there are few things you should take care of. Most of the screens are self explanatory but there are some bits where one should focus little more. I am running the Aug version of it and the screenshots below are from that version.

1. On the **Define customer networks** screen, the client network is the actual network where your data is going to flow. So typically it is going to be bonded (for high availability) and depending upon the network in your data center you have to select one out of 1/10 G copper and 10 G optical. [![image](http://amardeepsidhu.com/blog/wp-content/uploads/2016/09/image_thumb.png "image")](http://amardeepsidhu.com/blog/wp-content/uploads/2016/09/image.png)
2. If you are going to use trunk VLANs for your client network, remember to enabled it by clicking the **Advanced** button and then entering the relevant VLAN id.
[![image](http://amardeepsidhu.com/blog/wp-content/uploads/2016/09/image_thumb-5.png "image")](http://amardeepsidhu.com/blog/wp-content/uploads/2016/09/image-5.png)

Also if it is going to be an OVM configuration, you may want to have different VMs in different VLAN segments. It will allow you to change VLAN ids for individual VMs on the respective cluster screens like below

[![image](http://amardeepsidhu.com/blog/wp-content/uploads/2016/09/image_thumb-6.png "image")](http://amardeepsidhu.com/blog/wp-content/uploads/2016/09/image-6.png)

6. If all the cores aren’t licensed remember to enable Capacity on Demand (COD) on the **Identify Compute node OS** screen.
[![image](http://amardeepsidhu.com/blog/wp-content/uploads/2016/09/image_thumb-7.png "image")](http://amardeepsidhu.com/blog/wp-content/uploads/2016/09/image-7.png)

8. On the **Define clusters** screen make sure that you enter a unique (across your environment) cluster name. [![image](http://amardeepsidhu.com/blog/wp-content/uploads/2016/09/image_thumb-8.png "image")](http://amardeepsidhu.com/blog/wp-content/uploads/2016/09/image-8.png)
9. The **cluster details** screen captures some of the most important details like
1. Whether you want to have flash cache in WriteBack mode instead of WriteThrough
2. Whether you want to have a role separated install or want to install both GI and Oracle binaries with oracle user itself.
3. GI &amp; Database versions and home for binaries. Always good to leave it at the Oracle recommended values as that makes the future maintenance easy and less painful.
4. Disk Group names, redundancy and the space allocation.
5. Default database name and type (OLTP or DW). [![image](http://amardeepsidhu.com/blog/wp-content/uploads/2016/09/image_thumb-9.png "image")](http://amardeepsidhu.com/blog/wp-content/uploads/2016/09/image-9.png)


Of course it is important to carefully fill the information in all the screens but the above ones are some of them which should be filled very carefully after capturing the required information from other teams, if needed.