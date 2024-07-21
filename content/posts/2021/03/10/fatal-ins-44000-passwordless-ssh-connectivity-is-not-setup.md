---
title: '[FATAL] [INS-44000] Passwordless SSH connectivity is not setup'
date: '2021-03-10T18:17:34+05:30'
status: publish
permalink: /2021/03/10/fatal-ins-44000-passwordless-ssh-connectivity-is-not-setup
author: Sidhu
excerpt: ''
type: post
id: 797
category:
    - RAC
tag:
    - gridsetup
    - RAC
    - scp
    - SuperCluster
post_format: []
---
Faced this while running installer for setting up a 2 node RAC setup (version 19.8) on an Oracle SuperCluster. The error reported in the log is:

<div class="wp-block-syntaxhighlighter-code ">```
<pre class="brush: plain; title: ; notranslate" title="">
[FATAL] [INS-44000] Passwordless SSH connectivity is not setup from the local node node1 to the following nodes:
[node2]
[INS-06006] Passwordless SSH connectivity not set up between the following node(s): [node2]

```

</div>From the error it appears that the ssh is not setup between two nodes but actually that is not the case. Here the error message is bit misleading. It turned out to be an issue with scp with openssh version 8.x. Running the setup with -debug option gives the clue:

<div class="wp-block-syntaxhighlighter-code ">```
<pre class="brush: plain; title: ; notranslate" title="">
<protocol error: filename does not match request>
```

</div>The reason is a new check introduced in openssh version 8.x. It is explained [here](https://askubuntu.com/questions/1126612/how-to-use-scp-to-transfer-files-between-ubuntu-and-windows10), [here ](https://stackoverflow.com/questions/54598689/scp-fails-with-protocol-error-filename-does-not-match-request/54599326#54599326)and [here](https://www.openssh.com/txt/release-8.0). MOS note 2555697.1 also talks about it.

Workaround is to pass the -T option to scp to ignore the new checks. You can rename the scp binary to something like scp.original and create a new shell script there like this:

<div class="wp-block-syntaxhighlighter-code ">```
<pre class="brush: bash; title: ; notranslate" title="">
cd /usr/bin
mv scp scp.original
vi scp
/usr/bin/scp.original -T $*
chmod 555 scp
```

</div> This time, the install should succeed. You can revert the changes back once the install is done.