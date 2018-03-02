---
layout: post
title:  "Raspberry Pi Boot Issue - Root account locked!"
date:   2017-11-05 09:03:13
categories: blog
---

Last weekend I took a project to build myself a nice Pi powered NAS. Everything worked perfectly, but as I saved my changes to `/etc/fstab` and rebooted all hell broke loose. The Pi would simply not boot up. I run almost all my Pis in a headless enviroment so I had to get hold of our TV and a keybaord to see what was going on. 

Lo and behold, there it was, The Pi would get stuck at boot with the following error message

	"Cannot open access to console, the root account is locked." 

The error message while not too descriptive, it asked me use `sulogin` and run `journalctl -xb` however there was no shell to run or list anything. 

After some frantic net searches and a lot of trial and erros, I found the following workaround that worked. Note: You will need access to a display,keyboard and a laptop/desktop to do the following steps 

1. Retrive your SD card from the Pi and sing a adapter mount the card to your PC,Mac or Linux.
2. You should be able to see the /boot partition of your SD card.
3. Locate the file cmdline.txt and add the following at the end of the line `init=/bin/sh` Note: Do not create a new line, just add the above to end of the current line.
4. Load the SD card back to your pi and boot up. 
5. You should now get the a root shell prompt. From here you can undo the changes to `/etc/fstab` or whatever else that initially broke your system

In some cases you will not be able save your changes and the system will complain of a read only file system. If you get that move to the next sections

A raspberry pi SD card will has two main partitions, since we cannot read the partition table directly you must manually locate the device for your root and boot partitions. You can do this by going to the `/dev` directory and you should see something similar to `mmcblk0p1` & `mmcblk0p2`. The second device `mmcblk0p2` will be your root partition. You need to remount this with read write permissions

	mount -o remount,rw /dev/mmcblk0p2 /

Once this is remounted, go ahead and edit your /etc/fstab and save it.

Before you exit, make sure you revert the change to the cmdline.txt in the /boot partition. You may need to mount that in read write mode as well before you can change it.

	mount -o remount,rw /dev/mmcblk0p1 /boot

Alternatively you can revert back the change to cmdline.txt on your laptop or desktop.

If everything goes well, you should be able to boot back your Pi in a normal way.
