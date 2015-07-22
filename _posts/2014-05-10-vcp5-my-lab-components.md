---
layout: post
title: VCP5: My Lab Components
date: 2014-05-10 15:21
author: chrisneale
comments: true
categories: [DS213j, ESXi, Home Lab, Homelab, HP Microserver, iSCSI, LUN, N40L, nas, NFS, Study, synology, Uncategorized, VCP, VCP5, VCP5-DCV, whitebox]
---
This one should hopefully be short :-)

I have: 
	<a href="http://chrisneale.files.wordpress.com/2014/05/n40l.jpg"><img src="http://chrisneale.files.wordpress.com/2014/05/n40l.jpg?w=262" alt="n40l" width="262" height="300" class="alignright size-medium wp-image-112" /></a>HP N40L Microserver.  If you've looked into a good cheap box to run ESXi on then you have surely come across this server or one of it's more recent sped up relatives.
<a href="http://h20195.www2.hp.com/V2/GetDocument.aspx?docname=c04111672&amp;doctype=quickspecs&amp;doclang=EN_US&amp;searchquery=Servers/HP%20ProLiant%20MicroServer/658553-421&amp;cc=uk&amp;lc=en" title="Quick Specs for N40L">Here are the quick specs</a>
It came slightly underspecced with only 2GB.  That was quickly upped to 8GB.  It can run 4-6 VMs comfortably depending on what spec you want to give them and how heavily they are utilised.

<a href="http://chrisneale.files.wordpress.com/2014/05/nas.jpg"><img src="http://chrisneale.files.wordpress.com/2014/05/nas.jpg?w=192" alt="nas" width="192" height="300" class="alignleft size-medium wp-image-115" /></a>Synology DS213j NAS box.  With 2x1TB drives in, mirrored.  This may seem like a lot, but this is also my NAS box for my home network storing my CDs, music downloads and rips of the kids' DVDs so that they can watch them on the xbox or tablet.  I use less than 100Gb on the NAS.  It's primary use in the lab is to make iSCSI and NFS VCP objectives much simpler to try out practically (anyone shouting OpenNAS/FreeNAS now can just shush!).

<img src="http://ecx.images-amazon.com/images/I/71c2KHp59WL._SL1500_.jpg" width="375" height="104" class="alignright" />Netgear GS308-100UKS 8 Port Gigabit Ethernet 10/100/1000 Mbps Switch.  £22 from Amazon.  Bargainous and it means that you are running your shared storage over 1Gbps network and aren't getting interference from your Sky+HD box downloading the latest episode of 24!!!

I've got a "Big PC" which I run VI Client from.  It's just a home built PC, E6650, 4GB ram and 256GB SSD, just think of it as your management Console.  
I've also got a work laptop, but I'm pretty sure I won't be using that in this scenario...
.......and finally yes, I know I need to dust/hoover the NAS/Microserver!

