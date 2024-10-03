---
layout: post
title: Powershell to retreive Alerts which use a specific Action Group
excerpt: joining the dots from alerts to action groups
comments: true
date: 2021-05-14T22:00:00+00:00
image:
  feature: https://docs.microsoft.com/en-gb/azure/virtual-machines/linux/media/index/logo_powershell.svg
tags: 
  - Powershell
  - Azure
  - Alerts
  - Monitor
  - Logs
  - ActionGroup
---
<img src="https://docs.microsoft.com/en-gb/azure/virtual-machines/linux/media/index/logo_powershell.svg" height="20%" width="20%">

<H1>"Is this Action Group used by any alerts?"</H1>
If you want to find out if an Action Group is used by any alerts (metric or Log) how do you do it?

Two simple queries help here:

To check if any Metric alerts point to your Action Group, run
```
Get-AzMetricAlertRuleV2 | ?{$_.actions.ActionGroupId -like 'YourActionGroupName'}
```

To check if any Log Alerts (Known as Scheduled Query alerts since classic was retired)
run
```
Get-AzScheduledQueryRule | ?{$_.action.aznsaction.actiongroup -like ''YourActionGroupName'}
```

Now you know if the Action Group is being used, and can be safely removed.
