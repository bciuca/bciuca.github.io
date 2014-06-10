---
layout: post
title: "Raspberry Pi and Node.js"
description: "Quick setup of Raspberry Pi and Node.js"
---
{% include JB/setup %}

##Prerequisites

* Raspberry Pi (Model B)
* 8GB SD card (class 4)
* Micro USB cable
* USB wall charger with minimum specs of 700mA at 5V.
* The basics: monitor, keyboard, mouse, and wired network connection.

##Installing Raspberry Pi (NOOBS)
Detailed setup instructions can be found [here](http://www.raspberrypi.org/help/noobs-setup/). SSH is installed by default. After initial setup, Raspberry Pi can be run in a headless configuration.

* Download [offline](http://downloads.raspberrypi.org/NOOBS_latest) or [online](http://downloads.raspberrypi.org/NOOBS_lite_latest) setup.
* Extract and copy files to FAT formatted SD card.
* Plug in the SD card into the Raspberry Pi.
* Plug in keyboard, mouse, monitor, and network.
* Lastly, plug in the power.

###OS Installation

The NOOBS installation will have a few choices for OSes to install. This tutorial is based on the Rasbian flavor.

* Select the Rasbian OS and select/click Install.
* After setup is complete you will configuration (raspi-config).
* At minimum change your password and set the date and time.
* To run through the configuration again, run `sudo raspi-config`

After configuration is completed, check for updates.

    sudo apt-get upgrade
    sudo apt-get update


###Node.js Installation

At the time of writing, the stable version of Node.js for Raspberry Pi is 0.10.24.

Download and extract Node.js binaries.


    wget http://nodejs.org/dist/v0.10.24/node-v0.10.24-linux-arm-pi.tar.gz
    tar -xvzf node-v0.10.24-linux-arm-pi.tar.gz

* Test that node works as expect: `node-v0.10.2-linux-arm-pi/bin/node --version` should output `v0.10.24`
* Move installation dir: `mv node-v0.10.24-linux-arm-pi /usr/local/bin/node`

Edit `.bash_profile` to add the node installation dir to the environment variables and the node `bin` folder to the path. Using vi, open the file `vi ~/.bash_profile`


    NODE_JS_HOME=/usr/local/bin/node
    PATH=$PATH:$NODE_JS_HOME/bin


* Load your saved profile `. ~/.bash_profile`
* Test to make sure settings were loaded: `node --version` should print the version as expected.

If native binaries need to be built, install node's native build tool, `node-gyp`

    npm install -g node-gyp
