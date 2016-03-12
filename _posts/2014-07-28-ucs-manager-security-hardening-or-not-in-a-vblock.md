---
layout: post
title: UCS Manager Security Hardening (or not) in a vBlock
date: 2014-07-28T18:00:00+00:00
tags:
- CIMC
- IPMI
- UCS
---
Another of the outputs of our recent penetration test was an issue with Weak Ciphers in both the IPMI service on C200 Rack servers and B220 M3 blades and the CIMC https server of the C200's

Ok so can we fix it?

Well the answer isn't a black/white yes or no.

We are running the above UCS servers in 2 vBlocks.  The first is a vBlock 100 the second is a vBlock 300.

To keep consistent with the VCE Certification Matrix we have to stick to approved versions of firmware, ESXi OSes etc.  This means that the fix isn't "Just go to Cisco's site and get the latest everything and install it".

&nbsp;

So we decided as we weren't using IPMI over Lan to disable that.  One the Rack servers this wasn't a problem.  Simply connect to the CIMC webpage and login.  Then go to Communications Services and untick IPMI over Lan.

Problem solved......but wait the Blades don't have the same style CIMC, it's managed and configured via service profiles in UCS Manager.  A quick search tells you that if you are running UCS 2.2 then choose to restrict IPMI access

<a href="http://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/sw/gui/config/guide/2-2/b_UCSM_GUI_Configuration_Guide_2_2/b_UCSM_GUI_Configuration_Guide_2_2_chapter_011110.html#concept_C6A0C24AE90D4AE6BC0D29FDC17DFECD">http://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/sw/gui/config/guide/2-2/b_UCSM_GUI_Configuration_Guide_2_2/b_UCSM_GUI_Configuration_Guide_2_2_chapter_011110.html#concept_C6A0C24AE90D4AE6BC0D29FDC17DFECD</a>

Great stuff, but wait I can't see those options in my GUI (or the equivalent commands in the CLI).................checks UCS version.................ahh erm 2.1.3b.................which is what the supported Matrix version is.

<a href="http://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/sw/gui/config/guide/2-1/b_UCSM_GUI_Configuration_Guide_2_1/b_UCSM_GUI_Configuration_Guide_2_1_chapter_011110.html#d184278e3464a1635">http://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/sw/gui/config/guide/2-1/b_UCSM_GUI_Configuration_Guide_2_1/b_UCSM_GUI_Configuration_Guide_2_1_chapter_011110.html#d184278e3464a1635</a>

So now I'm stuck on a version that has a vulnerability and no resolution available to me because I am bound by the supported config and, even though mine isn't the latest supported config, the latest doesn't support 2.2 yet either.

Fortunately we were able to mitigate the issue via other means due to the location behind firewalls of the particular vulnerability.

But this still means

<strong>"Caveat Emptor"  </strong>

It's great buying into a vendor who promises to do all the legwork in compatibility testing to provide you with matrices of the component versions which will work together........unless they are on top of their game.........they keep you off yours!
