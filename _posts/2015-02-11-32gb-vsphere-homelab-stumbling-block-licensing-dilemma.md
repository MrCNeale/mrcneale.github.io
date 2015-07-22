---
layout: post
title: 32Gb vSphere Homelab stumbling block: Licensing dilemma
date: 2015-02-11 00:57
author: chrisneale
comments: true
categories: [#vmware #homelab #win7 #win8 #win10 #licensing, Uncategorized]
---
I upgraded my pc to a beefy spec just before Christmas.  i7 4770s giving me 8 threads to throw at VMWorkstation and 16Gb of memory, along with my existing SSD.

A quick reinstall of windows and install of VMWorkstation and I had a great lab to revise for VCAP-DCA510 in a hurry.  However I had promised myself that I'd set aside £120 this month to max out my motherboard with another 16Gb.  There's another good reason to go that limit.  I didn't pass 510 and the 550 exam has a different setup including more hosts and VMs/SSO boxes etc.

So I bought the 16Gb and it sat ready to be installed whilst home life happened and stopped me opening up my case.  I did that tonight.  Duly inserted the new memory and booted up.

No errors (time to put the side cover back on now!)....But WAIT....Windows is only showing 16Gb of memory.  However CPU-z dutifully informed me there were 4 banks occcupied and 32gb there.

DOH!!!! I suddenly realised, but had never really considered.  That I was running Win7 Home Premium.  It had sufficed for the last 5 years and like most others I had been in no rush to jump to Windows 8.

A quick visit to MS confirmed my suspicions.

<a href="https://msdn.microsoft.com/en-gb/library/windows/desktop/aa366778(v=vs.85).aspx#physical_memory_limits_windows_7">https://msdn.microsoft.com/en-gb/library/windows/desktop/aa366778(v=vs.85).aspx#physical_memory_limits_windows_7</a>

I'll confess to doing a quick google to see if there was something as cheeky as a reg hack around this "oversight" it quickly became apparent there wasn't (Kernel hacking isn't something I really fancy).

So what were my options.
<ol>
	<li>Well the obvious one is Windows 7 Pro.  That is £129 now.  Not a fortune but a lot when you consider I bought all the hardware recently.</li>
	<li>Perhaps a spare MSDN license via work?  Well this wouldn't technically be in the spirit of the license as I do use my PC for other stuff, not just testing</li>
	<li>A surprising option was upgrade to Windows 8.1 (Standard/Core/Basic) as when MS released 8 they also increased the memory Limit to 128gb!
This option comes with a £99 price tag.  Hmm slightly easier to swallow but I don't really need any of the other features</li>
	<li>The final option is a very precarious one.  Windows 10 Tech Preview.
What!? I hear you cry.  Well as you will all have read this is coming free to Windows 7&amp;8 users soonish anyway (some sites say June, others say months after) so I may be jumping the gun a little bit but it would give me 32Gb in windows.....However.......My licensed version of VMworkstation is 10 which MAY but most likely not run on Windows 10.  So I would need an upgrade to Workstation11.
Cost? £109</li>
</ol>
I may test W10 TP on my HP Micrososerver and test if vmworkstation 10 runs on that...but it is frustrating that just to get around a single byte of code in the Win 7 Kernel which says 0x10 not 0x20 is going to set me back another £99 and an OS I'm not itching to jump to.....but ho hum such is progress!

&nbsp;

I'll update you on which route I decide

&nbsp;

&nbsp;
