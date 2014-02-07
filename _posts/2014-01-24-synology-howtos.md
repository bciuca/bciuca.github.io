---
layout: post
title: "Synology NAS"
description: ""
---
{% include JB/setup %}

Some notes regarding [Synology](http://www.synology.com/en-us/) NAS device setup and other tasks.

## Services and Daemons

Services reside in the system user directory. I'm not very familiar with BSD based OSes, but in Linux, I would usually go to `/etc/init.d/` dir to find/restart any service or daemon needed.

Restarting services and daemons must be done in the `/usr/syno/etc.defaults/rc.d/` directory. You will need to list the directory as the service file names have an ID appended. For example, restarting SSH:

    /usr/syno/etc.defaults/rc.d/S95sshd.sh restart