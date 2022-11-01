---
comments: true
date: "2014-05-30T20:00:00Z"
tags:
- ESXi
- hyperthreading
- memory compression
- ntp
- VCP
title: VCP 5 - Objective 1.2 - Host/Configuration Tab (in essence!)
---
So here I'm going to describe what many possibly have done before, but it's needed for the exam.....so here goes.

1. Configure NTP on an ESXi host.
Connect to the ESX/ESXi host using the vSphere Client.
Click the <strong>Configuration</strong> tab.
Click <strong>Time Configuration</strong>/<strong>Properties/</strong><strong>Options/</strong><strong>NTP Settings</strong>.
Click <strong>Add</strong>. Enter the NTP Server name. Click <strong>OK</strong>.
Click the <strong>General</strong> tab. Click <strong>Start automatically</strong> under Startup Policy.
Click <strong>Start</strong> and click <strong>OK</strong>. Click <strong>OK</strong> to exit.

2. Enable/Disable/Configure Hyperthreading
Connect to a host in vSphere Client
Click the <strong>Configuration</strong> tab.
Select <strong>Processors</strong>/<strong>Properties</strong>.
In the dialog box, you can view hyperthreading status and turn hyperthreading off or on (default).

3. Enable/Disable/Configure Memory Compression Cache
Connect to a host in vSphere Client
Click the <strong>Configuration</strong> tab.
Click <strong>Software/Advanced Settings
</strong>Select Mem from the options on the left.
On the right Mem.MemZipEnable can be set to 1 to enable or 0 to disable
Mem.MemZipMaxPct sets the maximum target size as a % of VM size.  Default 10%, Range 5-100

4. License an ESXi Host
Connect to a host in vSphere Client
Click the <strong>Configuration</strong> tab.  (Anyone seeing a pattern here??)
Click Software/Licensed Features
Click Edit
In there you can assign an existing license key or enter a new one.
Alternatively you can license from a vSphere Level via
<strong>Home/Administration/Licensing
</strong>Click<strong> Manage vSphere Licenses
</strong>From here you can<strong> add license keys (with labels for your identification then assign or remove them)</strong>

The End.
