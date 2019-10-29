---
layout: post
title: Get-Module AZ fails because Azure=Azerbaijan
excerpt: Culture Wars on the command line
comments: true
date: 2019-10-28T22:00:00+00:00
image:
  feature: https://avatars2.githubusercontent.com/u/11524380?s=200&v=4
tags: 
  - Powershell
  - Security
  - Scripting
  - Azure
---
<img style="float:right;" src="https://avatars2.githubusercontent.com/u/11524380?s=200&v=4">




<H2>When is a Culture a Module?</H2>

Including pre-req checks in your code is good practice....so you add it in.  
In our Powershell scripts which require Azure Powershell cmdlets we have moved, as should anyone keeping abreast of releases, to Powershell Core.  
But our checks to see if the Az Module is installed were giving mixed results.  
  
Even though the commands all worked, *Get-AzVM* etc. the module check for "Az" returned nothing, blank, nada.  
But if you check for a sub module, e.g. Az.Accounts, it works...*strange*  
  
  
```Powershell
PS C:\> $PSVersionTable

Name                           Value
----                           -----
PSVersion                      5.1.15063.1805
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.15063.1805
CLRVersion                     4.0.30319.42000
PS C:\> Get-Module -ListAvailable -Name az
PS C:\> Get-Module -ListAvailable -Name az.accounts
    Directory: C:\Program Files\WindowsPowerShell\Modules
ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Script     1.6.3      Az.Accounts                         {Disable-AzDataCollection,
PS C:\>
```
(I know the above is Windows .Net Powershell, but I didn't have an old Powershell Core 6.1.x installation)
As you can see it fails.  
This will be observered on Windows Powershell and Powershell Core < 6.2  

It seems that there was a bug where Powershell ignores any directory in the Powershell Modules path which matches a "Culture". Think language packs here. e.g. en/us/fr/de
Those of us less travelled or from that region will not have known that az=Azerbaijan so that is why the command fails.

After much searching and lots of linked bug reports the following link has the "Fixed" pull request  
[https://github.com/PowerShell/PowerShell/pull/8777](https://github.com/PowerShell/PowerShell/pull/8777)  
This PR gives you a reason to move from Windows Powershell, because they currently don't forsee the fix making it into Powershell 5.1 for Windows and there is no plan for a Powershell 5.2 release.  
If your version of Powershell Core is < 6.2 then you should update as soon as is feasible  
  
> @ronhowe The fix is only for PowerShell Core. You'll get it in PowerShell Core 6.2 RC in hours and in release PowerShell Core 6.2 in weeks.
> Windows PowerShell 5.2 is not expected. Although MSFT may port this fix to 5.1.

