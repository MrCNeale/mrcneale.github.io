---
layout: post
title: ESXi and AD Part 2 - Joining a Domain without Domain Admin Privileges
date: 2015-12-13
author: Chris Neale
comments: true
categories: [VMware,ESXi,AD,Active Directory,Security]
---

Problem
-------
* You need to join an ESXi host to AD but you do not have Domain Admins.
* Your AD team ask what rights ESXi requires and you can't tell them
* VMware Support give you a stock "You must have Domain Admins" reply to a support call.

Solution
--------
* Delegate the rights as per the following Microsoft KB Article 
[https://support.microsoft.com/en-us/kb/932455](https://support.microsoft.com/en-us/kb/932455)
* Also Delegate **Read All Properties** and **Write All Properties**
* Your OU permissions should look like this now (Tenant1Admin is the Account Mr Domain Admin has given you) (https://github.com/MrCNeale/mrcneale.github.io/blob/master/_posts/sshot1.PNG?raw=true)

The latter was the key to getting ESXi to be able to update random fields in the Computer Object such as *OperatingSystemServicePack* which VMware decided to populate with Likewise!

The Story/Diagnosis and how to Troubleshooting ESXi/AD/Likewise logs
---------------------------------------------------------------------

I have an e-mail from VMware support stating the following

<I>"I've discussed this issue with my escalation engineer's. 

To add/join the ESXi host to AD you need to have a Domain Admin privilege. 

This email can be considered to be an official document, however if you have any more queries, please reply back to this email."</I>

This is beyond disappointing.  It means that they don't know or care how AD works and are not serious about supporting it as an Identity Provider.

In the environment I'm working in I am not getting Domain Admins any time soon, and rightly so as it's a sledgehammer to crack a nut!

I started off using this slightly outdated article from Microsoft on how to delegate rights to a user to allow them to join computers to a domain

[https://support.microsoft.com/en-us/kb/932455](https://support.microsoft.com/en-us/kb/932455)

This lists how to delegate Creation and Deletion of Computer Objects to an OU and 4 permissions required to complete a domain join.  This works fine for Windows Hosts and Redhat Linux hosts joining using sssd. But ESXi's Likewise/Netlogon domain join borks at these permissions and throws either

"Errors in Active Directory operations" (An error so useless, yes frequent it should have it's own Twitter account!)

There are a vast array of reasons why ESXi might throw this, but presuming you followed Part 1 of this blog topic then the reason you are seeing it is because you don't have enough permissions in AD...........yet

The next step was to find out what ESXi/Likewise REALLY needs (seeing as Support don't know or won't tell).  So if you go into 