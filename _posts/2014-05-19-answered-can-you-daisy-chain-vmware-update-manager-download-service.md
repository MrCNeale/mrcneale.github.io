---
layout: post
title: Answered-Can you Daisy Chain VMware Update Manager Download Service?
date: 2014-05-19T18:00:00+00:00
comments: true
tags:
- UMDS
- Update Manager
- VCenter
- vmware-umds
- VUM
---
I had a design/suggested infrastructure set up on a recent project which basically had UMDS on a server in a DMZ getting ESXi patches from the internet via a proxy.  Then a server in a secondary DMZ outside one of the target environments running another UMDS instance.

The idea was to open 80/443 from UMDS in one DMZ to the other.  Adding the internet connected UMDS server as a url to the secondary UMDS server.

e.g.

<a name="GUID-BA1A86BE-3EF9-4551-A370-650BC5F8A4E3__CODEBLOCK_2B0481D8A3454708B8E7532D2AC1D467"></a><!-- --><kbd class="userinput">vmware-umds -S --add-url https://<var class="varname" style="font-style:italic;">host_URL</var>/index.xml --url-type HOST</kbd>

Can you do it?
<h1><em><strong>No.</strong> </em></h1>
It only works with vendor website, not a downstream UMDS.

Here endeth the lesson for today!

&nbsp;

(Ok, so what you should do instead is simply UNC copy the patch store to a server/location that the upstream "VUM" can access, wherever that may be.  From there it can be replicated on upstream if necessary via Robocopy etc.
This was discovered after many hours of headbashing.)
