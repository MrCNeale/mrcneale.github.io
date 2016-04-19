---
layout: post
title: Sometimes Windows is just better - Configuring RHLES OS updates with mrepo/RHN
excerpt: RHLES RHUI configuration tricks and tips
tags: 
  - bluecoat
  - mrepo
  - patching
  - redhat
  - rhles
  - rhn
  - rhn_register
  - up2date
date: 2014-05-20T00:00:00+00:00
comments: true
---

I was setting up a small environment in a DMZ to be used to pull in patches for VMware UMDS, Shavlik (windows OS) and Redhat.

Now just the act of "connecting the utility to the internet via the proxy" is what I'm focussing on.

In Shavlik there is a box in the gui to enter the proxy authentication username and password

In UMDS you specify the credentials during setup/installation (you can also easily edit xml after install if you forget)

Then it came the redhat server.  I ran their "text-gui" to register on the redhat network..."ooh it's asking me for proxy details, good".  That bit worked fine.  Then came the mrepo command to start dragging down all the latest updates.

BOOM failure.  "Proxy authentication required".  Hmm ok so I checked mrepo.conf and could only see the updated mrepo.conf
<strong>http_proxy </strong>and <strong>https_proxy</strong> variables were declared.  Searching around various sources said there were variables listed as proxyUser, proxyUsername, proxyPassword all of which I tried but to no avail.

After much head banging I went all 90s ftp and tried declaring the <strong>http_proxy</strong> variable in the following format

<em><strong>http://username:password@proxyip:proxyport/</strong></em>

and success was attained.  This is crazy and not being able to set a system wide proxy in RHLES or at least to have a proper, supported method of downloading updates through an authenticated proxy is just poor.

This one time.
<h2><strong>Windows Wins!</strong></h2>
