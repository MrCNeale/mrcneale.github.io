---
layout: post
title: OMS Agent Syslogs Not Collecting
excerpt: Changes in the Linux OMS agent highlight missed Syslog Configuration (RTFM or RTFRN)
comments: true
date: 2018-10-22T10:00:00+00:00
image:
  feature: /public/rtfm.png
tags: 
  - Azure
  - ARM Template
  - Template
  - Automation
  - Monitoring
  - Log Analytics
---

<img style="float: top;" src="https://msdnshared.blob.core.windows.net/media/2016/11/OMS-Log-Analytics-e1479220299227.png">

We use Log Analytics to collect logs so that we can query the pooled logs to generate alerts.  A common scenario. 
We had an ARM template that had worked for a long time, but when we had an issue, the first thing Microsoft said was "Why are you using such an old version?"
Ok, we'd missed the agent updates on this particular template for Linux, it's always best to be up to date.  So two things happened.

1 Setting Linux OMS agents to always deploy the latest version
Microsoft documentation can be hit and miss.  Sometimes it's brilliant, other times it seems to miss the obvious.  This is one such case.
Here is the Windows Docs page for the OMS/LogAnalytics agent ARM template syntax:  
[https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/oms-windows](https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/oms-windows)
It shows a property named *AutoUpgradeMinorVersion* this little gem always selects the latest 1.x version to install whenever you deploy a template...It doesn't upgrade after install...that's a different issue.
Here is the Linux Docs page:  
[https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/oms-linux](https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/oms-linux)
But this doesn't show the property!
  
Ok so we got over that.  New installs chugged along and we were getting OMS/LA agent 1.8 instead of 1.3.  GREAT!
Except we hadn't checked the release notes to see what had changed.
When our automated testing ran, an alert that previously worked no longer fired when the condition it was supposed to detect occured.
This was odd. After looking at the alert, it was based on the contents of Syslog.  Looking at  the Log Anlaytics Workspace Logs area, we could see that no Syslog logs were being collected. Even more strange. But wait 1 VM was populating Syslog in OMS/LA.  How was it doing that?

Have you guessed yet?
Here's a screenshot of our Workspace/Advanced Settings/Data/Syslog configuration to help. <img src="/public/emptylawsyslog.png">
Spot the deliberate mistake? Yep, syslog wasn't set to be collected.  
Hang on a minute though, we hadn't changed our workspace config. I double checked the code to deploy our workspaces.  It never enabled syslog collection.  So how did this *EVER* work?  
<img style="float:top;" src="/public/confused.gif">  
Simple...it was a "bug".
Checkout the release notes for a prior release of OMS agent:  
[https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.3-174](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.3-174)
Bugfix = Disable Syslog data collection by default  
So until v1.4.3 all Linux agents pushed syslog, whether you asked for it or not!  
    
A simple update to the workspace code to make sure Syslog collection was enabled with correct facilities and all working.

Lesson for today.  Always Read The Flippin Release Notes (RTFRN)
<img src="/public/rtfm.png">
