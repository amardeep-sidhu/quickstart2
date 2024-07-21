---
title: 'Garbled display while running FMW installer on Linux'
date: '2017-11-18T16:26:02+05:30'
status: publish
permalink: /2017/11/18/garbled-display-while-running-fmw-installer-on-linux
author: Sidhu
excerpt: ''
type: post
id: 589
category:
    - Troubleshooting
tag:
    - FMW
    - Installer
    - Linux
    - Troubleshooting
post_format: []
wpsd_autopost:
    - '1'
---
A colleague faced this while running FMW installer on a Linux machine. The display appeared like this

[![](http://amardeepsidhu.com/blog/wp-content/uploads/2017/11/fmw_installer.jpg)](http://amardeepsidhu.com/blog/wp-content/uploads/2017/11/fmw_installer.jpg)

[This thread](https://stackoverflow.com/questions/46270769/weblogic-12c-12-1-3-installation-on-unix-garbled-character-in-gui-over-xming) gave a clue that it could have something to do with fonts. So I checked what all fonts related stuff was installed.

```
<pre class="brush: bash; title: ; notranslate" title="">[root@someserver ~]# rpm -aq |grep -i font
stix-fonts-1.1.0-5.el7.noarch
xorg-x11-font-utils-7.5-20.el7.x86_64
xorg-x11-fonts-cyrillic-7.5-9.el7.noarch
xorg-x11-fonts-ISO8859-1-75dpi-7.5-9.el7.noarch
xorg-x11-fonts-ISO8859-9-100dpi-7.5-9.el7.noarch
xorg-x11-fonts-ISO8859-9-75dpi-7.5-9.el7.noarch
libXfont-1.5.2-1.el7.x86_64
xorg-x11-fonts-ISO8859-14-100dpi-7.5-9.el7.noarch
xorg-x11-fonts-ISO8859-1-100dpi-7.5-9.el7.noarch
xorg-x11-fonts-75dpi-7.5-9.el7.noarch
xorg-x11-fonts-ISO8859-2-100dpi-7.5-9.el7.noarch
libfontenc-1.1.3-3.el7.x86_64
xorg-x11-fonts-ethiopic-7.5-9.el7.noarch
xorg-x11-fonts-100dpi-7.5-9.el7.noarch
xorg-x11-fonts-misc-7.5-9.el7.noarch
fontpackages-filesystem-1.44-8.el7.noarch
fontconfig-2.10.95-11.el7.x86_64
xorg-x11-fonts-ISO8859-2-75dpi-7.5-9.el7.noarch
xorg-x11-fonts-ISO8859-14-75dpi-7.5-9.el7.noarch
xorg-x11-fonts-Type1-7.5-9.el7.noarch
xorg-x11-fonts-ISO8859-15-75dpi-7.5-9.el7.noarch
[root@someserver ~]#
```

stix-fonts looked suspicious to me. So I removed that with rpm -e stix-fonts.

That actually fixed the issue. After this the Installer window was displaying fine.