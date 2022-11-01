---
comments: true
date: "2019-06-12T22:00:00Z"
excerpt: Dig a bit deeper to bet better error reporting
image:
  feature: vm.png
tags:
- ARM
- Template
- Troubleshooting
- Azure
- Extensions
title: Troubleshooting Azure VM Extension Installation Failures
---
<img src="/images/vm.png">   

# What can you do to find out more about why a VM extension is failing or not working quite right?
Azure VMs are great and VM extensions are even better.  Either a Vendor, or you, can create some code to do a thing to a VM.  Usually install software or agents and configure it.
Most common for the average user will be things like the Log Analytics Agent Extension and the Custom Script Extension (where you can run your own Powershell or Bash post install).
Oh...and the AD Domain Join extension for Windows VMs.

So I'm going to work on the presumption you've copied a quickstart template

[https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-domain-join-existing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-domain-join-existing) or written your own template or manually added an Extension via Portal>VM>Extensions>Add say for the Symantec Cloud AV extension.

You run the install or deploy the template, but you get a failure.  I'm willing to bet the deployment message is cryptic and not very useful, especially for Domain Join.  
<img src="/images/faileddep.png">   <img src="/images/failedop.png">   
So you can log in to the VM and start looking in the event log for clues, but that's more effort than I can be bothered with trawling for.  

What's the alternative?  
Well every extension installed downloads to a particular folder.  
C:\Packages\Plugins\<extension name>  
e.g. for Domain Join it's C:\Packages\Plugins\Microsoft.Compute.JsonADDomainExtension\1.3.2
Every installation of an extension creates a log in another folder.  
e.g. C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.JsonADDomainExtension\1.3.2  
So the first 2 easy steps are  
1. Confirm the contents of the install folder look good (if this is a custom script, you'll know best, if it's 3rd party, it may not be clear)  
<img src="/images/installfolder.png">  
2. Next look at the logs sub-folder named after your extension and review the logs for more detail.  
What we can see is that it fails and throws error 1355  
Which good old eventid.net tells us is a connection failure (coz bob.com doesn't exist or is resolvable on my vnet)  
[http://www.eventid.net/errorsdisplay.asp?error_code=1355](http://www.eventid.net/errorsdisplay.asp?error_code=1355)  
<img src="/images/installlog.png" width="1365" height="498">(/images/installog.png)    
There you have it. How to drill down inside a VM to find issues with installs of Microsoft, 3rd Party or your own CustomScript Extensions.

NOTE: For linux the folders are  
Logs = /var/log/azure/  
Install = (this runs in the cwd of where it's downloaded) so search the filesystem for PackagesDirectory  


Links  
-----  
AD Domain Join Extension
[https://blogs.msdn.microsoft.com/igorpag/2016/01/25/azure-arm-vm-domain-join-to-active-directory-domain-with-joindomain-extension/](https://blogs.msdn.microsoft.com/igorpag/2016/01/25/azure-arm-vm-domain-join-to-active-directory-domain-with-joindomain-extension/)
Log Analytics Agent VM Extension  
[https://docs.microsoft.com/en-gb/azure/virtual-machines/extensions/oms-windows(https://docs.microsoft.com/en-gb/azure/virtual-machines/extensions/oms-windows)
[https://docs.microsoft.com/en-gb/azure/virtual-machines/extensions/oms-linux](https://docs.microsoft.com/en-gb/azure/virtual-machines/extensions/oms-linux)
Custom Script Extension  
[https://docs.microsoft.com/en-gb/azure/virtual-machines/extensions/custom-script-windows](https://docs.microsoft.com/en-gb/azure/virtual-machines/extensions/custom-script-windows)
[https://docs.microsoft.com/en-gb/azure/virtual-machines/extensions/custom-script-linux](https://docs.microsoft.com/en-gb/azure/virtual-machines/extensions/custom-script-linux)
