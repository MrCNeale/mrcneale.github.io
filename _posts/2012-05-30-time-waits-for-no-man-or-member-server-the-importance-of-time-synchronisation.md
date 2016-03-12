---
layout: post
title: Time waits for no man, or member server - The Importance of Time Synchronisation in Windows 2008 Domains
date: 2011-05-30T18:00:00+00:00
comments: true
tags: 
- 2008
- activation
- dns
- kms
- ntp
- slmgr
- Windows
---
<div>Perhaps I should put it down to it being a Friday, like the inverse of a Monday instead of lots of things breaking, things just didn't seem to be working.</div>
<div>

I had built a Windows 2008 server from a custom SOE disc, all seemed well but it wouldn't register in DNS.  I'd specified the name servers correctly and the server I'd built the week before had no issues registering in the same domain.  This was a relatively new domain however and the server was pinging all the same and joined the domain without issue.  I continued...but then spotted that the SOE post build tasks had failed on Activating Windows.

I popped in to server manager to run the activation manually.....no joy.  It prompted me for a product key and seemed to have lost the product key it originally had.  The domain had a KMS server so I decided to try and activate using the command line script <strong>slmgr.vbs </strong>for updating/edit KMS configuration on clients and for activating clients.  I've added links to the syntax below and most usefully a list of the Microsoft Product Keys, which is not easily googleable, so one to stick in your bookmarks.

The vbs script kept erroring and complaining that activation failed.  However when RDP-ing to the KMS server itself and running

<strong>cscript slmgr.vbs /dlv</strong>

To get the detailed license server information I could see that every time I tried to activate the client (in this case a member server) that both the attempted activations and failed activations counts increased.

So I knew that KMS was functioning and that the communications were working ok between the two.  I went into the member servers event log (when in doubt RTFM type moment) and before I found any license/activation errors I spotted a Time service error.  It was a familiar one to most complaining that the server was more than 5000ms out from it's time source, e.g. the DC.  That's when it all clicked I did remember having to reset the clock on the build last week and having not done this (I claim Friday as my defence) the KMS server was refusing to activate the Windows Product due to the time difference.

Now this wasn't the error message given by slmgr.vbs or in the event log.  However there is an error code that might have been more useful
<table>
<tbody>
<tr>
<td>0xC004F06C</td>
<td>The Software Protection Service reported that the computer could not be activated. The Key Management Service (KMS) determined that the request timestamp is invalid.</td>
<td>KMS client</td>
<td>The system time on the client computer is too different from the time on the KMS host.</td>
<td>Time sync is important to system and network security for a variety of reasons. Fix this issue by changing the system time on the client to sync with the KMS. Use of a Network Time Protocol (NTP) time source or Active Directory Domain Services for time synchronization is recommended. This issue uses UTP time and is independent of Time Zone selection.</td>
</tr>
</tbody>
</table>
So after manually updating the clock (and date...ahem) to the correct time and then syncing up with the DC

<strong>w32tm /config /syncfromflags:domhier /update</strong>

<strong>net stop w32time </strong>
<strong>net start w32time</strong>

Then I re-ran the activation command

<strong>cscript slmgr.vbs /ato</strong>

and bosh...all activated........and DNS registered fine too...........and everything "just worked"

<strong>What have "we" learned?</strong>
<ul>
	<li>Always make sure your server's time is synced with the domain</li>
	<li>Always check the event log first (or early on)</li>
	<li>If possible have repeatable build processes, even if those are manual checklists or team-held scripts (in the case where an automated deployment solution is not available/practical)</li>
</ul>
Syntax and examples of how to run Slmgr.vbs script to license/activate a server from the command line, primarily for KMS activations.
<a href="http://technet.microsoft.com/en-us/library/ff793433.aspx">http://technet.microsoft.com/en-us/library/ff793433.aspx</a>

List of Windows Product keys for use with slmgr.vbs if your server/desktop loses it's product key.
<a href="http://technet.microsoft.com/en-us/library/ff793421.aspx">http://technet.microsoft.com/en-us/library/ff793421.aspx</a>

</div>
