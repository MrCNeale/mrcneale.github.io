---
layout: post
title: Host Profiles to the Rescue!
date: 2015-01-12T13:00:00+00:00
comments: true
---
I haven't used host profiles much in anger.  I've seen them used as first snapshots of host config, but rarely updated (naughty I know!).

I'm currently in the midst of revising/labbing for my VCAP5-DCA. So I have my nested esxi lab environment running in workstation 10.  All was going well until I tried to test out some VMware Update Manager tasks.  I had installed VUM on a 2k3 VM running on one of my esxi hosts (I may break it out to run in workstation when I have more memory).  I could scan/remediate the host the VM was on, but I couldn't scan the other host.  I tried vmotioning the VM.  It failed...due to a networking issue.  I cold vmotioned the powered off VM and started it on the 2nd host and VUM could scan it...but not the original host.

A quick run through was that the hosts could ping the default gateway (the VMWorkstation switch) they could even both see my Synology NAS and mount/map NFS/iSCSI LUNs.  But they couldn't ping each other.  They could ping everything else and everything else could ping them.  They had just plain fallen out with each other!

I ran over the network config in workstation and there is very little to customise on a per-vm setting.  Plus they were just NAT'd exactly the same as the other VMs.
VMworkstation Virtual Network ruled out.

So it was something specific to both hosts.  Now foolishly I've been doing lots of lab exercises and not got my esxi hosts set to have non-persistent disks.  So somewhere in the last week or so I have changed the networking and not recorded/documented the random PowerCLI/esxcli/vma command which has stopped them talking.

Then I had an epiphany.  I don't need to know what's different.  I'll just use vSphere/vCenter to set them the same!

Simple Solution
<ol>
	<li>Create host profile of HostA</li>
	<li>Apply host profile to HostB</li>
</ol>
And voila.  Ping was working!

<img class="alignnone size-large wp-image-236" src="https://chrisneale.files.wordpress.com/2015/01/hostprofile.png?w=800" alt="hostprofile" width="800" height="315" />

(Note that there is an error but this can be ignored as storage was still visible/accessible on both hosts and is caused because the Host Profile captures storage settings which are unique.  Read here &gt;<a href="http://virtualcloudzz.blogspot.co.uk/2013/06/host-profiles-compliance-error-in.html"> http://virtualcloudzz.blogspot.co.uk/2013/06/host-profiles-compliance-error-in.html </a> )
<h3>The other message here is:</h3>
<h3>If you're using a lab environment set your disks non persistent.</h3>
<h3> If you're in Production....don't make a change without documenting it and creating a new host profile with similar level of detail/description to explain what changes have been made.</h3>
<h3></h3>
