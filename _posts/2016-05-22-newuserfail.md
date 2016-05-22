---
layout: post
title: Newly created vsphere.local SSO user gets username/password error
excerpt: Why VMware's own defaults are wrong
tags: 
  - vSphere
  - VMware
  - VCSA
  - SSO
  - PSC
date: 2016-05-21T22:00:00+00:00
comments: true
---

I was recently creating new users in vsphere.local in a lab environment.  When I came to test logging in I got "invalid username/password" from the web-client.

Hmm odd.  So I went back in and checked the username was spelt correctly and reset the password typing slowly.

Back out.  Tried login again....same error.  Ok maybe I'm being really fat fingered.  Nope I typed the password into a notepad and it was correct.  No special characters and exactly the same as an old vsphere.local account I can login with fine.......

My setup was a VSCA 6.0 pointed at an external PSC for SSO.  So I went to the PSC/SSO logs which are stored (for windows) at
C:\PogramData/VMware/vCenterServer/logs/sso/vmware-sts-idmd.log

I saw this and was confused 

{% highlight ruby %}Failed to authenticate principal [username@SSO_Domain]. User password expired.{% endhighlight %}

How can a newly created user have an expired password.  I checked the password expiry length setting and it was to this.
[password expiry setting](/public/pawd1.png "Password Expiry Setting")
Plenty I thought.  Then I found this KB.

[https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2125495&src=vmw_so_vex_cneal_850](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2125495&src=vmw_so_vex_cneal_850)

So if the **maximum lifetime password expiration value** is larger than the permitted maximum it fails to create users correctly.

But why did I set it wrong?

Well I didn't.  This environment was used to document an upgrade and so the root cause was that the setting above is what vSphere 5.5 and SSO 5.5 set it to!

This was the first account I had created after the upgrade.  How do you fix your new accounts?

1. Change the setting in SSO to below the maximum [Password Expiry Setting](/public/pawd2.png "Password Expiry Setting")
2. Then DELETE and recreate your user! (very poor show vmware!)

So there we have it.  VMware's own default changing between versions. Smooth move.....
