---
layout: post
title: Reasons to upgrade NSX 6.0 - Part 1
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

This will show that only some of the ESXi Hosts are connected to controllers, most likely the missing controller is running on the host where your VM is missing.

Check which VNIs are present on the affected host:

This is not normal, as the host has a bunch of VMs in it, connected to a number of VNIs, here's a normal response (from another host):

 
So.... we have a suspect  ...
 
Test migration of the affected VM onto another host and see if the problem persists.... it didn't.. the VM responded straight away 
 
Putty onto the ESXi Host and restarts the user world agent (netcpa):

 
Re check to see if the ESXi host has joined the VNIs:

 
We have lift off !!!!
 
This is a known issue with NSX 6.1.x (http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2137005)

