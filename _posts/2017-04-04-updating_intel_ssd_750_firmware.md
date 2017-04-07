---
layout: post
title: Updating Intel 750 SSD Firmware on system with Pascal GPU
published: false
---

This is an interesting piece to start a blog with, but I guess one has to start somewhere.

One of my interests is deep learning research, so I got to build myself a workstation last year. How to build a good deep learning machine is another days topic which I may write about (you may want to check grapifix guide for now or tims great blog post about it), but today’s topic is Intel 750 1.2TB SSD.

I am using a workstation with Intel 750 1.2TB SSD and Titan X Pascal GPUs. Something struck me with this particular SSD. Although the reported read speeds were around 2.6GB/s and I could reach those speeds with multithreaded reading. There seemed to be something odd with single thread reading. When you have small networks like Alexnet with such strong GPUs, loading the data from the hard drive may become a significant bottleneck for training. Seeing the activity led on this drive is blinking like christmas lights no matter if it is idle or not made me question the firmware on the drive. 

Checking intel firmware logs [link to release log] I realized that they have addressed some performance issues in October 2016. Great, now how do we update the firmware on Ubuntu. Intel has SSD toolbox [link to toolbox] for windows users. They also have a firmware update tool. Following their steps to put their tool on a USB and booting from that causes an “out of range error”. Now, I could not locate the root of this problem and decided to follow a different route. Lets create a Windows-To-Go usb and update the firmware with their toolbox instead.

First, download windows 10 iso following the steps here:
https://www.howtogeek.com/186775/how-to-download-windows-7-8-and-8.1-installation-media-legally/
Second, download rufus https://rufus.akeo.ie. If you don’t have access to a windows machine, you can install the just downloaded windows on a virtual machine.

Open rufus, 
Install windows-to-go

Plug your windows-to-go drive and boot from it by changing the boot order from BIOS.

Once windows boots up, install intel nvme drivers [https://downloadcenter.intel.com/download/26451/Intel-SSD-Data-Center-Family-for-NVMe-Drivers?product=79678]. If you use windows generic storage controller drivers the toolbox wiill have update firmware option greyed out.

install toolbox https://downloadcenter.intel.com/download/26574/Intel-Solid-State-Drive-Toolbox

update firmware
restart unplug usb and boot as usual to ubuntu
check read speed with simple python script 
500 -> 950 MB/s great!
voila.


nouveau driver https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1602340
