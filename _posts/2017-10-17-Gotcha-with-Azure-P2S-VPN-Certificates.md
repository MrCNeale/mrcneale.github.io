---
layout: post
title: Gotcha when importing client certificates for Azure P2S VPN
excerpt: RTFM, and then read the addendums, and the notes and all the links as well
comments: true
date: 2017-10-16T22:00:00+00:00
image.feature: azurevpn.png
tags: 
  - Powershell
  - Security
  - Scripting
  - Azure
  - P2SVPN
---
<img style="float: right;" src="http://www.chrisneale.org/public/azurevpn.png">
After waiting 30 minutes for my P2S VPN gateway to deploy in Azure I was obviously eager to get VPN-ing, who wouldn't be!  
I ran through the instructions to generate a self-signed Root Cert and a client cert from Microsoft on this page [https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-certificates-point-to-site](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-certificates-point-to-site)  
\*(Put a pin here, we'll come back to this)  
I'd imported the certs to my test client VM and the Client VPN setup exe ran ok and created a connection in "Network Connections".  I tried to connect and received an error    
<BR><BR>
**A certificate could not be found that can be used with this Extensible Authentication Protocol. (Error 798)**  
<BR><BR>
<img style="float: left;" src="http://www.chrisneale.org/public/certerrror.png">
I foolishly didn't Google first and I checked the comments section at the bottom of the instructions.  The Microsoft engineer referred to how they moved from MakeCert.exe to the new Powershell <p style="color:blue">New-SelfSignedCertificate</p> cmdlet.  However, it became clear during testing and reading that the nice bod from Microsoft was using all the new things and not testing backward compatibility.  
That's fine but they didn't initiall stipulate that this would only work for Windows 10/2016 clients.  
A bit more digging and it turns out that the default import options for a cert **BEFORE** Windows 10/2016 enable "Strong Private Key Protection". That sounds like a good thing, but it actually means the OS prompts whenever the certificate (and it's private keys) are accessed.  Which breaks the VPN client connection package.  

So the simple solution is that:  
When importing a VPN Client Cert and you are presented with the option to *"Enable strong private key protection"*, untick it.  If you're on a newer OS, you won't get this prompt (tested on Server 2016).  Now this is in the troubleshooting guide, but it should be in the instructions, rather than something you have to search for after the fact.
[https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems)

So RTFM, and also any related KBs....and the TroubleShooting guide....and you'll be fine

