---
layout: post
title: Troubleshooting Azure VM Extension Installation Failures
excerpt: Dig a bit deeper to bet better error reporting
comments: true
date: 2019-06-12T22:00:00+00:00
image:
  feature: vm.png
tags: 
  - Powershell
  - Security
  - Scripting
  - Azure
  - Metadata
---
<img src="/public/vm.png">   

# What can you do to find out more about why a VM extension is failing or not working quite right?
Azure VMs are great and VM extensions are even better.  Either a Vendor, or you, can create some code to do a thing to a VM.  Usually install software or agents and configure it.
Most common for the average user will be things like the Log Analytics Agent Extension and the Custom Script Extension (where you can run your own Powershell or Bash post install).
Oh...and the AD Domain Join extension for Windows VMs.

So I'm going to work on the presumption you've copied a quickstart template
(https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-domain-join-existing) or written your own template or manually added an Extension via Portal>VM>Extensions>Add say for the Symantec Cloud AV extension.
You run the install or deploy the template, but you get a failure.  I'm willing to bet the deployment message is cryptic and not very useful, especially for Domain Join.
So you can log in to the VM and start looking in the event log for clues, but that's more effort than I can be bothered with trawling for.  
What's the alternative?
Well every extension installed downloads to a particular folder.  
Every installation of an extension creates a log in another folder.  
So the first 2 easy steps are
1. Confirm the contents of the install folder look good (if this is a custom script, you'll know best, if it's 3rd party, it may not be clear)  

2. Next look at the logs sub-folder named after your extension and review the logs for more detail.



Links  
-----  
AD Domain Join Extension
(https://blogs.msdn.microsoft.com/igorpag/2016/01/25/azure-arm-vm-domain-join-to-active-directory-domain-with-joindomain-extension/)
Log Analytics Agent VM Extension  
(https://docs.microsoft.com/en-gb/azure/virtual-machines/extensions/oms-windows)
(https://docs.microsoft.com/en-gb/azure/virtual-machines/extensions/oms-linux)  
Custom Script Extension  
(https://docs.microsoft.com/en-gb/azure/virtual-machines/extensions/custom-script-windows)
(https://docs.microsoft.com/en-gb/azure/virtual-machines/extensions/custom-script-linux)
