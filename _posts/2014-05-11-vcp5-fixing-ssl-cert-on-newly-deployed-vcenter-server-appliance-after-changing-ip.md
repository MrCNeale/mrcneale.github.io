---
layout: post
title: VCP5 - Fixing SSL Cert on newly deployed vCenter Server Appliance after changing IP
date: 2014-05-11 20:19
date: 2014-05-11T18:00:00+00:00
comments: true
tags:
- SSL
- Study,
- Vcenter Server Appliance
- VCSA
---
So you deploy the VCA from the OVA.  Easy peasy.  Any monkey could do that!

You config it from the webpage.  Things like changing the default password, general good practice etc.

....and obviously you change the IP to a static one.

<strong>Wait! Stop!  You just broke the VCA.  </strong>Well you sort of did.  You changed the name but didn't update the SSL cert that was generated when it was first built/configured.  Hrrrumph.

Ok so how do I update it then to the new IP/name I've just given my shiny new???

Don't panic, it's relatively simple.  Go to the webpage to manage your VCA https://192.168.0.200:5480/ in my case.  Click the Admin Tab. Click <strong>Toggle Certificate setting, </strong>now go to the System Tab and reboot the VCA.

If yours is a lab like mine and you're running the VCA with the minimum 4Gb....then go and get a coffee or do some star jumps...it will be a while!

<a title="VMware Docs Article" href="http://pubs.vmware.com/vsphere-51/index.jsp#com.vmware.vsphere.troubleshooting.doc/GUID-D9ACB836-2BB4-4DDC-9383-91EF6E546A30.html" target="_blank">http://pubs.vmware.com/vsphere-51/index.jsp#com.vmware.vsphere.troubleshooting.doc/GUID-D9ACB836-2BB4-4DDC-9383-91EF6E546A30.html</a>
