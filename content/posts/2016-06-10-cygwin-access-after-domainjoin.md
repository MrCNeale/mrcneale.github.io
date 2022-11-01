---
comments: true
date: "2016-06-10T22:00:00Z"
excerpt: recreating the missing local userDB
tags:
- ssh
- windows
- linux
- cygwin
title: ssh to Cygwin fails after Computer joins domain
---

I was asked to look into an issue where ssh connections to a windows server running cygwin had started to fail after the computer joined an AD domain.


The user was running the scripts under a user local to that computer.  They needed to use that to run scripts with the same user before AND after it joined the domain.


I checked the usual, updated a GPO to disable "Deny access to this computer from the network".  Still no joy.
After joining the domain I could SSH in as a Domain User, but not the local user any more.


In checking the event logs I noticed that the Security Event Log was registering that when the local user tried to log in it showed as NOUSER.
After reading up on Cygwin some more I found that once a Computer is joined to a domain it uses that for it's default Account Database.  To allow
Cygwin to be able to search and match local user accounts it was necessary for it to have a copy of the local SAM DB dumped.
So I opened a Cygwin session and ran
{{< highlight ruby >}}mkpasswd -l > /etc/passwd{{< / highlight >}}

As soon as I had run that I could log in using COMPUTERNAME+localusername as my login.
The reason I needed to do that is that the passwd file generated AFTER joining the domain tags the hostname on to users.
I tested on a non-domain joined computer and that just added the users as localusername.  

So my tip is  
**Before joining a domain run the mkpasswd step above then you will be able to continue running scripts (presuming no other GPOs lock down your computer and prevent it!)**  
Why weren't they using WinRM? Don't ask :-(
