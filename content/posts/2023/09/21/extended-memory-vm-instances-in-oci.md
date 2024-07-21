---
title: 'Extended memory VM instances in OCI'
date: '2023-09-21T15:48:11+05:30'
status: publish
permalink: /2023/09/21/extended-memory-vm-instances-in-oci
author: Sidhu
excerpt: ''
type: post
id: 9701
category:
    - Compute
    - OCI
tag:
    - Compute
    - 'Extended Memory'
    - OCI
post_format: []
---
While provisioning compute instances in OCI, you may come across scenarios which need more memory and cores than what the standard shapes provide. Extended memory VMs are meant to solve that problem. Such VMs are able to access cores and memory across a single physical socket and it allows them to go beyond the limits of standard shapes. There are no additional charges for using this specific feature. Customer is charged on the basis of total number of cores and total amount of memory used.

On the provisioning screen, the sliders for selecting number of cores and amount of memory allow you to go beyond the limits for standard shapes. Once you cross the limit of a standard shape, the instances becomes an extended memory instance.

<figure class="wp-block-image size-large">[![](../../../../uploads/2023/09/image.png)](https://amardeepsidhu.com/blog/wp-content/uploads/2023/09/image.png)</figure>There are a few things to keep in mind while provisioning such an instance:

- Not all shapes support extended memory instances. Currently it is supported with VM.Standard3.Flex, VM.Standard.E3.Flex and VM.Standard.E4.Flex
- Extended memory instances don’t support burstable feature.
- Capacity reservations aren’t available with Extended memory instances.
- Preemptible instances don’t work with this feature.
- To take advantage of the extended memory, it is important to make the application NUMA aware when it is deployed on a Extended memory instance.

More details about extended memory instances are available in the [official documentation page](https://docs.oracle.com/en-us/iaas/Content/Compute/References/extended-memory-vm-instances.htm).