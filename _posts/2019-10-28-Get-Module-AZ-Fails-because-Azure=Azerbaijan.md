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




<H2>Fixed in 6.2</H2>

Including pre-req checks in your code is good practice....so you add it in.
In our Powershell scripts which require Azure Powershell cmdlets we have moved, as should anyone keeping abreast of releases, to Powershell Core.  
But our checks to see if the Az Module is installed were giving mixed results.  

Even though the commands all worked, *Get-AzVM* etc. the module check for "Az" returned nothing, blank, nada, 
But if you check for a sub module, e.g. Az.Accounts, it works...*strange*
'''Powershell
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
'''

@ronhowe The fix is only for PowerShell Core. You'll get it in PowerShell Core 6.2 RC in hours and in release PowerShell Core 6.2 in weeks.

Windows PowerShell 5.2 is not expected. Although MSFT may port this fix to 5.1.

https://github.com/PowerShell/PowerShell/pull/8777
