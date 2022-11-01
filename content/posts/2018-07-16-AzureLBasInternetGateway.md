---
comments: true
date: "2018-07-16T22:00:00Z"
excerpt: Reverse Bel-Air on the LB
image:
  feature: https://azure.microsoft.com/svghandler/load-balancer/?width=600&height=315
tags:
- Powershell
- Security
- Scripting
- Azure
- Networking
- LoadBalancer
title: Using the Azure Load Balancer in reverse to funnel all outgoing traffic via
  a single IP
---
<img style="float: top;" src="https://azure.microsoft.com/svghandler/load-balancer/?width=600&height=315">

<H2> An Odd Request</H2>
This implementation started from a SAP team who said they wanted to replicate, in Azure, the AWS Internet Gateway functionality. In particular the option for all outbound traffic from VMs to route via a single outbound IP.  

As you may know even if your default Azure VM does not have a Public IP assigned to it, it can still access the internet for things like windows updates etc. However when it does so the Public IP it appears to have come from is ephemeral and changes.  Similarly each VM will have different IPs. For a reason known to the SAP team, they needed all VMs to appear to be coming from the same IP.  

Obvious options are NVAs but they are quite big, cumbersome things to deploy and configure quickly and easily. They would also required ongoing changes, such as updating rules/IPs if a new VM is added.  

That's when we found that the Azure Load Balancer can be used to "Balance" outgoing connections also.
[The Microsoft Docs detail this behaviour here](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-outbound-connections#lb)  
By creating an LB with a public IP, then creating a dummy health probe to a non existent service, then connecting that, via a rule to a BackEnd Pool containing your VMs, any traffic that goes to the internet from those VMs will exit via the Public IP of the LB.

<H2>Implementation Steps</H2>
1. Create a Public IP (Or create one during load balancer creation)
2. Create a Load Balancer and associate with the Public IP from step 1 <img src="/images/LB1and2.png" align="bottom">
3. Create a Health Probe, select a non used port on the VMs, say 65534 <img src="/images/LB3.png" align="bottom">
4. Create an Availability Set <img src="/images/LB4.png" align="bottom">
5. Create a BackEndPool with the availability set as the member <img src="/images/LB5.png" align="bottom">
6. Create a LB Rule selecting your Public IP, Health Prob and BackEnd Pool you just created <img src="/images/LB6.png" align="bottom">

Here's the LB IP  
<img src="/images/LB-IP.png" align="bottom">  
Here's the VM nic config, showing only a private IP  
<img src="/images/vmnicconf.png" align="bottom">  
Then we find out what the internet thinks our IP is with this command:  
<img src="/images/ps-command.png" align="bottom">  
And we see it matches the LB  
<img src="/images/wmip.png" align="bottom">  
  
Now any VMs deployed to the Availability Set will automatically have their outbound traffic via the Public IP  
If you already have VMs, or require more granular control over the IPs/Networks/VMs that are included, then at step #2 deploy a STANDARD SKU Load Balancer and do not deploy the availability set in step #4, as this allows more control over BackEnd Pool selection of members, but needs manual updating of the members.
