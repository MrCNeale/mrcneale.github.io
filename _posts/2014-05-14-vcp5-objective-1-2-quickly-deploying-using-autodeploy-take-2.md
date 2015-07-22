---
layout: post
title: VCP5 : Objective 1.2 “Quickly” Deploying Using AutoDeploy - Take 2
date: 2014-05-14 12:47
author: chrisneale
comments: true
categories: [Study, Uncategorized]
---
So when last we spoke I had left Linux updating.

It was when I came to resume last night that I couldn't track down the original article I had been following.

It was only when I found<a href="http://www.vclouds.nl/vsphere-5-auto-deploy-in-20-steps/"> Marco Broeken's blog </a>"vSphere5 Auto-Deploy in 20 Steps".  This started straight away with the, now, blindingly obvious statement that the VCVA is a Linux box, therefore my Ubuntu box was a waste of space.

Simply configure and start the Tftp Daemon and the DHCP daemon services on the VCVA server and then you're ready to move on to downloading the Image Bundle zip from VMware and importing that via the powerCLI <strong>Add-ESXSoftwareDepot. </strong>I did have some issues with a corrupted install of powercli 5.5 and 5.1 on the same PC that still isn't resolved even after combing the registry and file system.  Luckily I had my laptop to run the PowerCLI steps from.

Minor edits to Marco's post are to
<ul>
	<li>Just list the filename not the full /tftpboot folder.  Then my (virtual) ESXi Host started to gpxe boot ok.  However it did pick up a different IP for the tftp step.</li>
	<li>Disable any other DHCP servers you may have on your lab network.  In my case my Sky Hub was sneaking in there and giving the gpxe boot an ip address and therefore my host wasn't getting my tramp.</li>
</ul>
Also contrary to my previous post I did find where in the web-client the AutoDeploy settings are:

<strong>Drill down to the vCenter Server system. Click the Manage tab, select Settings, and click Auto Deploy.</strong>

Here are some pictures of the amazing process of esxi loading itself onto a VM, running on an ESXi host!

It was the final message about persistent storage that taught me the most about Auto-Deploy, which will follow in the next post!

<a href="http://chrisneale.files.wordpress.com/2014/05/autodeploy-working.png"><img class="aligncenter size-large wp-image-139" src="http://chrisneale.files.wordpress.com/2014/05/autodeploy-working.png?w=750" alt="autodeploy-working" width="750" height="421" /></a> <a href="http://chrisneale.files.wordpress.com/2014/05/autodep2.png"><img class="aligncenter size-large wp-image-138" src="http://chrisneale.files.wordpress.com/2014/05/autodep2.png?w=750" alt="autodep2" width="750" height="405" /></a> <a href="http://chrisneale.files.wordpress.com/2014/05/autodep3.png"><img class="alignnone size-large wp-image-137" src="http://chrisneale.files.wordpress.com/2014/05/autodep3.png?w=750" alt="autodep3" width="750" height="405" /></a>
