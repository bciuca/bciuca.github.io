---
layout: post
title: "EdgeRouter Lite Ad Blocking"
description: ""
---
{% include JB/setup %}

The [Ubiquiti Networks](http://www.ubnt.com/) EdgeRouter Lite is a powerful, Linux based wired router. For under $100 you get a dual-core MIPS64 CPU, hardware accelerated packet processing with encryption/decryption, 512 MB DDR2 RAM, and 2 GB flash storage. It is marketed as a sub-$100 million packet per second router. All that sounds pretty awesome, but the biggest feature I was interested in was the configurability and software features. 

I have played around in DD-WRT on an aging Linsys WRT54G but it always left more to be desired. I was even considering building a Linux based rig as a router for more flexibility and performance enhancements of my home network. After some research, the EdgeRouter Lite seemed like the perfect package -- small, Linux based OS, cheap, and low power consumption.

One of the first things I did was set up [dnsmasq](http://en.wikipedia.org/wiki/Dnsmasq) to block ads. The benefit of doing this at the router level is that you will block ads on all devices connected to your network. The only caveat is that the devices must be configured to point to the router as the default DNS server, which is usually the default of many network connection setups.

### Ad Blocking

The following steps and script closely mirror the steps outlined in the thread from the Ubiquiti Networks [forum](https://community.ubnt.com/t5/EdgeMAX/Adblocking-at-home-using-EdgeMAX/m-p/623239/highlight/true#M17854). These are my notes from the setup of my EdgeRouter Lite (firmware v1.4.0).

You will need to ssh into your router with an admin account or using sudo for most of the commands.

#### 1. The Shell Script

Copy the following lines to `/config/user-data/update-adblock-dnsmasq.sh`

    #!/bin/bash

    ad_list_url="http://pgl.yoyo.org/adservers/serverlist.php?hostformat=dnsmasq&showintro=0&mimetype=plaintext"
    #The IP address below should point to the IP of your router or to 0.0.0.0
    pixelserv_ip="0.0.0.0"
    ad_file="/etc/dnsmasq.d/dnsmasq.adlist.conf"
    temp_ad_file="/etc/dnsmasq.d/dnsmasq.adlist.conf.tmp"

    curl -s $ad_list_url | sed "s/127\.0\.0\.1/$pixelserv_ip/" > $temp_ad_file

    if [ -f "$temp_ad_file" ]
    then
        mv $temp_ad_file $ad_file
    else
        echo "Error building the ad list, please try again."
        exit
    fi

    /etc/init.d/dnsmasq force-reload

If there are certain sites that you need to allow, you can add the following line for each site you need to remove from the block list. These lines should be placed directly below the "then" line. For example, to allow ads on stackoverflow, I would add the following line:

    sed -i -e '/ads\.stackoverflow\.com/d' $temp_ad_file

#### 2. Set File Permissions

Mark the shell script as executable.

    chmod a+x /config/user-data/update-adblock-dnsmasq.sh

#### 3. Run It

    /config/user-data/update-adblock-dnsmasq.sh

#### 4. Test It

Check to see that everything is working. I was not able to install dig, so I used the host command instead.

    host measuremap.com localhost

You should see something like this:

    Using domain server:
    Name: localhost
    Address: 127.0.0.1#53
    Aliases: 

    measuremap.com has address 0.0.0.0

You can also try to ping a blocked address from a client machine

    ping measuremap.com

Ping should fail.

    PING measuremap.com (0.0.0.0): 56 data bytes
    ping: sendto: No route to host
    ping: sendto: No route to host
    Request timeout for icmp_seq 0
    ping: sendto: No route to host
    Request timeout for icmp_seq 1
    ping: sendto: No route to host
    Request timeout for icmp_seq 2

**Note: Using a custom DNS server configuration on the client side will bypass the router's DNS forwarder. When checking that adblock is working at the router level you will also need to check the client machine's network configuration for the DNS server entries.**

#### 5. Automatic Ad Block Updates

Using cron, you can schedule the ad server list to update periodically. Open up the crontab file to add a rule.

    crontab -e

The rule will run the update at a specific interval. Cron parameters are `minute hour dayOfMonth month dayOfWeek command` 

You can read more about cron [here](http://www.thegeekstuff.com/2009/06/15-practical-crontab-examples/) and [here](http://en.wikipedia.org/wiki/Cron).

For example, I want to run the update at 3:00 AM every Sunday:

    0 3 * * 0  /config/user-data/update-adblock-mdns.sh