---
layout: post
title: VCIX-NV Exam Experience
excerpt: 
tags: 
  - VMware
  - Exam
  - Certification
  - Study
  - VCIX
date: 2016-11-21T22:00:00+00:00
comments: true
---
<IMG src=/public/VCIX-NV.png align=right>
Last month I sat and passed the VCIX-NV exam.  Driven, like a lot of others, by the impending retirement of the exam that keeps getting extended...more on that later.

This is a post in two parts, first the exam experience, second a few pondering on VMware Education's current trajectory.

The Day Itself
===============
Here in the UK there are only 3, I believe, VCAP accredited test centres.  I hopped on a train across to Leeds, that ended up being delayed and I only had time for a coffee and sandiwch before having to get started.  Moral here is give yourself plenty of time to get to your chosen test centre and if possible/affordable, go the night before.  It seems silly to waste the £300+ exam fee on being late?!

The Exam Environment
====================
The room was ok, very standard fayre for VCP/VCAPs.  The connection to the remote lab was ok and the infamous LAG wasn't overly noticeable compared to a Lab in the US I do a lot of my day job in.....so no excuse to blame it on there! DOH!  
There are quite a few posts detailing the lab layout and Josh Andrews gives a good precis of it at
[http://sostechblog.com/2016/06/16/current-vcix6-nvvcix-nv-exam-environment/] ([http://sostechblog.com/2016/06/16/current-vcix6-nvvcix-nv-exam-environment/] "VCIX Lab Layout") now this will be of very little use to anyone now as the exam is set to retire at the end of the month and the new VCAP6-NV Deploy has been released.
The exam was ESXi5.5 and NSX6.0.2.  So very old stuff and needed some unlearning.

The Prep
========
I have worked with NSX for about 18months now in development, Customer Proof of Concepts and with Customers too.  That was one of the biggest advantages as customers were almost working through the exam blueprint at one point :-)  
"What about a Load Balancer with Round-Robin and SSL offload? Can you show us that?"  
Ok I was lucky.  I was getting paid to revise and the customer was getting the solution they wanted.  Everyone was #winning.  
However there were still the corner cases or bits I never touched.  OSPF/IS-IS/BGP were all alien to me.  I'm from a vSphere background and whilst I understand my fair share of networking, these very traditional networking areas such as routing advertisement protocols were new to me and very similar to each other, but with slight differences.  
This is where *Iwan Hoogendoorn* steps in to help everyone!  He sat the VCIX-NV some time ago but was generous enough to work through every Blueprint item and make a video blog of it, then upload them all to YouTube.  
[https://www.youtube.com/channel/UCcE9U_Gf0MhGpsDIhneMyDg](https://www.youtube.com/channel/UCcE9U_Gf0MhGpsDIhneMyDg)
![Examples](/public/chrome_2016-11-21_23-10-09.png)
It's essential watching for anyone pondering the exam...even the new 6.2 version I would argue.  Ok you will need to update a few of the boxes but if you copied along with the video using 6.2 I'm sure you would be able to figure out the differences in drop-downs etc.

The Questions
=============
Now *obviously* due to the NDA I won't be discussing the content.  However what I would say is this.
The Blueprint was great and up to VMware's standards as usual.   
The actual tasks in the exam weren't.  What I mean by this is that the level of ambiguity and contradiction didn't add to the exam experience or the automated marking afterwards.  
By specifying in task A that you must not do xyz, then stating in task C that you need to ensure abc.....but the only way to ensure abc was to do xyz....but hang on you've been told not to do that??!  
Now I'm used to plenty of ambiguity in my job.  It's my favourite [Lominger Competency](http://www.ptc.com/content/production_content_server/cninv000000000014107/content.pdf).  However with a client or a set of requirements you can ask questions to remove that ambiguity.  In a 3hr lab exam to test if you can resolve issues or deploy to instructions...there's no debate or clarification possible.  As such I'd argue this level of ambiguity shouldn't be in the eam.
Similarly as almost everyone says about VCAPs/VCIX's  There isn't enough time.  
It's worth noting that like previous VCAP's the questions were clear if a particular method was required, e.g. gui/esxcli/powercli/api 

The Time
========
Having sat the exam last month.  I studied lots before hand and there wasn't a question in the exam where I thought "that's unfair, it wasn't on the blueprint.  I also think I could answer all the questions. BUT..........I also think that, now I know what all the questions are, I couldn't go back in to the exam tomorrow and finish them all in the time given!  
I was efficient, I noted down things to confirm/review/return to.  I did admittedly waste 10-15 minutes down a rabbit hole in the GUI.  However I don't think it's possible to complete all the tasks in the time.  That also feels a bit unfair, but hey maybe it's changed.

The Result
==========
As has become the norm now within 3-4hrs I got the e-mail through telling me I had passed.  Great relief, not least at not having to wrangle with trains to Leeds again.  
It has to be hailed as a great win for automation that a complex multi-task exam can be marked via powercli script and turn around an answer so quickly.


The Future of VMware Certification
==================================
There are some positives here.  Myles Gray recently sat the VCAP6-NV Deploy and reviewed his experience at 
(https://blah.cloud/personal/vcix6-nv-exam-experience/)
Also VMware now give you a preview of the Lab Experience in a PDF at (https://mylearn.vmware.com/lcms/web/portals/certification/VMware%20Certification%20Platform%20Interface.pdf#src=vmw_so_vex_cneal_850)
Notice anything familiar?
![vcap-hol-view](/public/chrome_2016-11-21_23-01-55.png)
That's right it's HOL style.  Something you should be very familiar with as you should be hammering the HOLs to learn NSX and anything else ;-)
That's positive.  
The negatives are the ongoing debacle that VCIX-NV will/won't/will/won't/will/won't/will/won't get upgraded to VCIX6-NV.  It was version 6 of NSX so it should, and there was no Design exam available either to do a full VCAP6-NV...  They really need to sort their roadmap out, it changes too often and then lacks support.
Also a very random point to add is that the entry level actual exam, the VCA are the most bizarre exams I have ever encountered.  A muddled mix of sales/marketing/buzzwords/incorrect-technical terms and more....I got some vouchers recently to do some.  Whilst I am grateful for that I cannot see why someone would pay to do those exams.  They don't pre-qualify for VCP, they aren't very beneficial and the content falls well below the par of VCP/VCAP/VCIX questions.  I think these should be scrapped or revamped.  I've also heard the Certifcition team has had a shake up/re-org. Let's hope that sets things back on the good track it used to be on.

Now for a rest..........
