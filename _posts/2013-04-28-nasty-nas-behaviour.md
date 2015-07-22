---
layout: post
title: NASty NAS behaviour!
date: 2013-04-28 22:21
author: chrisneale
comments: true
categories: [cloudstation, dsm, fix, nas, root, storage, synology, Uncategorized]
---
Now let me start by saying that 95% of the functionality and workings of my Synology DS212J.

It sits there, it hosts my mp3'd up collection of CDs, tens of thousands of pictures and hundreds of GB of family videos that I don't watch.  But it's easy to access and I love the mobile phone app/apps.

However I recently bought an SSD for my "big pc".  That is a bit of a Peter Kay-ism, e.g. it's the nickname most people I know have for their desktop (for those at either end of the spectrum who either are scared/too stingy to move to a laptop or the ones who still think in their mind they need a giant box, as well as the tablet, xbox, skyHD, phone and mini-server!)

Anyway back to the point.  So my plan was thus

1. Move all my docs/music/pics/videos from "big pc" to Synology.  This would free mean my 1Tb drive in the big pc would only be being utilised around 150Gb

2. Put SSD in Big PC and image data from the 1TB drive across (only 150Gbs worth)

3. This frees up 1Tb drive to go in Synology as a mirror drive in case the current one goes pop!

4. Involves ensuring that the Synology is backed up to USB which is where I spotted the problem.

I have about 300-400Gb of shtuff that isn't programs and OS.  More like 300 than 400.  So when I came to back my stuff up to my external 500gb usb drive (small by current standards I know and it's a WD My Book so it is massive and has an external PSU) I found that I had used nearly 700GB on my 1Tb drive in the Synology!!

What the flip!

I did the sums a few times and kept coming up with 375Gb max.  So what was using the other 400Gb?
Well to save you the suspense it was some sort of caching done by the Cloudstation Application.

I did install this package on the Synology.  I did tell it to sync photos from my phone.  That was it.  I didn't tell it to sync to any PC etc.  So I've no idea what the 400Gb of cache was! or where it came from, or which network it was saturating! (just my lan or my net connection too?)

Getting rid of it was a pain though.  Here's the steps
<ol>
	<li><span style="line-height:1.5;">Uninstall Cloudstation package from your Synology.  Easily done from Package Center</span></li>
	<li>Enable Telnet on the Synology.  Go to Control Panel/Terminal and tick the "enable telnet box" then click apply</li>
	<li>Telnet to your synology IP from your PC.</li>
	<li>Login is as "root" using the same password you set for "admin"</li>
	<li>Locate the cloudstation cache folder by typing (note that your volume may be called something else)
cd /volume1/@cloudstation</li>
	<li>Delete all files in there (ensure you are in the folder first using pwd)
rm -Rf /volume1/@cloudstation/*</li>
	<li>This may take a while but when it's done refresh your synology management web page and voila, 100s of GB back!</li>
</ol>
I have DSM 4.2-3211, but I installed Cloudstation last revision when it was first released in 4.2.

Enjoy!
