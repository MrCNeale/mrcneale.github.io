---
layout: post
title: ESXi and AD - It Shouldn't be this hard!
date: 2015-11-29
author: Chris Neale
comments: true
categories: [VMware,ESXi,AD,LDAP,Security]
---

<style type="text/css">
	table.tableizer-table {
	border: 1px solid #CCC; font-family: Helvetica, Arial, serif;
	font-size: 10px;
} 
.tableizer-table td {
	padding: 4px;
	margin: 3px;
	border: 1px solid #ccc;
}
.tableizer-table th {
	background-color: #104E8B; 
	color: #000;
	font-weight: bold;
}
</style>

It should be simple.  It should be a case of typing in a domain name and account with credentials and you're done.
Your host is joined to AD.  But no.  If you aren't doing completely vanilla AD and have a real production environment it looks like AD Authentication wasn't written for production environments.

I have been trying to write a script to join some hosts.  It's been done before, I'm not reinventing the wheel.  But I've hit a myriad of issues.  Here's a summary of all the issues you could find when you combine ESXi and AD.

* You aren't adding hosts to the default Computers OU
  You'll need to specify the AD OU when you join the host.  This isn't possible from the web client, so you need to use the fat client or powershell.
  <A HREF=http://www.definit.co.uk/2013/10/vsphere-security-active-directory-authentication/> http://www.definit.co.uk/2013/10/vsphere-security-active-directory-authentication/</A>
  This is a pain and if you've prestaged the computer account in AD then why oh why can't ESXi just go find it?
* 
