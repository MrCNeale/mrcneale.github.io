---
layout: post
title: My vSphere HomeLab
date: 2015-01-04 01:32
author: chrisneale
comments: true
categories: [Uncategorized]
---
Everyone's got one, and like petrolheads showing off their v8 or GTI here's my vSphere homelab.

Originally it was just an HP Microserver accessed via a pc running virtualised esxi hosts.

But I needed to upgrade my main PC and I still had a VMWorkstation 10 license from my VCP5 in June to use up.

So I went for a Core i7 4770s.  It's quad core with HT so 8 threads for VMs to consume.  Currently only 16gb but it was only built in December and now Christmas is out of the way the first thing after January's payday will be another 16Gb to take it to 32Gb.  I have a 256Gb Samsung Pro SSD in and run Windows 7 Home Premium +VMWorkstation 10.

I already had the SSD/PSU/Blu-Ray Drive so the upgrade for case/Mobo/CPU/Memory cost me around £500 which is a bargain considering I haven't upgraded my PC for around 5 years and had been running an E6550 with 4Gb Memory.  Future Proofing doesn't exist but I think I'll get another 5 years at least out of this (once I add the next 16Gb) so £100/year for a Micro-ATX sized vSphere Lab+PC is good value in my book!

I will probably at some point recommission the HP N36L Microserver which has a puny CPU but has 16Gb too so I could do real, proper vmotions not virtual vmotions over virtually nested vswitches inside VMWorkstation (serious Inception style confusion).  However it doesn't support Passthrough/Direct I/O so I'm not rushing on that one :-)

To help understand iSCSI and NFS storage I use my Synology DS212j NAS with 2x1Tb Western Digital HDDs in, running mirrored (well it's Synology Hybrid RAID, whatever that is :-) but with just two drives it behaves like RAID 1 and eats up 50% of your capacity).  I use the NAS for storing all my other personal stuff so it work quite well as a Lab NAS as well as a home NAS without one impacting on the other.  I'd really recommend it for Lab and home/media/backup/dlna use.  They even support VAAI which makes them a great choice for a vSphere lab.

Here it is as a Google Drawing (I don't have a visio license!) alongside a photo of the kit.

<a href="https://chrisneale.files.wordpress.com/2015/01/homelab.png"><img class="alignnone size-medium wp-image-217" src="https://chrisneale.files.wordpress.com/2015/01/homelab.png?w=300" alt="HomeLab" width="300" height="225" /></a><a href="https://chrisneale.files.wordpress.com/2015/01/img_20150103_000850.jpg"><img class="alignnone size-medium wp-image-218" src="https://chrisneale.files.wordpress.com/2015/01/img_20150103_000850.jpg?w=225" alt="IMG_20150103_000850" width="225" height="300" /></a>

I've been using it for a week or so now in anger as a lab to learn the VCAP-DCA510 objectives on.  I know I need to build/add a server for VUM (don't really want or need to install it on the DC) and I need to create some more client VMs on the nested ESXi hosts, but for now it's running fine and with NAT'd networking as I haven't had chance to "Design" a network for the vSphere layer and isolate is and manage it properly (I'll do that for the DCD :-)

My VCAP-DCA exam is on the 15th Jan.  I got the voucher for VCAP-DCA510 in September but got seconded to a very very very very non-technical piece of work so my hands-on skills started to atrophy quickly.  Like many of us I postponed it as I knew the assignment was temporary.  It finished just before Christmas and when I came to reschedule I got a shock because I couldn't.  Obviously, now, that is because it is being retired at the end of Jan.  So I am cramming in as much practice and focus as I can leading up to that and am taking some study leave the 3 days leading up to the exam.  I am fortunate that I only have to drive one hour to the test centre.  I have seen tales of people having to fly across the US or drive/fly around Europe.  However relatively speaking it is three to four times further than I normally travel to sit VMware/MS exams.  I would like to see VMware expand the locations in which you can sit the exam by working with testing partners to get their sites up to whatever spec is required.

<em><strong>Administrator Tools</strong> </em>and <em><strong>Network Administration</strong> </em>Done.
On to my next Chapter/Objective of the <a href="http://www.amazon.co.uk/VCAP5-DCA-Official-Cert-Guide-Administration/dp/0789753235/ref=sr_1_1?ie=UTF8&amp;qid=1420335087&amp;sr=8-1&amp;keywords=vcap-dca">Official Cert Guide</a>!

<em><strong>Storage Concepts</strong></em>

&nbsp;
