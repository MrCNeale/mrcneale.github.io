---
date: "2014-05-10T18:00:00Z"
tags:
- Amazon Web Services
- AWS
- Study
- UCS
- vBlock
- vCD
- vCD5.1
- VCE
- VCP5
title: VCP5 - Me and my virtual history
---
I'm 37, work at a global IT services company with big customers.  When I started in 2005 my first project was helping deploy 10s of ESX hosts.  Yes "ESX" v2.5 "Virtual Infrastructure" hosts and Virtual Center 1.something or other.  It's 9 years ago now but we had multiple farms of 7 hosts (I think) which had shared LUNs, and all ran training courses.  Each training course was 6 VMs.  Only 1 was unique as a citrix presentation server v4 front-end. The other 5 were identical copies on an internal private network, to stand up a training environment to train lots of people at lots of sites on a particular product.  At it's peak there were around 1500 VMs running and regularly "rebuilt" as training enviroments needed to be restarted.

Back in those days, when clouds were things in the sky full of rain and automation was DIY, the way a training course got "built" was simple.
<strong>A 1 line command! passing an IP and a training course number/id.
</strong>
Admittedly that one line command went off and ran a big shell script on the ESX host (ahh the good old days of having the shell still included, pre ESXi)
It used vmkfstools to provision the 5 copies and renamed the vmx and vmdk files and their entries which were registered in vcenter.
The 6th, and tricky one was the Citrix Presentation server which was sysprepped, ready and waiting to run a clever vbs script after unprepping.
The VM would be provisioned as the non-unique VMs, however before powering on the vmx file would be edited heavily using SED to change the VMs MAC address to match the training course ID.
After that when the VM booted up the sysprep would complete and run a vbs script which would query the MAC address and then rename the server accordingly and configure it's networking, join it to the internal private domain etc.

All magical stuff.  But took a lot of engineering first.
Since then I've worked on plenty of other accounts, sadly some still use VI3 (maybe it's just so good they can't see the cost/benefit of upgrading!).  Lots, possibly the majority are still on vSphere4 and many are now moving to v5, frequently via the Cloud route.

In the middle of that in Jan 2011 I deployed a vBlock to a large UK customer of ours.  The purpose of this was to P2V the majority of their server estate.  Some 900 servers.  I have to admit that after analysis this number rapidly shrunk to around 600.  Why?  Well a number of reasons, practicality, legacy, regulatory.  Since then they have had 3 more vBlocks and migrated plenty more so I'm sure a lot of those blockers have been worked out.  This was a great experience, seeing a vBlock in the flesh/metal was cool as the majority of my work is/was remote, so it was nice to get some hands on (although I had a horrendous cold/flu and freezing server rooms weren't helping).

Following that I have worked more on UCS (non-vBlock systems) and on the whole it's a good design/idea, but when it comes to the upgrade of the central components it is a gargantuan process that scares engineers and customers alike :-)

Most recently I've been loaned out to our Cloud division and have been working on newer vBlocks 100/200/300's.  This has included first using vCloud Director as a "customer" on a project for one of our customer labs, and then as an administrator to create other new customer Clouds, including the creation of a Virtual Private Enterprise Cloud for one customer.

Finally most recently I was involved, albeit briefly as the project was rescoped, in building out systems on Amazon Web Services.  This was different again as there was provision of the Standard Amazon Machine Images(AMIs) is a different way of working as we normally have our own in-house Machine Images (Or SOEs).

So virtualisation, virtualisation, virtualisation.
Ever since VI, ESX2.5 and seeing a vMotion transfer a live running VM, from one physical host to another....I've been hooked
[caption width="356" align="aligncenter"]<a href="http://technichelp.blogspot.co.uk/2013/07/vmotion-powered-on-virtual-machine.html"><img src="http://chrisneale.files.wordpress.com/2014/05/0bf8e-vmotion.jpg" width="356" height="209" class /></a> Image from 
http://technichelp.blogspot.co.uk/2013/07/vmotion-powered-on-virtual-machine.html[/caption]
