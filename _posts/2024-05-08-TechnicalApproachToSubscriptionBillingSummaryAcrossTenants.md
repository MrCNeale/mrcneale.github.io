---
layout: post
title: How to Use Powershell to create a simple Cost Summary By Subscription for internal reporting 
excerpt: Powershell billing commands for internal summaries
comments: true
date: 2024-05-08T09:00:00+00:00
image:
  feature: cost.jpg
tags: 
  - Cost
  - Billing
  - Code
  - Automation
  - Time saving
---

<img src="/public/cost.jpg" alt="copilot" width="100" height="100"/>

# Fulfilling a need for a brief Subscription Monthly Cost report acost multiple subs in multiple tenants

Azure cost management and analysis in the portal is great.  Summaries, breakdowns, forecasts and budgets.  They all have their uses, but most are focussed at the individual subscription level....some do work at the Management Group level.  But one thing was missing there, the ability to "subscribe" to get a nice summary of multiple subscriptions total cost.
Export to a storage account? That only works to a storage account in the same tenant.  
Yes there are other solutions, Power Bi and 3rd party tools but those would be sledgehammers in cracking this relatively simple nut.  

So what then?  Go with what you know. In my case that's Powershell.
I set about a familiar pattern of:  

1. Retreiving the available list of subscriptions I have access to
2. Initialise an array and clear the error variable history
3. Retrieve the previous billing period, by returning the top two results, ordering them by name (as billing periods are in the format YYYYMMDD) and trim to the first in the list
4. Now loop over each subscription in the list changing context
5. Query the usage for the prior billing period and summing it up
6. Add that to an array
7. Write out the array when the last sub has been processed

Most of the above is standard Powershell and Az Cmdlets.  
Finding the billing periods in your subsccription required the Get-AzBillingPeriod command
[Get-AzBillingPeriod](https://learn.microsoft.com/en-us/powershell/module/az.billing/get-azbillingperiod?view=azps-11.6.0)  

Finding the total cost for the subscription resources requires a little bit of maths, nothing to arduous.  
The Get-AzConsumptionUsageDetail command will return all the costs incurred during the billing period, line by line.
[Get-AzConsumptionUsageDetail](https://learn.microsoft.com/en-us/powershell/module/az.billing/get-azconsumptionusagedetail?view=azps-11.6.0)  
But the column titled _"PreTaxCost"_ can be used along with Powershell's Measure-Object command to sum up that column.
[Measure-Object](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/measure-object?view=powershell-7.4)  

Add in a smattering of error checking to ensure any failures to change context or read billing data are trapped and reported along with the error summary and we're almost there.  
Round up the cost totals to the nearest whole number and add the result to the Powershell Array and we're there.

I will be adding this code to a function app which has access to a keyvault storing Service Principal credentials for the tenants wihch manage the subscriptions I need to retrieve costs from.
That will just be another loop encapsulating this, first reading in the credentials and then looping around them logging in to each tenant one by one to produce a consolidated report.  

Keep an eye out for that enhancement in future posts!

```powershell
#Read in list of subscriptions visible and initialise array of costs
$error.Clear()
$subsinscope = get-azsubscription
$subcosts = @()

#Loop over subs attempting to retrieve cost for last billing period and report progress and any errors
foreach ($sub in $subsinscope) {
    Write-Host "Processing Subscription " $sub.Name -NoNewLine
    try {
        $out = set-azcontext -subscriptionid $sub.SubscriptionId -ErrorAction Stop
    }
    catch {
        write-host "error setting context to subscription " $sub.name
    }
    try {
        #Retrieve previousBillingPeriod
        $previousBillingPeriod = Get-AzBillingPeriod -MaxCount 2 -ErrorAction Stop| Sort-Object Name | Select-Object -First 1
        $cost = ((Get-AzConsumptionUsageDetail -BillingPeriodName $previousBillingPeriod.Name -ErrorAction Stop | Measure-Object -Property PretaxCost -Sum).Sum)
    }
    catch {
        write-host " - Error retrieving cost for subscription" $sub.Name "likely due to lack of permissions. Actual error was" (Get-Error -Newest 1).exception.message
    }
    if ($cost) {
        $cost = [math]::Round($cost)
    }else {
        $cost = $null
    }
    $subcosts += [PSCustomObject] @{
        SubscriptionName = ($sub.Name)
        Cost             = $cost
    }
    Write-Host " - Processed"
}
#Write out table of subscriptions and last month's cost
$subcosts
```
