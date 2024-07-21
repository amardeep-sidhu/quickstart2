---
title: 'Can&#8217;t su to oracle user'
date: '2023-01-20T16:55:50+05:30'
status: publish
permalink: /2023/01/20/cant-su-to-oracle-user
author: Sidhu
excerpt: ''
type: post
id: 7530
category:
    - Database
    - OCI
    - Unix/Linux
tag:
    - Database
    - Linux
    - OCI
    - Oracle
post_format: []
---
Last week, got this issue reported by a DBA that he wasn’t able to su to oracle user from root on a Oracle Base Database VM in OCI. The login of opc user worked fine and he could do sudo su to root but he couldn’t su to oracle. When he did it just came back to root shell.

<div class="wp-block-syntaxhighlighter-code ">```
<pre class="brush: bash; title: ; notranslate" title="">
[root@xxx ~]# su - oracle
Last login: Fri Jan 12 10:20:38 UTC 2023
[root@xxx ~]#
```

</div>There was nothing relevant in /var/log/messages or /var/log/secure. I tried it for some other user and it worked fine. Then I suspected something with the profile of oracle user and voila ! The .bashrc looked like this

<div class="wp-block-syntaxhighlighter-code ">```
<pre class="brush: bash; title: ; notranslate" title="">
[root@xxx oracle]# pwd
/home/oracle
[root@xxx oracle]# more .bashrc
exit
[root@xxx oracle]#
```

</div>So the moment it logged in with user oracle, there was an **exit** command in .bashrc and it used to exit. Problem solved. I don’t know who did it but it looks like a mischief done by someone.