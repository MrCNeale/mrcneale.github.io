---
comments: true
date: "2015-07-06T00:00:00Z"
tags:
- homelab
- study
- vcp
- vmworkstation
- windows10
title: Upgrading your HomeLab to Windows 10 + VMware Workstation ??
---
I run my home lab on a PC running Windows 8.1 and VMware Workstation 10.  The PC has an i7 with HT, 32GB RAM and an SSD.

I had to upgrade from Windows 7 as the Home Premium Edition only supported 16GB of RAM, which was irritating.  I knew Windows 10 was coming but it's End of July release date hadn't been announced.  So when it popped up offering me my "free upgrade" I accepted and am now dutifully waiting a couple of weeks.

But what about VMware workstation?  Like quite a few home-labbers I am sure they also got their license by passing their VCP.  It's no mean feat and after investing time, effort and £2000+ on an ICM course it's very nice to get a copy of VMware Workstation when you pass.  After my VCP4 I didn't get around to using Workstation 9 much at all.  But after VCP 5 I have been using Workstation 10 quite a bit to build out a lab for VCAP-DCA and subsequently NSX/VCP-NV revision.

So....will Workstation 10 run on Windows 10?

Well the official support doc here is tight lipped on Windows 10 so far <a href="http://kb.vmware.com/selfservice/search.do?cmd=displayKC&amp;docType=kc&amp;docTypeID=DT_KB_1_1&amp;externalId=2088579&amp;src=vmw_so_vex_cneal_850">http://kb.vmware.com/selfservice/search.do?cmd=displayKC&amp;docType=kc&amp;docTypeID=DT_KB_1_1&amp;externalId=2088579&amp;src=vmw_so_vex_cneal_850</a>

It doesn't list it as a host OS yet.  However after reaching out on Twitter <b>Wil van Antwerpen </b>replied with a link<b>
<a href="https://communities.vmware.com/message/2517232#2517232#src=vmw_so_vex_cneal_850">https://communities.vmware.com/message/2517232#2517232#src=vmw_so_vex_cneal_850
</a></b>to his communities post pointing out that bug fixes to Workstation 11 had been made to smooth operation with WS11 running on Windows 10.  Those same fixes didn't appear in recent updates to Workstation 10.

That's fine.  I understand things move with the times but now Windows 10 "Free Upgrade" was looking like it will cost me £105 to upgrade to Workstation 11.  So I thought I'd try something crazy.

On my Windows 8.1 PC I created a VM inside Workstation 10.
On that VM I installed Windows 10.
On that VM I then installed Workstation 10 (which did warn me against running WS on a virtual machine)
I then created a Debian 64bit VM with 2GB memory and it ran fine.

So, whilst it likely to be wholly unsupported and any problems that occur would come with the fix "Upgrade to Workstation 11".

It is possible to run VMware Workstation 10 on Windows 10.....!

<em><strong>(NOTE: It is necessary to run Workstation as administrator or you will get an error about writing to a user folder...possibly something that can be changed with UAC or a shortcut pre-set to run as admin)</strong></em>

<a href="https://chrisneale.files.wordpress.com/2015/07/ws10inaw10vm.png"><img class="alignnone size-full wp-image-256" src="https://chrisneale.files.wordpress.com/2015/07/ws10inaw10vm.png" alt="ws10inaw10vm" width="660" height="490" /></a>
