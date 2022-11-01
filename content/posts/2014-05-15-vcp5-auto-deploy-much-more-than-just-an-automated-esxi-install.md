---
comments: true
date: "2014-05-15T18:00:00Z"
tags:
- Auto Deploy
- ESXi
- Study
- VCP
title: VCP5 Auto Deploy - Much more than just an automated ESXi install
---
Revision.  It means to review (previously studied materials) according to dictionary.com

However as anyone who has ever studied for a tech exam knows it is usually the complete opposite!  You know what you know.  You work with it and click those options every day. BUT you know that the exam covers all the things you never use, some you may have never even looked at.

<em><strong>"Connecting windows to a Unix print server!!! NO-ONE does that, so why is there a question on it!!!"</strong></em>

So after reading the first objective about vcenter/vsphere editions I moved on to Installing ESXi.  I've done this tons of times.  However I've never used Auto Deploy.

Auto Deploy was new in vSphere 5 and if you skim read the blog posts or documentation headings then you might well presume it's just Automated Install.  A robot that once set up will dump a specified ESXi image on a server, then it's back to you.

Well it's much more than that.  After my first successful Auto-Deploy I checked the Console of the machine and thought, "ok so I need to set DHCP now".  Then I wondered how it had carved up the disk I'd assigned it.  Hang on a sec. It's only utilised a few Kb of the 5Gb I'd assigned it.  I then went to look at it's storage and saw this message.

<a href="http://chrisneale.files.wordpress.com/2014/05/autodep3.png"><img class="alignnone size-medium wp-image-137" src="http://chrisneale.files.wordpress.com/2014/05/autodep3.png?w=300" alt="autodep3" width="300" height="162" /></a>

Then it dawned on me (and I remembered a distant bit of documentation).  Auto Deploy loads ESXi into memory.  It doesn't do a traditional install.  Then words like stateless and stateful started popping back into my brain and I remembered that you can, depending on the powercli you define, and the host profile you apply, have ESXi built/loaded on the fly every time it boots.

Why?

Well think about it.  You get a standard image.  You can tell them all to have a consistent host profile.  It will automatically place your host in the folder you specify in vCenter.

All great, but a big shift from how people normally work and think.  There are still plenty of Service Managers obsessed with "which host is VM x running on?".

If you're not comfortable with it then fine.  Go for a stateful install and then you are using Auto-Deploy just like HP/Altiris RDP or Tivoli or SCCM etc. (but with much more VMware integration and flexibility).  The size of your environment also will dictate if this configuration is worth it.  It's not hugely time consuming but it does add a little bit of time to your deployment.  Not forgetting that whoever will be looking after the environment after you've gone will need to understand it!

The activities are:
<ol>
	<li>Install vCenter (either on your first host as a VM or on a physical)</li>
	<li>Install Auto-Deploy (either by starting it on the VCA or installing it on the vCenter server or another valid server)</li>
	<li>Configure your tftp &amp; dhcp server (very quick and easy if you're using the VCA)</li>
	<li>Install powercli somewhere</li>
	<li>Download the "Offline Bundle" zip of your chosen ESXi 5.x image</li>
	<li>Add the zip to the Auto-Deploy server using powercli</li>
	<li>Create an Auto-Deploy rule specifying the image to use etc. using powercli</li>
	<li>Assign that rule as the default rule</li>
	<li>PXE boot a new host (physical or a sneaky virtual with 2Gb min and cores)</li>
</ol>
That is where I got to.  However I missed out

6b.  Create a host profile for your new host(s) (this should include where in vCenter you want the host to be put)
7b.  Ensure the Auto-Deploy rule includes the host profile you want

This blog post will give you a great overview
<a href="http://blogs.vmware.com/vsphere/2012/01/understanding-the-auto-deploy-components.html">http://blogs.vmware.com/vsphere/2012/01/understanding-the-auto-deploy-components.html</a>

This KB article gives more in depth detail of Auto-Deploy with links and ref to the Admin Doc PDF/Page numbers where you can read more
<a href="http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&amp;cmd=displayKC&amp;externalId=2005131">http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&amp;cmd=displayKC&amp;externalId=2005131</a>

So here's to the Software Defined DataCenter (SDDC)
