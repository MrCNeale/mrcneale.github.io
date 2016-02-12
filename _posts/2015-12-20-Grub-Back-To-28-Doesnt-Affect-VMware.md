---
layout: post
title: "vSphere vApps unaffected by Back to 28 Vulnerability (CVE-2015-8370)"
excerpt: Back to 28 Vulnerability
tags: 
  - VMware
  - VROPS
  - VCOPS
  - VCSA
  - Log Insight
  - Security
  - SLES
  - Vulnerability
  - CVE-2015-8370
image:
  thumb:
date: 2015-12-20T00:00:00+00:00
comments: true

---
<IMG src="http://hmarco.org/bugs/grub_hacked-b.png" width="100">

Recently Hector Marco and Ismael Ripoll discovered a vulnerability in common Linux bootloader GRUB.

It affects GRUB2 specifically and was introduced into that version in 2009

[http://hmarco.org/bugs/CVE-2015-8370-Grub2-authentication-bypass.html](http://hmarco.org/bugs/CVE-2015-8370-Grub2-authentication-bypass.html)

[https://web.nvd.nist.gov/view/vuln/detail?vulnId=CVE-2015-8370](https://web.nvd.nist.gov/view/vuln/detail?vulnId=CVE-2015-8370)


However it doesn't affect the original GRUB.


I was concerned about the Virtual Appliances we were running in one of our environments.

* VCSA -  vCenter Server Appliance 5.5 Update 2e | 16 APR 2015| Build 2646489
* vRealize Log Insight 3.0 GA Build 3021606
* VROPS 6.1.0 Build 3038036

Each of these VApps runs on SLES 11 patch 3.  Now whilst the release notes do indicate that GRUB2 is a new package
it doesn't specify that GRUB2 is now the default bootloader.  VMware also refer to their Virtual Appliances as "Security Hardened" 
[https://blogs.vmware.com/vsphere/2013/09/virtual-appliances-getting-more-secure-with-vsphere-5-5-part-1.html](https://blogs.vmware.com/vsphere/2013/09/virtual-appliances-getting-more-secure-with-vsphere-5-5-part-1.html#src=vmwsovexcneal_850)
So whether SUSE just didn't make GRUB2 default in 11.3 or VMware chose to revert back to GRUB a quick check on any of  the appliances can show you the grub version

{% highlight ruby %}
grub-install.unsupported -v
{% endhighlight %}

This command will return the GRUB version which for (all that I can tell) current vSphere 5 or 6 Virtual Appliances is version 0.97 so GRUB not GRUB2.
Panic over.  Normal service is resumed
