---
comments: true
date: "2014-05-12T18:00:00Z"
tags:
- Study
title: VCP5 - Objective 1.2 "Quickly" Deploying Using AutoDeploy (3hrs later!!! ;-)
---
So tonight's revision was meant to be pretty quick.  It is Game Of Thrones night after all!

Well no such luck.  I powered on my Microserver host and the vCenter appliance fired up with it automatically.  Nice!

Logged on to the web client and the fat client.  I have been trying to alternate between the two to make sure I can spot any differences, or things you can't do in one but can in the other.

Item 1 on my mini-agenda.  "Sizing the vCenter Database".  Now technically that is Objective 1.1 but unless you use the v4 spreadsheet from VMware, then you can't size your VC DB until after you've installed Vcenter.  Crazy!

So I looked in the web client and it doesn't seem to be an option in there....back to full VI client.
Sizing the DB was simple.  Go to Home-&gt;Administration-&gt;vCenter Server Settings-&gt;Statistics.  From there you can plug in your number of hosts/vms and frequency of stat collection.  This will show you estimated DB usage so you can resize your newly created VCenter DB as appropriate.

Next on to AutoDeploy.  I have the vCenter Appliance installed so I logged in to it via https on port 5480 and started the autodeploy service.  A quick trip back to the full vi client and voila, under Home-&gt;Administration, AutoDeploy has now appeared.  The slight surprise was that there ain't much to config.  This is where RTFM comes in and you find you need a DHCP server (or to set the config on your existing one) to point to your TFTP server (which you need to setup/install).

I did a quick search for a pre-packaged v.appliance, but none sprang out from the marketplace (which seemed a bit flaky).  So I opted for a Linux box.  Trying to be clever I picked an older distro of Ubuntu v9 to try and make install quicker.  But that backfired so here I am leaving the host and my NAS (serving an iSCSI lun to my ESXi host) on overnight to update flipping Linux!

Lesson learned!  Just not the intended one for tonight.

...Tomorrow night....actually doing something with AutoDeploy other than just switching it on :-)

<a href="http://chrisneale.files.wordpress.com/2014/05/auto-ubu-screengrab.png"><img class="aligncenter size-large wp-image-133" src="http://chrisneale.files.wordpress.com/2014/05/auto-ubu-screengrab.png?w=750" alt="auto-ubu-screengrab" width="750" height="405" /></a>

&nbsp;
