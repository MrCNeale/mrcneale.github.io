---
layout: post
title: PowerCLI running in a Docker Container on Photon OS in under 10 minutes
excerpt: From OVA download to Get-VM, sub 10 mins!
tags: 
  - Docker
  - PowerCLI
  - PowerShell
  - Containers
  - VMware
date: 2016-11-07T22:00:00+00:00
comments: true
---

I needed a utility VM to run a PowerCLI script as a scheduled task.  
I set out to do this using Windows but stopped as VMware's Geniuses had just released the PowerCLIcore Fling.  
This instantly made me think, smaller footprint, quicker to deploy, zero OS license cost......**No Brainer really**

To recreate this all you need is something to host your Photon VM.  Standalone ESXi host or VMWorkstation.

Oh and a VCSA/ESXi to connect to (v5.5 or higher)...but then you wouldn't be here if you didn't have that already!

Erm...I also decided to demonstrate it with a video, because, erm, why not?
  
<iframe width="560" height="315" src="https://www.youtube.com/embed/hhsu00m05zU" frameborder="0" allowfullscreen></iframe>
<HR>
Glossary of commands/useful information


PowerShell Command to download OVA
{% highlight ruby %}
Invoke-WebRequest -Uri https://bintray.com/vmware/photon/download_file?file_path=photon-custom-hw10-1.0-13c08b6.ova -OutFile c:\users\chris\downloads\photon.ova
{% endhighlight %}

Photon OVA default root password=changeme


Photon/Linux systemctl setup Commands
{% highlight ruby %}
systemctl enable docker
systemctl start docker
{% endhighlight %}

Docker Commands
{% highlight ruby %}
docker run -it vmware/powerclicore
docker ps
docker images
docker rmi -f vmware/powerclicore
{% endhighlight %}


PowerCLI Commands
{% highlight ruby %}
set-powercliconfiguration -invalidcertificateaction ignore -confirm:$false
connect-viserver -server 192.168.0.200 -user administrator@vsphere.local -password vmware
get-cluster
get-vmhost
get-vm
exit
{% endhighlight %}
