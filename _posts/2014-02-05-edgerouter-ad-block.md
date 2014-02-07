---
layout: post
title: "EdgeRouter Lite Ad Blocking"
description: ""
---
{% include JB/setup %}

The following steps and script closely mirror the steps outlined in the thread from the Ubiquity Networks [forum](https://community.ubnt.com/t5/EdgeMAX/Adblocking-at-home-using-EdgeMAX/m-p/623239/highlight/true#M17854). These are my notes from my setup of my EdgeRouter Lite (firmware v1.4.0).

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