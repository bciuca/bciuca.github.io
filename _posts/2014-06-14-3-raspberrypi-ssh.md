---
layout: post
title: "Raspberry Pi SSH Configuration"
description: "Raspberry Pi ssh configuration."
---
{% include JB/setup %}

## Enable RSA Authentication

For convenience and security, turn on RSA authentication in `sshd`.

    sudo vim /etc/ssh/sshd_config 
