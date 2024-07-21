---
title: 'Failed to execute the command &#8220;&#8221;/u01/app/xag/bin/clsecho&#8221;'
date: '2019-01-08T22:52:14+05:30'
status: publish
permalink: /2019/01/08/failed-to-execute-the-command-u01-app-xag-bin-clsecho
author: Sidhu
excerpt: ''
type: post
id: 624
category:
    - GoldenGate
tag:
    - GoldenGate
post_format: []
wpsd_autopost:
    - '1'
---
I was configuring GoldenGate in HA mode by following [this document](https://www.oracle.com/technetwork/database/features/availability/maa-wp-gg-oracledbm-128760.pdf). Everything worked ok but in the end while running agctl config goldengate to view the configuration of GoldenGate resource, it was failing with the following error:

```
<pre class="wp-block-preformatted">[oracle@exadatadb02 ~]$ agctl config goldengate GG_TARGET<br></br> Failed to execute the command ""/u01/app/xag/bin/clsecho" -p xag -f xag -m 5080 "GG_TARGET"" (rc=134), with the message:<br></br> Oracle Clusterware infrastructure fatal error in clsecho.bin (OS PID 126367_140570897783808): Internal error (ID (:CLSB00107:)) - Error -1 <strong>(ORA-08275) determining Oracle base</strong><br></br> /u01/app/xag/bin/clsecho: line 45: 126367 Aborted                 (core dumped) ${CRS_HOME}/bin/clsecho.bin "$@"<br></br> Failed to execute the command ""/u01/app/xag/bin/clsecho" -p xag -f xag -m 5081 "/u01/app/oragg/product"" (rc=134), with the message:
```

If you look at the error in bold it sounds kinda obvious that it is not able to figure our where the ORACLE\_BASE is. But somehow it didn’t strike me at that moment. So started looking around. If we look at the command it is running, it runs clsecho. This is simply a shell script which in turn calls $CRS\_HOME/bin/clsecho.bin . In the script, it sets various environment variables and that is where the problem was. There are lines like:

```
<pre class="wp-block-preformatted">ORACLE_BASE=<br></br>export ORACLE_BASE
```

Nowhere in the script, it is setting the value of ORACLE\_BASE. That was causing an issue. I changed the first line to set the ORACLE\_BASE location and it worked fine after that. There was another issue i faced with ggsci after doing xag configuration. Will do another blog post on that.