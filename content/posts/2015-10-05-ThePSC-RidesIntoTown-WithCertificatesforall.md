---
author: Chris Neale
categories:
- VMware
- PSC
- vSphere
- Certificates
- Security
comments: true
date: "2015-10-05T00:00:00Z"
title: All hail the PSC - Long Live the appliance!
---
VMware started as a Type 2 Hypervisor for doing a bit of lab/development testing on.

They've come a long way since and even started chopping Windows out of the picture whenever possible.
VCSA did a great job at removing not just vCenter but a number of the vSphere components like dump collector, log browser, syslog collector and auto deploy.

But to have a fully tidy and enterprise environment you needed certificates.  You could buy these from Verisign/Symantec but as only internal people will be accessing your vSphere environment then why go to the expense and faff?
The other option is a Windows PKI.  Free/Easy and quick to set up (possibly quite badly) but you've already got Windows servers, why not just add a role?

So what have VMware done to remove some of this hassle and add to their portfolio?  Well a while back they released vSphere 6.0  If you haven't looked at it I recommend setting it up in a lab and getting familiar.  It has one key new component that I think is pretty cool.

<H1> The PSC</H1>
 PSC deals with identity management for administrators and applications that interact with the vSphere platform and more.
 It hosts a bunch of services, some existing some new
<LI>VMware Appliance Management Service (only in Appliance-based PSC)</LI>
<LI>VMware License Service</LI>
<LI>VMware Component Manager</LI>
<LI>VMware Identity Management Service</LI>
<LI>VMware HTTP Reverse Proxy</LI>
<LI>VMware Service Control Agent</LI>
<LI>VMware Security Token Service</LI>
<LI>VMware Common Logging Service</LI>
<LI>VMware Syslog Health Service</LI>
<LI>VMware Authentication Framework</LI>
<LI>VMware Certificate Service</LI>
<LI>VMware Directory Service</LI>

My favourite bit, having become an Identity&Access Management SME by stealth is the Certificate service.  In my experience customers struggle to really grasp what a PKI is for and to understand the relatively simple moving parts.
<OL>Certificate Authorities - They issue certs!</OL>
<OL>Certificate Revocation lists (CRLs) - Lists of revoked certificates</OL>
<OL>Crl Distribution Points (CDPs) - a Point where CRLs are hosted that clients can connect to and download the latest CRL</OL>
That simplifies things down to their minimum.  There are other bits but we don't need them yet.

To secure a connection a service needs an SSL cert. 
e.g. the Web Client Service. has a cert but it issued it to itself.  Nothing trusts it, there's nowhere to check if it's been revoked and it's very likely that you're accessing it using an ip/name different to the one in the cert.

The advantage of the Certificate Service or VMCA is that it simplifies or automates a lot of these processes for you, including the setting up of the Root CA.  
Here's a video overviewing the certificate services in vSphere6.0

<iframe width="560" height="315" src="https://www.youtube.com/embed/KaBF11Vd6aM" frameborder="0" allowfullscreen></iframe>

In a follow up post I'll contrast setting up a windows CA and manually issuing certs out to each versus the PSC.
