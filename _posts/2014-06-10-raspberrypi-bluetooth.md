---
layout: post
title: "Raspberry Pi Bluetooth setup"
description: "Raspberry Pi Bluetooth setup"
---
{% include JB/setup %}

## Setting up Bluetooth
Plug in USB dongle and using the command `lsusb` to get a list of connected devices.

    pi@raspberrypi:~$ lsusb
    Bus 001 Device 002: ID 0424:9514 Standard Microsystems Corp.
    Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 001 Device 003: ID 0424:ec00 Standard Microsystems Corp.
    Bus 001 Device 004: ID 0a12:0001 Cambridge Silicon Radio, Ltd Bluetooth Dongle (HCI mode)

Install USB software:

     sudo apt-get install bluetooth bluez-utils blueman

Check for bluetooth status

    /etc/init.d/bluetooth status

Scan for devices

    pi@raspberrypi:~$ hcitool scan
    Scanning ...
	    D3:A3:FA:FA:FA:00	MyPC

Ping device to check resonse times

    pi@raspberrypi:~$ sudo l2ping D3:A3:FA:FA:FA:00
    Ping: D3:A3:FA:FA:FA:00 from 00:1B:DC:02:44:21 (data size 44) ...
    44 bytes from D3:A3:FA:FA:FA:00 id 0 time 18.21ms
    44 bytes from D3:A3:FA:FA:FA:00 id 1 time 73.55ms
    44 bytes from D3:A3:FA:FA:FA:00 id 2 time 20.72ms
    44 bytes from D3:A3:FA:FA:FA:00 id 3 time 16.76ms
    44 bytes from D3:A3:FA:FA:FA:00 id 4 time 32.01ms
    44 bytes from D3:A3:FA:FA:FA:00 id 5 time 17.67ms
    44 bytes from D3:A3:FA:FA:FA:00 id 6 time 34.67ms
    44 bytes from D3:A3:FA:FA:FA:00 id 7 time 18.65ms
    44 bytes from D3:A3:FA:FA:FA:00 id 8 time 12.76ms
    44 bytes from D3:A3:FA:FA:FA:00 id 9 time 12.60ms
    44 bytes from D3:A3:FA:FA:FA:00 id 10 time 13.67ms

Pair

    sudo bluez-simple-agent hci0 C8:BC:C8:EA:BE:86

Add device to trusted list.

    sudo bluez-test-device trusted C8:BC:C8:EA:BE:86 yes

Set discoverable

    sudo hciconfig hci0 piscan
