---
layout: post
title: Using the Azure Load Balancer in reverse to funnel all outgoing traffic via a single IP
excerpt: Reverse Bel-Air on the LB
comments: true
date: 2018-07-16T22:00:00+00:00
image:
  feature: https://azure.microsoft.com/svghandler/load-balancer/?width=600&height=315
tags: 
  - Powershell
  - Security
  - Scripting
  - Azure
  - Networking
  - LoadBalancer
---
<img style="float: right;" src="https://azure.microsoft.com/svghandler/load-balancer/?width=600&height=315">

<H2> An Odd Request</H2>
This implementation started from a SAP team who said they wanted to replicate, in Azure, the AWS Internet Gateway functionality. In particular the option for all outbound traffic from VMs to route via a single outbound IP.  

As you may know even if your default Azure VM does not have a Public IP assigned to it, it can still access the internet for things like windows updates etc. However when it does so the Public IP it appears to have come from is ephemeral and changes.  Similarly each VM will have different IPs. For a reason known to the SAP team, they needed all VMs to appear to be coming from the same IP.  

Obvious options are NVAs but they are quite big, cumbersome things to deploy and configure quickly and easily. They would also required ongoing changes, such as updating rules/IPs if a new VM is added.  

That's when we found that the Azure Load Balancer can be used to "Balance" outgoing connections also.
[The Microsoft Docs deail this behaviour here](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-outbound-connections#lb  
By creating an LB with a public IP, then creating a dummy health probe to a non existent service, then connecting that, via a rule to a BackEnd Pool containing your VMs, any traffic that goes to the internet from those VMs will exit via the Public IP of the LB.

<H2>Implementation Steps</H2>
1. Create a Public IP
2. Create a Load Balancer and associate with the Public IP from step 1
3. Create a Health Probe, select a non used port on the VMs, say 65535
4. Create an Availability Set
5. Create a BackEndPool with the availability set as the member 
6. Create a LB Rule selecting your Public IP, Health Prob and BackEnd Pool you just created

Now any VMs deployed to the Availability Set will automatically have their outbound traffic via the Public IP
If you already have VMs, or require more granular control over the IPs/Networks/VMs that are included, then at step #2 deploy a STANDARD SKU Load Balancer, as this allows more control over BackEnd Pool selection of members.