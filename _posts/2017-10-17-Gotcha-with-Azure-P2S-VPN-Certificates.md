---
layout: post
title: Gotcha when importing client certificates for Azure P2S VPN
excerpt: RTFM, and then read the addendums, and the notes and all the links as well
tags: 
  - Powershell
  - Security
  - Scripting
  - Azure
  - P2SVPN
date: 2017-10-17T16:00:00+00:00
comments: true
---
After waiting 30 minutes for my P2S VPN gateway to deploy in Azure I was obviously eager to get VPN-ing, who wouldn't be!  
I ran through the instructions to generate a self-signed Root Cert and a client cert from Microsoft on this page [https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-certificates-point-to-site](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-certificates-point-to-site)
*(Put a pin here, we'll come back to this)
