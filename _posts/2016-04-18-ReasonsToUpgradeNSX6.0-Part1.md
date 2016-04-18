---
layout: post
title: Reasons to upgrade NSX 6.0/6.1 to 6.2 #325
excerpt: Bugs that are fixed in 6.2 cause controller issues
tags: 
  - NSX
  - vSphere
  - VMware
  - ESG
  - ARP
  - Controllers
date: 2016-04-18T22:00:00+00:00
comments: true

---

I'd like to thank my colleague Robbie Hancock [https://twitter.com/blobbieh](https://twitter.com/blobbieh) for teasing out the troubleshooting/identification of problem steps.
<P></P>
**Symptom** :

If you have VMs that

1. Stop responding to network requests
2. Cannot ping a VM from another VM or ESG
3. Do not have entries in their ARP table
4. Initiating a ping from the affected VM to ESG or another VM and traffic resumes (ARP table entry appears)
5. After an amount if inactivity, the problem returns.... (suspect ARP table ages out entry)


**To troubleshoot the issue** :  
Log on to an NSX controller and identify which one is the master for the affected VNI (5001 used here as an example)
{% highlight ruby %}
show control-cluster logical-switches vni 5001
{% endhighlight %}

This will show which controller is the master.  Cross reference this IP with the relevant NSX Controller in vCenter and ssh into that one.  
 
Check that all ESXi Hosts have a connection to controllers:  
{% highlight ruby %}
show control-cluster logical-switches connection-table 5001
{% endhighlight %}

This will show that only some of the ESXi Hosts are connected to controllers, most likely the missing host is where your missing VM is running.

Check which VNIs are present on the affected host (192.168.0.10 used here for example)
{% highlight ruby %}
show control-cluster logical-switches joined-vnis 192.168.0.10
{% endhighlight %}
If you get "error: not found" on a host with a lot of VMs on it, that's not right!  You should get a get a long list of VNIs

So now we have a pointer to the cause.  Test migration of the affected VM onto another host and see if the problem persists if this instantly resolves the problem then you have the issue.
<P></P>
**The Fix**

Putty onto the ESXi Host and restarts the user world agent (netcpa):

Re check to see if the ESXi host has joined the VNIs:
{% highlight ruby %}
show control-cluster logical-switches joined-vnis 192.168.0.10
{% endhighlight %}

 

This is a known issue with NSX 6.1.x  click here to see the 
<A href="http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&ampcmd=displayKC&ampexternalId=2137005&ampsrc=vmw_so_vex_cneal_850">KB</A>
