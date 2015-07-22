---
layout: post
title: Best VCAP Study Aid is another person's time & help
date: 2015-01-02 00:39
author: chrisneale
comments: true
categories: [Exam, Homelab, Study, Study, VCAP, vcap-dca, vExpert, VMWare]
---
Like many I'm currently cramming for my VCAP-DCA.

The reason behind the cramming is that it's on the 15th Jan and it's DCA510 which has been retired so I couldn't push it back despite not getting the voucher long ago or starting studying long ago enough.

That's my excuses out of the way early.

So I could write my own blog posts breaking down the blueprint.  Well that is useful to me, being able to explain something to a level where someone else can understand it is a good demonstration of your knowledge of something usually....but that's been done to death.  There are hundreds of VCAP Blueprint blogs out there, some great, some ok.  I'll list out some resources at the end.

The reason for this post is that in searching for study aids I found Josh Andrews blog http://sostechblog.com/ There are lots of VCAP blog posts on there, including interesting ones about setting up your own lab, something I've done (another post coming on that too!), and most importantly his Test Track Lab.  Hmmm my interest was piqued here.  There was an RDP file and a mention of login instructions!??

I read on and Josh had set a basic lab, 2 nested hosts, DC, VC and some powered off VMs on the nested hosts.  He had tried to make it as representative of a VCAP test without breaching any NDAs.  As you will have seen from the description of a VCAP exam and the demo flash http://mylearn.vmware.com/courseware/82526/VCAPDCA_Tutorial.swf much of the setup is described here and is what 99% of us will have in our home labs:
<ul>
	<li>2 nested hosts (or running on an HP Microserver or equivalent)</li>
	<li>DC (with Powercli and putty and viclient installed)</li>
	<li>vCenter</li>
	<li>It didn't have VMA, it didn't have a VUM server. Both of those you can configure and add easily yourself</li>
</ul>
Nothing amazing thus far.  But here is the great bit.  Just before VMworld 2012 Josh created his lab on a laptop roughly described above.
<ul>
	<li>He then went further and wrote some scenarios that you should know if you're going to attempt VCAP-DCA.  Currently these focus on VM management, Storage Management, Network Management and Miscellaneous tasks.</li>
	<li>He wrote four sets of these.</li>
	<li>His lab also contains scripts to reboot and reset the lab in between Sets of tasks</li>
	<li>He also wrote powershell scripts to check your homework!
It's pretty amazing and the final amazing part is..........</li>
	<li>He let's strangers on to this lab remotely!!!!  Giving away his compute, electricity and hard work for the benefit of fellow VMware enthusiasts.</li>
</ul>
I think this is a great example of how the VMware community can really do amazing things.  Taking time out of his day to setup and schedule me a slot to use his lab and scenarios has really boosted my thirst for learning.  I quickly realised there were some areas I needed to brush up.  But in others I confirmed what I knew and I even found a glitch in one of the scoring scripts ;-)  (That is in no way a criticism as the lab was set up some time ago and any edits can easily be lost with the non-persistent disks and also covers 5.1 and 5.5)

Josh is already, unsurprisingly, a vExpert and a VCI/VCP/VCAP......  If you're looking for something to test our your skills and to inspire you to better automate and configure you own home lab then I highly recommend looking through Josh's posts and getting in touch with him via Twitter or e-mail.

Here's where I got started with Josh's Test Track

http://sostechblog.com/2014/02/07/vcap-test-track-lab-on-a-lap/
