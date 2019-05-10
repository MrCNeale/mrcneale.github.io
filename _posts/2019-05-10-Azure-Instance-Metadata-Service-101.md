---
layout: post
title: Exam Experience - AZ-302 Microsoft Azure Solutions Architect Certification Transition
excerpt: A new improved exam - Now with Labs!
comments: true
date: 2019-05-10T22:00:00+00:00
image:
  feature: metadata.png
tags: 
  - Powershell
  - Security
  - Scripting
  - Azure
  - Metadata
---
<img src=/public/metadata.png>   

# What does Azure have that VMware never had?
There's probably a ton of answers here.  But the one I was angling for is "Which vCenter is my VM running on?"  
Who'd be crazy enough to lose a VM?  Well you'd be surprised in enterprise estates that have been running for 10 years and Sue who managed the VM farm just retired and moved to Cypus so you can't get hold of her to ask...
Anyway, what I'm going to talk about it more useful than just finding out where your VM is if you (or your script/code) only has access inside the VM.

So by design normally it's good to obfuscate (I love that word) details of the hypervisor above and not let the VM know it's anything but a physical server.
So far so 1999.
But what if we need more info to decide whether or not to install some software or configuration on an Azure VM.  What sub is it in? What RG or region? What tags does it have?
Well you could run Azure powershell commands out to azure but that would be a really circular Pop Will Eat Itself route and get messy quickly.

## Get to the point! The Azure Instance Metadata Service to the rescue
If you want the Long Read, here's the docs
(https://docs.microsoft.com/en-us/azure/virtual-machines/windows/instance-metadata-service)
But the short version is there is a rest api you can query from powershell or CMD on a windows server or curl on a linux Azure VM to get data about the VM that is from the Azure Subscription level.
How to do it?
1. Create a VM linux or Windows
2. Log on to it via your preferred method.  RDP, SSH or Serial Access Console
3. Run the following command `Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2018-10-01 -Method get | ConvertTo-Json`  for windows
or `curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01"` for Linux

This should return something like this:
```
PS C:\Users\blogadmin> Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2018-10-01 -Method get | ConvertTo-Json
{
    "compute":  {
                    "azEnvironment":  "AzurePublicCloud",
                    "location":  "eastus",
                    "name":  "WinVM1",
                    "offer":  "WindowsServer",
                    "osType":  "Windows",
                    "placementGroupId":  "",
                    "plan":  {
                                 "name":  "",
                                 "product":  "",
                                 "publisher":  ""
                             },
                    "platformFaultDomain":  "0",
                    "platformUpdateDomain":  "0",
                    "provider":  "Microsoft.Compute",
                    "publicKeys":  [

                                   ],
                    "publisher":  "MicrosoftWindowsServer",
                    "resourceGroupName":  "Blog-Metadata",
                    "sku":  "2019-Datacenter",
                    "subscriptionId":  "fe2c8ad9-fde6-41ee-8673-5d3cfd54d9f1",
                    "tags":  "",
                    "version":  "2019.0.20190410",
                    "vmId":  "eb3efc1e-f0d9-419c-82b1-d31ac6d543ff",
                    "vmScaleSetName":  "",
                    "vmSize":  "Standard_DS1_v2",
                    "zone":  ""
                },
    "network":  {
                    "interface":  [
                                      "@{ipv4=; ipv6=; macAddress=000D3A1FE6BF}"
                                  ]
                }
```

## Good Copy/Paste, but what about detail?
Ok so to get the public IP run this command:
`(Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2018-10-01 -Method get).network.interface.ipv4.ipaddress.publicipaddress
137.117.84.156`

Or something more useful like the tags so you can tell if it's a prod/qa/test or web/db/DC etc.
```
PS C:\Users\blogadmin> (Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2018-10-01 -Method get).compute.tags
Owner:Chris N
```
<img src=/public/vmtags.png>
