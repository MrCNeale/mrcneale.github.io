---
layout: post
title: The iSCSI Mystery
date: 2015-08-11 23:24
author: Chris Neale
comments: true
categories: [iSCSI, VMXNet3, VMware, Routing, Metrics]
---
I was asked to look at an issue recently with bad IO performance on an SQL VM.  SQLio tests were being run and the figures coming back were much lower than expected. There was a little bit of CPU Ready but only occaionally, and the environment was slightly overcommittted.  However after some quick investigation windows task manager showed that the iSCSI traffic was going over the Production Virtual NIC!?!?!

The VM had 2 NICs.  One for Production traffic and one for iSCSI, connected to different networks and only the Production NIC had a default gateway set.  However the iSCSI Filer IP was on the same subnet as the iSCSI NIC so why wasn't traffic routing down that NIC?  There was no overlap so I ran a 
{% highlight ruby %}
route print
{% endhighlight %}
Here's where I went down some rabbit holes and fixed some issues.

First check:
What kind of NICs were configured?
Here came the first
