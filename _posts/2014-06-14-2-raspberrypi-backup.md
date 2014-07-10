---
layout: post
title: "Raspberry Pi Backup"
description: "Raspberry Pi disk backup"
---
{% include JB/setup %}

## Disk Backup

Create a disk image of the SD card using Mac OS X.

Connect the SD disk to the host machine and search for the disk, `diskutil list`.

    bubbamac:~ bubba$ diskutil list
        /dev/disk0
           #:                       TYPE NAME                    SIZE       IDENTIFIER
           0:      GUID_partition_scheme                        *256.1 GB   disk0
           1:                        EFI EFI                     209.7 MB   disk0s1
           2:                  Apple_HFS OSX SSD                 255.2 GB   disk0s2
           3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3
        /dev/disk1
           #:                       TYPE NAME                    SIZE       IDENTIFIER
           0:     FDisk_partition_scheme                        *3.9 GB     disk1
           1:                WINDOWS_FAT_32 BOOT                 62.9 MB    disk1s1
           2:                       Linux                        33.6 MB    disk1s2

The disk to be cloned is `/dev/disk1`. Initialize the clone by specifying the source disk and destination file.

    sudo dd if=/dev/disk1 of=~/Desktop/2014-06-14_raspi.dmg

The imaging process takes some time (about 20 min for 4GB). The command line tool does not have a progress indicator, but will display a success message when complete.

## Writing the Disk Image

Insert the destination SD card. Using `diskutil list`, find the destination card.

Unmount the card.

    diskutil unmountDisk /dev/disk1

<br/>
Format the card.

    sudo newfs_msdos -F 16 /dev/disk1

<br/>
Similar to the disk image creation, specify the source and destination for the image.

    sudo dd if=~/Desktop/2014-06-14_raspi.dmg of=/dev/disk1

Writing the image to the card will take a lot longer than it did to create the image. It will probably take a few hours to complete.
