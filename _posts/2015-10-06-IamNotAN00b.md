---
layout: post
title: I am not a N00b! - Telling vCenter to stop showing you getting started pages!
date: 2015-10-06
author: Chris Neale
comments: true
categories: [VMware,vsphere,vcenter,config]
---
I should have found this years ago.

Like most of you reading this I get bugged by vCenter presuming this is my first Rodeo, as they say.

Everytime you go to a new Datacentre/Cluster/Folder/VM* (Delete as appropriate) it gives me the Getting Started pages.

So today I found the off switches:

For C# Client

<LI>Select Edit > Client Settings</LI>
<LI>Select the General tab</LI>
<LI>Deselect the Show Getting Started Tabs check box and click OK</LI>

For the Web Client

<LI> Go to https://hostname:9443/vsphere-client</LI>
<LI>Click the Help dropdown in the top right</LI>
<LI>Select Hide All Getting Started Pages</LI>

Job done.  You are now L33t @ vCenter
