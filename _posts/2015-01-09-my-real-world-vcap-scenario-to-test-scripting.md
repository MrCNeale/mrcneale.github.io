---
layout: post
title: My Real-World VCAP Scenario to test scripting
date: 2015-01-09 00:45
date: 2015-01-09T00:45:00+00:00
comments: true
---
I've been racking my brains thinking of a real-life VCAP-esque scenario that would force me to write a PowerCLI script.

I know Josh Andrews offered up "create 100 powered off VMs with a certain config or clones of an existing template" which is good but not something I'd normally do in my role.  Then one came to me.  So here it is

*****Disclaimer.  I have not sat the VCAP-DCA yet so have no visibility of the questions.  This question is based on a real-life scenario from my work but modified a bit.  It's main purpose is to test/teach you PowerCLI so you get the hang of the main components of a basic vSphere PowerCLI script.  *****

<hr />

&nbsp;

<strong>Scenario 1</strong>

Your company has reviewed storage use and found that a large amount of storage has been taken up by a group of VMs with names starting VMP.
Upon further inspection an engineer identifies that the pagefiles ARE on a separate drive, which is a the secondary virtual disk, but they are using Tier1/Premium storage.
Your storage team identify sufficient space on Tier2 storage and present an empty LUN which is visible to all your hosts.

You need to ensure that page files on the existing VMs are on Tier2 storage.
A new LUN has been presented with identifier naa.6001405df3d2046d4a02d3fe4db20bd5
Use iSCSI01 as the new datastore name and  use host 192.168.88.211 to create the lun from.
All secondary vmdks were created using the default naming convention.
1) Create a datastore on the new LUN
2) Migrate all VMs second virtual disk for the VMP* VMs to the new LUN

<hr />

This isn't rocket science and on a small number of VMs you could easily drag and drop in the GUI. But let's say it's 100 VMs or 1000.  Then you really want to be getting home some time today!
Breaking down the first step.  We have a host and a device identifier for the lun.  We have a host to create it from and we have a name for the new datastore.  If some of these weren't given then they could be found using <strong>Get-ScsiLun -vmhost 192.168.88.211</strong>

new-datastore -vmhost 192.168.88.211 -name iSCSI01 -path "naa.6001405df3d2046d4a02d3fe4db20bd5" -vmfs -FileSystemVersion 5
<a href="https://chrisneale.files.wordpress.com/2015/01/lunadd0.png"><img class="alignnone wp-image-229 size-large" src="https://chrisneale.files.wordpress.com/2015/01/lunadd0.png?w=800" alt="lunadd0" width="800" height="109" /></a>

Our Datastore is created.  You can watch that happen in vSphere client if you like.
<a href="https://chrisneale.files.wordpress.com/2015/01/lunadd.png"><img class="alignnone wp-image-230 size-large" src="https://chrisneale.files.wordpress.com/2015/01/lunadd.png?w=800" alt="lunadd" width="800" height="349" /></a>

Now we need to move all the secondary disks.
We know the VMs are all named with the prefix VMP
We know that their secondary disk follows the default naming convention.  e.g. vmname_1.vmdk
We know they need to be moved to Datastore iSCSI01

This snippet stores the name of the Datastore in one variable and then stores the harddisk details for any VMs hard disk where the VMname matches VMP* and the hard disk filename matches *_1*
It then invokes the move hard disk cmdlet to actually relocate the vmdks.

$myDS = get-datastore -Name "iSCSI01"
$myDisk = get-vm | where-object {$_.name -like "VMP*"} | get-harddisk | where-object {$_.filename -like "*_1*"}
move-harddisk -harddisk $mydisk -datastore $myDS

As seen from the command line, I notice I need to add a <strong>-Confirm $false </strong>to the New-Datastore command to stop the interactive prompt :-)
<a href="https://chrisneale.files.wordpress.com/2015/01/lunadd3.png"><img class="alignnone size-large wp-image-231" src="https://chrisneale.files.wordpress.com/2015/01/lunadd3.png?w=800" alt="lunadd3" width="800" height="116" /></a>

And viewed from VI Client

<a href="https://chrisneale.files.wordpress.com/2015/01/lunadd2.png"><img class="alignnone size-large wp-image-232" src="https://chrisneale.files.wordpress.com/2015/01/lunadd2.png?w=800" alt="lunadd2" width="800" height="105" /></a>

So there we have it.  To make it a standalone runnable script, simply add 2 lines to the top.  One to add the Powershell Snapin for vSphere automation.  Then connect to a vCenter server.
<p style="text-align:left;"><strong><em>add-pssnapin vmware.vimautomation.core</em></strong>
<em><strong>connect-viserver vc</strong></em></p>
<p style="text-align:left;"><strong><em>$newLun = get-scsilun -vmhost 192.168.88.211 | Where-Object {$_.canonicalName -like "naa*"}</em></strong>
<strong><em>new-datastore -vmhost 192.168.88.211 -name iSCSI01 -path "naa.6001405df3d2046d4a02d3fe4db20bd5" -vmfs -FileSystemVersion 5</em></strong></p>
<p style="text-align:left;">
<strong><em>$myDS = get-datastore -Name "iSCSI01"</em></strong>
<strong><em>$myDisk = get-vm | where-object {$_.name -like "VMP*"} | get-harddisk | where-object {$_.filename -like "*_1*"}</em></strong>
<strong><em>move-harddisk -harddisk $mydisk -datastore $myDS -Confirm $false</em></strong></p>
<p style="text-align:left;"></p>
<p style="text-align:left;">I left out the bit where it took me an hour to see the iSCSI Lun because the networking on my virtual hosts went screwy...!  But otherwise it felt like a productive night!</p>
<p style="text-align:left;"></p>
&nbsp;
