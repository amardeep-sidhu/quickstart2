---
title: 'ERROR: SPFile in diskgroup <> does not match the specified spfile'
date: '2016-09-20T22:12:50+05:30'
status: publish
permalink: /2016/09/20/error-spfile-in-diskgroup-does-not-match-the-specified-spfile
author: Sidhu
excerpt: ''
type: post
id: 525
category:
    - Uncategorized
tag: []
post_format: []
wpsd_autopost:
    - '1'
---
Just a stupid error. Posting it so that someone else googling for the same thing can get a clue.

An ASM instance running with default parameters (no pfile, no spfile). Updated spfile for the instance with asmcmd spset command and bounced crs. After reboot also, it still wasn’t using spfile. Got puzzled and checked GPnP settings again. All looked good. Then in alert log came across this

```
<pre class="brush: plain; title: ; notranslate" title="">ERROR: SPFile in diskgroup <> does not match the specified spfile +DATA/asm/asmparameterfile/registry.253.769187275
```

The problem was that while copying the spfile path the complete name didn’t get copied. The last character got missed. So the filename that it was looking for wasn’t there. Updating GPnP with correct filename and bouncing crs resolved the issue.