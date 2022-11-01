---
comments: true
date: "2014-09-22T18:00:00Z"
tags:
- AD
- GPO
- Windows
- XenApp
- Citrix
title: vSphere VM Hotplug functionality confuses AD GPO and locks out non system drives
---
I was asked to look at an issue on a customer account where when a server VM, used as a XenApp Dynamic Desktop server is moved from Computers OU to the desired destination OU and gpupdate run then suddenly the D and E drives were inaccessible.  They could be seen in "My Computer" and also in "Disk Management" but only listed as NTFS and no used/free space in My Computer, despite me having admin rights.

&nbsp;

Obviously this had to be caused by policy as that's the difference between moving the VM into the new OU and running gpupdate.

&nbsp;

I reviewed the policies, there weren't a vast number, but none appeared to have anything about locking down local fixed drives.

The key word there is FIXED.  There was a setting in one GPO under the section

<strong>Administrative Templates/</strong><strong>System/</strong><strong>Removable Storage Access</strong>

For

<strong><strong>All Removable Storage classes: Deny all access</strong></strong>

Which was to lock down USB drives etc.

&nbsp;

But these were fixed drives.................or were they?

Well this is where VMware is clever and Windows isn't quite caught up, or you might argue VMware is TOO clever.

The SCSI controller provided by VMware is detected as hotplug.  You can confirm this by going to the system tray and clicking the eject/remove drive icon

<a href="https://chrisneale.files.wordpress.com/2014/09/sg.png"><img class="alignnone size-full wp-image-193" src="http://chrisneale.files.wordpress.com/2014/09/sg.png" alt="sg" width="234" height="62" /></a>

Fortunately as it booted from it C isn't able to be ejected, but the other drives were therefore seen as removable storage and locked down.

&nbsp;

Two solutions presented here.  One is change the GPO.  This is a VM on a host in a secure data center.  No-one's plugging a USB into that host and mapping it to the VM via vCenter or Directpath any time soon.

&nbsp;

However there is a VMware workaround.  You can disabled the hotplug functionality of the scsi controller driver.  Thereby "un-confusing" Windows.

Simply edit the VM configuration under the settings options/general/configuration parameters and add the setting

<span style="font-family:'Open Sans', sans-serif;line-height:19.5px;">devices.hotplug and</span> set the value to <strong>false</strong>

as described here.

<a title="http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&amp;cmd=displayKC&amp;externalId=1012225&amp;ui=www_cert&amp;src=vmw_so_vex_cneal_850" href="http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&amp;cmd=displayKC&amp;externalId=1012225&amp;ui=www_cert&amp;src=vmw_so_vex_cneal_850">VMware KB: Disabling the HotAdd/HotPlug capability in ESXi 5.x and ESXi/ESX 4.x virtual machines</a>

&nbsp;

And a reboot later and Windows now knows these aren't removable drives and all is well with the world

&nbsp;
