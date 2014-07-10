---
layout: post
title: "Raspberry Pi Wireless Network Setup"
description: "Raspberry Pi network setup"
---
{% include JB/setup %}

## Installation

Before beginning, make sure your device is updated.

    sudo apt-get update
    sudo apt-get upgrade

### Verify USB Device

This guide is based on the [Edimax](http://www.amazon.com/Edimax-EW-7811Un-Wireless-Adapter-Wizard/dp/B003MTTJOY/ref=sr_1_1?ie=UTF8&qid=1402764906&sr=8-1&keywords=edimax) USB wireless adapter. With the Raspberry Pi turned on and booted, plug in the wireless module.
Check for USB devices using `lsusb`.

    pi@raspberrypi:~$ lsusb
    Bus 001 Device 002: ID 0424:9514 Standard Microsystems Corp.
    Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 001 Device 003: ID 0424:ec00 Standard Microsystems Corp.
    Bus 001 Device 004: ID 7392:7811 Edimax Technology Co., Ltd EW-7811Un 802.11n Wireless Adapter [Realtek RTL8188CUS]
    pi@Rasputin:~$

Verify that the kernel module was loaded, `lsmod`. Module `8192cu` should be in the list. If the USB device is not seen in theses, try unplugging the wireless adapter and plugging it back in to reinitialize it. Otherwise, try rebooting.

    pi@raspberrypi:~$ lsmod
    Module                  Size  Used by
    rfcomm                 33132  0
    bluetooth             225849  10 bnep,rfcomm
    rfkill                 19567  2 bluetooth
    snd_bcm2835            18169  0
    snd_soc_bcm2708_i2s     5486  0
    regmap_mmio             2818  1 snd_soc_bcm2708_i2s
    snd_soc_core          128166  1 snd_soc_bcm2708_i2s
    regmap_spi              1913  1 snd_soc_core
    snd_pcm_dmaengine       5481  1 snd_soc_core
    snd_pcm                81518  3 snd_bcm2835,snd_soc_core,snd_pcm_dmaengine
    snd_page_alloc          5168  1 snd_pcm
    regmap_i2c              1657  1 snd_soc_core
    snd_compress            8136  1 snd_soc_core
    8192cu                551136  0

Using the `iwconfig` command, check that the wireless network configuration exists. This command will be used again to verify connection to the network.

    pi@raspberrypi:~$ iwconfig
    wlan0     unassociated  Nickname:"<WIFI@REALTEK>"
              Mode:Managed  Frequency:2.462 GHz  Access Point: 70:56:81:11:22:33
              Bit Rate:72.2 Mb/s   Sensitivity:0/0  
              Retry:off   RTS thr:off   Fragment thr:off
              Power Management:off
              Link Quality=100/100  Signal level=67/100  Noise level=0/100
              Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
              Tx excessive retries:0  Invalid misc:0   Missed beacon:0

    lo        no wireless extensions.

    eth0      no wireless extensions.

<br/>

### Configuration

#### Network Configuration
Edit the network configuration file `/etc/network/interfaces` (use `sudo nano` or another text editor).

    # local network loopback
    auto lo
    iface lo inet loopback

    # LAN settings
    auto eth0
    allow-hotplug eth0
    iface eth0 inet dhcp

    # WAN settings
    auto wlan0
    allow-hotplug wlan0
    iface wlan0 inet manual
    wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf
    iface default inet dhcp

The `auto wlan0` and `allow-hotplug wlan0` are needed to start the wireless device on boot. It also connects to a network automatically when the USB adapter is first plugged in.

The configuration above used DHCP for both LAN and WAN configuration. To set a static IP replace the `iface default inet dhcp` setting for the adapter.

    iface default inet static
        address 10.0.0.10
        netmask 255.255.255.0
        network 10.0.0.0
        gateway 10.0.0.1

#### Wireless Network Configuration
The `/etc/wpa_supplicant/wpa_supplicant.conf` configuration file is used to connect to a wireless network. SSID, encryption protocol, pass key, and other settings are set here. The configuration below was used for WPA2 protocol.

    network={
    ssid="MYSSID"
    proto=RSN
    key_mgmt=WPA-PSK
    pairwise=CCMP TKIP
    group=CCMP TKIP
    psk="MYPASSKEY"
    }

<br/>
Restart the adapter. Most likely it will be up, so bring it down then up again.

    sudo ifdown wlan0
    sudo ifup wlan0

<br/>
There may be some error messages, which is normal.

    sudo ifup wlan0
    ioctl[SIOCSIWAP]: Operation not permitted
    ioctl[SIOCSIWENCODEEXT]: Invalid argument
    ioctl[SIOCSIWENCODEEXT]: Invalid argument

### Checking the Connection

Use the `ifconfig wlan0` to check that you are connected.

    pi@Rasputin:~$ ifconfig wlan0
    wlan0     Link encap:Ethernet  HWaddr 80:1f:02:11:22:33  
              inet addr:10.0.0.12  Bcast:10.0.1.255  Mask:255.255.255.0
              UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
              RX packets:2583 errors:0 dropped:12 overruns:0 frame:0
              TX packets:714 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:1000
              RX bytes:428510 (418.4 KiB)  TX bytes:104111 (101.6 KiB)

<br/>
`iwconfig` will display the wireless network configuration.

    pi@raspberrypi:~$ iwconfig
    wlan0     IEEE 802.11bgn  ESSID:"MYSSID"  Nickname:"<WIFI@REALTEK>"
              Mode:Managed  Frequency:2.462 GHz  Access Point: 70:56:81:11:22:33
              Bit Rate:72.2 Mb/s   Sensitivity:0/0  
              Retry:off   RTS thr:off   Fragment thr:off
              Power Management:off
              Link Quality=100/100  Signal level=68/100  Noise level=0/100
              Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
              Tx excessive retries:0  Invalid misc:0   Missed beacon:0

    lo        no wireless extensions.

    eth0      no wireless extensions.
