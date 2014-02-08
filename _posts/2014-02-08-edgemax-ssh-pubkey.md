---
layout: post
title: "Disabling Password Login on EdgeMAX Routers"
description: ""
tags: [edgerouter, edgemax, ssh]
---
{% include JB/setup %}

Enabling public key authentication on the EdgeMAX router is not as straight forward as manually configuring sshd_config and adding an authorized key. Running through the CLI configuration manager is the appropriate procedure in this case. Using the configure command, the ssh settings and the public key will be added to the routers configuration file, which is convenient when backing up settings and restoring configurations.

The setup is based on the EdgeRouter Lite (firmware v1.4.0) and the [ubnt wiki](http://wiki.ubnt.com/Access_Using_SSH). 

### Setup

In steps 2 and 4, substitute the appropriate username and router IP address.

1. Generate keys on client machine
2. SCP file to router home: `scp ~/.ssh/id_rsa.pub USERNAME@ROUTERIP:~/id_rsa.pub`
3. `configure`
4. `loadkey USERNAME ~/id_rsa.pub`
5. `set service ssh disable-password-authentication`
6. `commit`
7. `save`
8. `exit`

### Undo

To enable password authentication again, using an already open ssh session or through the web CLI, delete the configuration line in step 5 above.
   
1. `delete service ssh disable-password-authentication`
2. `commit`
3. `save`
4. `exit`
