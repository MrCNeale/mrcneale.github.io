---
layout: post
title: VCAP-DCA510: Failing to pass doesn't mean failing to learn
date: 2015-01-22 23:10
author: chrisneale
comments: true
categories: [Uncategorized]
---
Last Thursday I got the early train to Leeds stationed myself in a coffee shop and proceeded to stuff coffee and pastries into my mouth and last minute vSphere knowledge into my brain.

I had obtained a voucher to book the exam last year.  However I'd been seconded to a "Business Transformation" project.  Whilst this was quite interesting it did cause my technical skills to atrophy.  To cut a long story short I rescheduled the exam a couple of times.  However when a friend retweeted just before Christmas that 510 was going to be retired that was the end of that.  I had 3-4 weeks to revise/read/cram and lab.  Unfortunately Christmas isn't the season conducive to revision.

So, having gotten my excuses in early, I proceeded to study.  I used the following resources which many others will have referred to and used before me:
<ul>
	<li>The Blueprint (no brainer, but I will come back to this one)</li>
	<li>The Unofficial VCAP Study Guide (as recommended by everyone and anyone)</li>
	<li>The Official Cert guide (Not free but £25 well spent in my opinion, more again on this later)</li>
	<li>PluralSight "Scale  &amp; Optimise" online course (I got access to this the week before the exam)</li>
	<li>Chris Wahl's VCAP-DCA Study Sheet</li>
	<li>Josh Andrews' Test-Track Lab</li>
</ul>
In real terms I studied every evening for 2 weeks.  I spent a lot of time reading, with occasional visits to my lab PC.  That was sufficient time to absorb and understand all the topics on the blueprint.

There wasn't anything on the actual exam that surprised, shocked or surprised me.  I'd read about it or practiced it all at least once.

Why didn't I pass then? Well that's simple.  Not enough lab time.

It sounds obvious but it really really can't be overstated.  There were 26 tasks on the exam and 3.5hrs to do them in.  That's less than 7 minutes per task.  Don't forget it takes a few minutes to familiarise yourself with the exam environment and hostnames/ips.  So think 6 minutes per task.

Jason Nash in the Optimise and Scale (I'm pretty sure it was) said "Think of it as a customer asking you to do some work but they can only pay for 3.5hrs"

I would put it slightly differently, and this is where the blueprint comes in.

Think of the Blueprint (and definitely Chris Wahl's Study Sheet/Checklist) as a list of things you will need to do for the customer.  It's their requirements doc.  It's not specific.  The IPs or hosts or networks or VMs details will be given to you in the task "on the day".  But they have an outage window of 3.5 hrs with a hard stop.  You need to have done what they need in that time as users will be let back on the environment afterwards.

Given that premise then you need to be able to do each and every one of the Blueprint/Checklist tasks in 5 minutes.  Not think you can.  Not do it once.  I mean you need to drill yourself on it.
If the task is (and I'll quote blueprint/checklist items here) change a pathing policy then all you should need it the LUN/Path/Disk detail and which policy.  Bosh.
If the task is migrate all of a host's, or specific standard, switches to a DVSwitch then 5 minutes later. Bosh.
Set up FT? 5 minutes Bosh!

You get the idea.  If you have to think too long about it, you will waste too much time
(I do think it's a bit harsh they take your watch off you when you go into the test room!)

A mini resource review is:
<ul>
	<li>The many VCAP study blogs out there are great.</li>
	<li>I would recommend the Official Cert Guide if £25 is in your budget.  It's got a good structure to it and worth it if only for the practice scenarios it gives you. (Just make sure you get the additional ones from the online website using the code in the book)</li>
	<li>I got my access to Pluralsight late.  I also got it free as a 2014 vExpert.  but I would really consider spending $29 for 1 month's access.  Again it won't break the bank and it's an easy/convenient way to consume information</li>
	<li>Chris Wah's study sheet/checklist should form the basis of your practice tasks that you drill yourself on.  If you can't do something on it.  Read up on how then practice it in your lab 100 times until you can do it in 3 minutes</li>
	<li>Finally a massive shout out to Josh Andrews.  His test-track lab which he generously allows remote access to is a great resource that I can't recommend enough.  It really gives you a feel for the exam and if you're ready to do the kinds of tasks that you might encounter.</li>
</ul>
What next?
<ol>
	<li>Request authorisation for VCAP-DCA550 - DONE ✓</li>
	<li>Get a voucher/funding from work to book 550</li>
	<li>Forget all about AutoDeploy and learn about SSO, vSphere replication, Orchestrator etc*</li>
	<li>Revise the bits that are common to 510 and 550 that were highlighted on my score report</li>
	<li>Drill in the lab 100s of times until I can perform each task in the time it takes an egg to boil.</li>
</ol>
*Josh Andrew did a good post about the Blueprint differences here <a href="http://sostechblog.com/2014/04/07/vcap5-dca-update-for-vsphere-5-5/">http://sostechblog.com/2014/04/07/vcap5-dca-update-for-vsphere-5-5/</a>
