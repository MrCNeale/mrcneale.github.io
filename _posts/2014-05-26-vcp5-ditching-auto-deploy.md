---
layout: post
title: VCP5 - Ditching Auto-Deploy!
date: 2014-05-26T18:00:00+00:00
comments: true
tags:
- Auto Deploy
- VCP
---
After being an evangelist last week I'm now converting my Auto-Deploy ESXi hosts into Stateful installs and therefore ditching their wonderful ephemeralness!

Why?  Well  for the purpose of a small lab which I will have to rebuild at some point I don't have the time to configure the supporting architecture which would mainly be the host profiles and dhcp/pxe environment.

Also it's another option and part of using Auto-Deploy.  You can choose to apply a host-profile, once the machine has first booted into ESXi which will then write the install to disk.

Presuming you did assign some storage to your ESXi host (be it virtual or physical) then it is quite simple to make your stateless install stateful in about 2 minutes.
<ol>
	<li>Ensure some storage is available to your host (as per the <a href="http://pubs.vmware.com/vsphere-55/index.jsp?topic=%2Fcom.vmware.vsphere.install.doc%2FGUID-DEB8086A-306B-4239-BF76-E354679202FC.html">vSphere Docs </a>this should be 5.2Gb minimum to host a 4GB scratch Partition on the boot device)</li>
	<li>If this is the first host then Right click the host in VI Client or Web client and select <strong>Host Profile&gt;Create Profile from Host</strong></li>
	<li>Give the profile a name then drill down into the following option in Profile/Policy on the left
System Image Cache Profile Settings
Set the dropdown to "Enable Stateful installs on the host"
Set <strong>Arguments for first disk</strong> to be "local" if you intend to use the first local disk found (There are other options detailed int he vSphere Documentation such as controller names)
Tick <strong>Check to overwrite any VMFS volumes on the selected disk</strong> if you want to overwrite your storage that ESXi finds</li>
	<li>Ok the profile and return to hosts/clusters view</li>
	<li>Right click the host you want to make "stateful" now and select <strong>Host Profile&gt;Manage Profile</strong></li>
	<li>Select the profile you just created and click OK</li>
	<li>Wait, not done yet!!! You've only attached it.</li>
	<li>Put your host into maintenance mode..................</li>
	<li>Then apply the profile via <strong>Host Profile&gt;Apply Profile</strong></li>
	<li>vCenter shows you the configuration changes it will make to get the host in what it believes will be a complaint state to match the profile.
This should  just be listing the Disk to be used for stateful installs
and also VMFS volumes on that disk will be overwritten.</li>
	<li>Click Finish and go and get a brew whilst the <strong>Apply host configuration</strong> step completes.  This took 15 minutes on my virtual ESXi host and then 5 minutes to reboot!</li>
</ol>
Just as a minor memory saver I've stopped Auto-Deploy on the vCenter Appliance too.

That will conclude my adventures in Auto Deploy land but it has led me nice and gently into host profiles and I'll be working on them in the next blog (as you'll find if you do the steps above..you won't end up with a perfect ESXi host after the reboot step!)

Bedtime now!

&nbsp;
