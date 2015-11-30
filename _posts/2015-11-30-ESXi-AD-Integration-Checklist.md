---
layout: post
title: ESXi and AD Part 1: Integration Checklist
date: 2015-11-30
author: Chris Neale
comments: true
categories: [VMware,ESXi,AD,Active Directory,Security]
---

This first post is to run through common issues that are simple and "reasonable" to presume the end-user/sysadmin has checked first

* Firewall ports on the ESXi Host.  I'm going to presume host and AD are on the same subnet, if not then you are responsible for checking routing and firewalls between Host and AD Domain Controllers are configure correctly

Port 88 - Kerberos authentication

Port 123 – NTP

Port 135 - RPC

Port 137 - NetBIOS Name Service

Port 139 - NetBIOS Session Service (SMB)

Port 389 - LDAP

Port 445 - Microsoft-DS Active Directory, Windows shares (SMB over TCP)

Port 464 - Kerberos - change/password changes

Port 3268 - Global Catalog search

This KB lists the ports for ESXi 4.1  If you have these open it should be fine for higher ESXi versions too

[AD Ports required in ESXi](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1026538)

[All ESXi 5.x ports](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1012382#ESXi5x)

[All Port requirements from Microsoft](https://technet.microsoft.com/en-us/library/dd772723(WS.10).aspx)

* Time Sync.  Your hosts should be synced to the same source as your Domain Controllers
<div> Get-VMHost -VMHost $esx | Add-VmHostNtpServer  -NtpServer servername

      Get-VMHostFirewallException -VMHost $esx | where {$_.Name -eq "NTP client"} | Set-VMHostFirewallException -Enabled:$true

      Get-VmHostService -VMHost $esx | Where-Object {$_.key -eq "ntpd"} | Start-VMHostService

      Get-VmHostService -VMHost $esx | Where-Object {$_.key -eq "ntpd"} | Set-VMHostService -policy "automatic"
  </div>
* Permissions of the AD user you are using to join the domain.  If you're unlucky enough to not have a domain admin account to do the join then you'll need to ensure that the AD user account user account you are using has the following rights delegated to it on the OU where you are creating or updating, if it's been prestaged, the computer account.
    1. Reset Password
    2. Read and write Account Restrictions
    3. Validated write to DNS host name
    4. Validated write to service principal name
    5. Create/Delete Computer Objects
* DNS.  Your host should either use the AD DNS servers or be pointed to a DNS server with a conditional forwarder to the AD integrated DNS.  Your host should have a record in that DNS pre-created ideally. An nslookup from your hosts console should return all the DCs if you simply do

nslookup mydomain.com mydnsserver.mydomain.com

this should return the list of DCs

If you've ticked all those boxes then it should work.  I'm guessing if you're reading this then it hasn't.  Part 2 shortly to follow will go through troubleshooting AD&ESXi which don't seem to play well if anything differs from a very vanilla environment.
