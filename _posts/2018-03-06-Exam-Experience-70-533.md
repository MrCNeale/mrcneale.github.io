---
layout: post
title: Exam Experience - 70-533 Implementing Microsoft Azure Infrastructure Solutions - (Post Jan 2018 Updates)
excerpt: A different, but real-life, exam experience
comments: true
date: 2018-03-05T22:00:00+00:00
image:
  feature: 
tags: 
  - Powershell
  - Security
  - Scripting
  - Azure
  - Exam
  - Study
  - MCP
  - ARM
---
<img style="float: right;" src="https://acclaim-production-app.s3.amazonaws.com/images/53022c34-15ad-4f64-a50f-2b76798f2df0/Microsoft_Exam533.png">

<H1> An Updated Exam</H1>
I sat the 70-533 exam last Wednesday. It was the version after the Jan 2018 updates.  
That last bit is critical. Cloud moves fast and Microsoft published the exam 4 years ago. It was originally based around ASM, the white and blue GUI and a different set of PowerShell commands.  
It also had a number of other concepts which have been migrated over that time into Azure Resource Manager as a deployment method.  
Initially that had me concerned as I came to Azure 12 months ago fresh onto ARM. It was new at first but I didn't have any ASM baggage. 
I saw the exam still had lots of ASM content when I looked into it last summer.  But there have been TWO updates to the exam since then.
One in October 2017 and one in January 2018.  These removed old content such as ASM and most of the SQL PaaS objectives.  

<H1>Areas I focussed on</H1>
Not having to learn the old ASM stuff I was never going to use, and I had used ARM for a year, so that was good.  
I hadn't done very much on the WebApps or on the very latest additions, like Azure Container Services, so had to brush up on:
* Non ARM Powershell commands as some of those are still used as not all features/services are fully ARM based yet
* WebApps - A big topic, but I work mostly in the IaaS/PaaS space but not on developing WebApps, so had to practice deploying them, staging slots, load balancing and traffic management to them
* Diagnostics - This was OK for me as I had done a lot of work with OMS.  OMS is going away as a product, but most of it's monitoring features are appearing in Azure Portal.
* SQL/PaaS - Whilst this has mostly been scrubbed from the exam objectives I would still recommend having an understanding of the basic service offerings and levels for SQL PaaS
* The new 2018 updates such as Containers (ACS) and Key Storage (KeyVault) should be on your list too.

A full change tracked version of the Exam Objectives is available here:  
<http://download.microsoft.com/download/8/4/8/848DD46A-05F2-4021-A118-036FC06647C5/533_OD_Changes.pdf>  
Make sure you read this and check whilst you're studying for any updates that might get made.

<H1> Study Resources </H1>
The main focus for my study was Scott Duffy's Udemy Course
