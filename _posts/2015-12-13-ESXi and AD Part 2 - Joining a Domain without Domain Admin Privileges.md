---
layout: post
title: ESXi and AD Part 2 - Joining a Domain without Domain Admin Privileges
date: 2015-12-13
author: Chris Neale
comments: true
categories: [VMware,ESXi,AD,Active Directory,Security]
---

I have an e-mail from VMware support stating the following

<I>"I've discussed this issue with my escalation engineer's. 

To add/join the ESXi host to AD you need to have a Domain Admin privilege. 

This email can be considered to be an official document, however if you have any more queries, please reply back to this email."</I>

This is beyond disappointing.  It means that they don't know or care how AD works an aren't serious about supporting it as an Identity Provider.

In the environment I'm working in I am not getting Domain Admins any time soon, and rightly so it's a sledgehammer to crack a nut!

I started off using this slightly outdated article from Microsoft on how to delegate rights to a user to allow them to join computers to a domain

[https://support.microsoft.com/en-us/kb/932455](https://support.microsoft.com/en-us/kb/932455)

