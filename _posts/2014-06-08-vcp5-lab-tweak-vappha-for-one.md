---
layout: post
title: VCP5 Lab Tweak - vApp/HA for one?
tags:
- VCP5
- VCP
- vapp
- HA
- High Availability
date: 2014-06-08T20:00:00+00:00
comments: true
---
I've been using my little HP Microserver and it's a beauty.  However I do have to keep powering it on and then connecting with vi client to the host then powering on the VC Appliance, then the virtual ESXi hosts, you know the drill..........

So how to get them to autostart.  Well it's a simple option and to my mind it looks like a mix of a vApp and HA :-)

You get to put a VM into one of three categories <strong>Manual Startup/Any Order/Automatic Startup</strong>.

So I moved my VCSA to the top and set a 30 second delay followed by my first ESXi host, then the second.  I've shortened the power down delay as it's a lab and I don't really care if they power off a bit dirty :-P

<a href="https://chrisneale.files.wordpress.com/2014/06/apo.png"><img class="alignnone wp-image-161 size-medium" src="http://chrisneale.files.wordpress.com/2014/06/apo.png?w=300" alt="Auto Power On vSphere VM with host" width="300" height="168" /></a>

Looking again it's bit more like a vApp Startup Priority Order than HA.  Oh well, 1 more bit of automation!

&nbsp;
