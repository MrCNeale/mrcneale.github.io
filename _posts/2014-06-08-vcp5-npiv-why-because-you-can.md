---
layout: post
title: VCP5 - NPIV, why? Because you can!
tags:
- ESXI
- FC
- HBA
- NPIV
- storage
- VCP5
date: 2014-06-08T18:00:00+00:00
comments: true
---
I've been revising for a good few weeks now.

Setting up my homelab, being wowed by autodeploy installations and practising other things I haven't had much exposure to in my day to day vSphere 5 usage.  However I still had to cover those niggly bits that come under the heading:

"Never used this, probably never will, don't understand it!"

NPIV was one of these.  You'll all have seen the questions if you're preparing for the exam, or more often wonder why it's there squirreled away on the <strong>options </strong>tab of a VMs configuration page.

Step 1 understand it - OK. That's easily achieved.  You read the VMware Storage Admin guide.  It's a small section of it and explains it pretty well.  In a nutshell it allows you to assign WWN pairs to your VM as if it were a physical.  To do this your VM needs to be using an RDM presented via a FC HBA.  Ok got it.

At this point I was happy.  I knew what I needed for the exam.  What it was and where to click in vCenter to enable/disable/configure it.  But wait, a colleague at work (when I was trying to show off this new knowledge) asked the best question yet.  WHY?

Now I used to be more of a WHY guy.  I blame being a team leader for 18 months as there are very valid situations when you stop asking WHY as it's outside your sphere of influence and someone may have more information than you so in this instance JFDI is applicable :-)

But he was right goddamnit!  Why would you want to give your VM it's on WWPN?  What was the point.  I did what everyone does first I asked Google.  It led me to a very good technical article on how to set it up and what to expect, by the StorageFather of vSphere Cormac Hogan.  However it still didn't answer my question.  Is there an obvious/common usage case for this option?

The odd blog suggests that it does allow you more granular monitoring of the storage as you can see the usage of a LUN by a particular RDM on a VM.  Whilst this is true I've not seen it used on any of our customer's virtual estates with HDS/NetApp/EMC

So I used social media to politely ask the man himself.  His answer?

<strong>"Not really Chris. Rarely comes up in conversation these days"</strong>

So there we have it. It's like Print Services for Unix in Windows.  It's possible, but no-one uses it!!!! :-)

<a href="https://chrisneale.files.wordpress.com/2014/06/npiv.png"><img class="alignnone size-medium wp-image-164" src="http://chrisneale.files.wordpress.com/2014/06/npiv.png?w=300" alt="npiv" width="300" height="267" /></a>
