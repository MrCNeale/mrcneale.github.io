---
layout: post
title: The iSCSI Mystery (+Death to e1000s)
date: 2015-08-11 23:24
author: Chris Neale
comments: true
categories: [iSCSI, VMXNet3, VMware, Routing, Metrics]
---
I was asked to look at an issue recently with bad IO performance on an SQL VM.  SQLio tests were being run and the figures coming back were much lower than expected. There was a little bit of CPU Ready but only occasionally, and the environment was slightly overcommitted.  However, after some quick investigation windows task manager showed that the iSCSI traffic was going over the Production Virtual NIC!?!?!

The VM had 2 NICs.  One for Production traffic and one for iSCSI, connected to different networks and only the Production NIC had a default gateway set.  However the iSCSI Filer IP was on the same subnet as the iSCSI NIC (accessed using a Microsoft iSCSI Initiator) so why wasn't traffic routing down that NIC?  There was no overlap so I ran a 
{% highlight ruby %}
route print
{% endhighlight %}
Here's where I went down some rabbit holes and fixed some issues.

The routing table showed that the default gateway had a lower Metric than the NIC that was on the same subnet.  I thought this was the issue initially so I set about trying to get the Metric for the iSCSI connected NIC lower.

I learned some things about how Windows 2012 Automatically works out metrics for NICs.
<LI>The line speed that a NIC has negotiated contributes to the Metric score faster=lower (change your PCs NIC from 1gb to 100Mbps and the metric jumps from 20 to 306 </LI>
More detail can be found here <A HREF=https://support.microsoft.com/en-us/kb/299540>https://support.microsoft.com/en-us/kb/299540</A>

It was then that I spotted something I had taken for granted....the virtual NIC types were differet.  Somehow this VM had been configured wrong by whoever added the iSCSI NIC and they had selected e1000 !!!<BR>
Changing the NIC type to VMXNet3 meant the line rate jumped up to 10Gbps from 1Gbps and now the Metric for the iSCSI nic was the same as the Production default gateway...but not lower.  

I tried manually editing the Production NIC's default gateway metric to add 300 to it to make it higher.  
This did make the routing table look right from a metrics perspective....but still the traffic went over the Production NIC.  Thankfully after bouncing the issue off a few colleagues one did come back with a config guide for Microsoft iSCSI Initiator on 2012.  <BR>
Now I hadn't configured the initiator either, we have a storage team who did that.  They had just left some settings default, and it appears that iSCSI ignores your IPv4 Routing Table in Windows.  So all my fidgetting above was in vain (although it did highlight the build issue with e1000 vs VMXNet3).

Microsoft's guide does state
{% highlight ruby %}If you want to dedicate iSCSI traffic to a specific set of the NICs, you can specify that in the “Connect using”. By default, any IPs can be used for iSCSI connection.{% endhighlight %}
<LI>Here is where you set it 
<IMG SRC=http://blogs.technet.com/cfs-file.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-00-47-85-metablogapi/7484.image_5F00_20EE0961.png></LI>
<LI>Here is the link to Microsoft's guide <A HREF=http://blogs.technet.com/b/filecab/archive/2012/05/21/introduction-of-iscsi-target-in-windows-server-2012.aspx> http://blogs.technet.com/b/filecab/archive/2012/05/21/introduction-of-iscsi-target-in-windows-server-2012.aspx</A></LI>

In Summary
<LI> Death to e1000s always go VMXNet3 FTW!</LI>
<LI> iSCSI is ignorant to traditional routing tables in Windows and should be pinned to a specific NIC as required</LI>

I hope that saves you the morning I had!
