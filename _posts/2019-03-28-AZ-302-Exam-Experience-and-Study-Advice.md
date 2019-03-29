---
layout: post
title: Exam Experience - AZ-302 Microsoft Azure Solutions Architect Certification Transition
excerpt: A new improved exam - Now with Labs!
comments: true
date: 2019-03-28T22:00:00+00:00
image:
  feature: azure-solutions-architect-expert-small.png
tags: 
  - Powershell
  - Security
  - Scripting
  - Azure
  - Exam
  - Study
  - MCSE
  - ARM
  - PaaS
---
<img style="float:right;" src="/public/azure-solutions-architect-expert-small.png">

<H2> NOW WITH LABS! - An Updated Exam and Exam Format</H2>
I sat the AZ-302 exam on Tuesday.  It has been updated since the Beta considerably.  Many who took the beta felt that it wasn't Architect focussed but more developer focussed.  Now whilst in this new brave cloud world you have to be part Admin/Pre-Sales/Architect and Developer, writing C# or node.js code in an Architect Exam didn't seem fair to me either.

It's also due to retire at the end of June, so if you're reading this now...GET STUDYING! :-)


<BR>
<TABLE><TR><TD>
<img style="float:left;margin: 0px 20px" src="/public/clock.jpg" width="25%" height="25%" >    
<H2> Format and Exam Day</H2>
As had already been reported, the coding sections of the exam have either been removed or dialled back considerably.  I am not a application coder.  I write Powershell scripts and relatively complex ARM templates on a day to day basis for the last 2 years or so.  Based on that experience there wasn't any code in the exam that I didn't think was unfair or not-relevant to a day job as an Architect. 
<BR><BR>
  There were also 2 quite large labs involved so time management is <B>critical</B> in this exam.  It may not be the same for everyone but you should ensure you make note of the time and questions at the start of the exam, as they could be split across the labs.
The labs are auto-graded, there was no wait to get my score, it was just as quick as previous question-only exams.

</TD></TR></TABLE>
<H2> Preparation/Study Resources</H2>
As usual I used Scott Duffy's Udemy Course as my main prep. [https://www.udemy.com/az302-azure/](https://www.udemy.com/az302-azure/)  I thought it was very content light compared to his usual courses, which was dissapointing.  There were some good examples in the "assignment" sections, but I think there could have been more live-video examples.  I'm not affiliated with Scott or Udemy and bought the course myself for £10.99 so it's hard to complain as Udemy is a great platform you can watch on PC/iPad/Mobile really easily and offline.  
  
As usual the IaaS sections and IAM/AD/Security areas, which I have most background in were quite simple to absorb.  Where I had to pay much more attention is around the PaaS/Serverless application space.  So Web Apps, Functions, Batch, Event Grids etc.  
This may be different for you, but where I am currently we're still helping a lot of customers move from on-prem into cloud, so they haven't expanded or transformed to start using PaaS services heavily.  

I advise finding yourself a demo application, like the Java Spring PetClinic and using that, or even writing something trivial that let's you upload a file via a webpage to storage blob, and let that be your "customer project", to test things out with.  

Another non-techie area to the exam is the familiar format (for anyone who did 70-535 and other exams) of Case Study reviews.  These test if you can assess a potential solution to a customer's requirements and validate if it will meet them or not.
  
<H2>Objectives and Changes</H2>
The list of objectives and split can be found here  
[https://www.microsoft.com/en-us/learning/exam-az-302.aspx](https://www.microsoft.com/en-us/learning/exam-az-302.aspx)
Unlike previous 70-x exams there doesn't appear to be a changes pdf, then again  

<H2>The Result</H2>
I got 807/1000, where 700 is the pass mark.  I was pleased with the pass but the exam made me realise I really need to up my PaaS and coding game. So I'll be writing a series of posts soon to leverage things like Function Apps, Logic Apps and possibly Event grid.  Also some very basic coding to make calls to Azure services.  

<H2>Summary</H2>
The new exam format with lab sections were a real improvement good and I think important to differentiate hands on experience from simple book memorisation, especially SLAs or SKUs or other things that really shouldn't have to be in your head when they often change or there is one per Azure service, of which there are hundreds.
What I'll never get is why Pearson for Microsoft/VMware keeps the screens at 800x600 or whatever awful res they are.  That has been bad enough for non text-only multi-choice questions in the past, such as drag and drops or diagrams.  Limiting the same resolution for the Azure portal meant lots and lots of annoying scrolling.
MS/Pearson/Whoever, fix it!  

Where next? Ultimately I will go for AZ-400 Microsoft Azure DevOPs Solutions, but to get there I need to transition my 70-533 to AZ-102.
So next stop? AZ-102...watch for blog posts on studying for that.  

**For a great resource to show the new AZ certification paths, checkout Ian Fieldings Tube Map of Azure Certs**  
[https://iainfielding.com/azure-certifications/](https://iainfielding.com/azure-certifications/)
<img src="https://iainfielding.com/wp-content/uploads/2019/01/AzureCert_TubeMap_JAN2019-1.png">
