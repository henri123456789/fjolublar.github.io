---
layout: post
title:  "Raspberry Pi"
date:   2021-01-21 01:27:15 +0200
categories: Linux
---

In this page you'll find `Raspberry Pi` commands and other useful information.

<style>
.tablelines table, .tablelines td, .tablelines th {
        border: 1px solid black;
        }
</style>

### Creating an Image from SD-card and vice-versa

| **Command**  | **What does it do?** |
|:-------------:|:-------------:|
| `sudo fdisk -l`                                     |  Check SD-card name |
| `sudo umount /dev/mmcblk0p0`                        |  Unmount the SD-card partitions |
| `sudo dd if=/dev/mmcblk0 of=~/raspbian_backup.img`  |  Create an Image from an SD-card |
| `sudo dd if=~/raspbian_backup.img of=/dev/mmcblk0`  |  Copy the image to an SD-card |
| `sudo pishrink.sh raspbian_backup.img`              |  Shrink the Pi Image.  \*First install pishrink |
{: .tablelines}
