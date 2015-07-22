---
layout: post
title: VCP5 Revision: WWTWCD - What Would The Web Client Do?!
date: 2014-06-03 22:21
author: chrisneale
comments: true
categories: [binding, ESXi, Port group, Uncategorized, VCenter, VCP5, VCP5-DCV, VDS, vsphere, Web Client]
---
Very very brief one this, but it had me foxed for 5 minutes and time is precious!

I was looking into Distributed Virtual Switches and reading up on Port Bindings.  I wanted to see what the options were (kinaesthetic learner me!)

Opened up my DVS in the vSphere client and right click, edit settings, and voila.....!?

<a href="https://chrisneale.files.wordpress.com/2014/06/viclientpg.png"><img class="alignnone size-medium wp-image-158" src="http://chrisneale.files.wordpress.com/2014/06/viclientpg.png?w=300" alt="viclientpg" width="300" height="224" /></a>

Erm the option is not there.

After checking the vSphere Docs online
[embed]https://pubs.vmware.com/vsphere-51/index.jsp?topic=%2Fcom.vmware.vsphere.networking.doc%2FGUID-69933F6E-2442-46CF-AA17-1196CB9A0A09.html[/embed]
I spotted the obvious.  There are two "methods" for doing things with DVS port groups, whether that is adding them, editing them or editing advanced settings.

<a href="https://chrisneale.files.wordpress.com/2014/06/viwebclientpg.png"><img class="alignnone size-medium wp-image-157" src="http://chrisneale.files.wordpress.com/2014/06/viwebclientpg.png?w=300" alt="viwebclientpg" width="300" height="122" /></a>

If you do it via the web-client you will get more.

VMware themselves did do a blog post about this back in 2012 which overviews at a high level which client to pick.

[embed]http://blogs.vmware.com/vsphere/2012/11/which-vsphere-client-should-i-use-and-when.html[/embed]

To be honest I would prefer it if they would make their mind up and pick one.  I know 5.5 warns you know when you launch the fat client that it is on it's way out, but it's still the first step to connecting to an ESXi host if it's in your lab or the first one you build or if you are, for instance, patching your vcenter VM and want to have the ability to easily revert to a snapshot etc.

<em><strong>In short, if you can't find it in the vSphere Client...check the Web-Client first!</strong></em>
