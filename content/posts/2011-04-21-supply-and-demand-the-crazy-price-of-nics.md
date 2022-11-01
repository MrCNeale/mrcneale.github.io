---
comments: true
date: "2011-04-21T18:00:00Z"
tags:
- 3com
- esxi
- Intel
- e1000
- VMware
- Whitebox
title: 'Supply and Demand : The Crazy Price of NICs'
---
Ok, so I want to have my own ESXi host at home.  Initially it may sound like a tall order but there are lots of good community sites out there describing how to do it and what works and what doesn't.

www.vm-help.com has a very good HWCL on it compiled from user experience.  So after trying and failing to install ESXi 4.1 I found on vm-help that my motherboard's on-board NIC isn't compatible as there are no drivers on the installation CD for it.

Now there is a workaround but the only person who has documented it did so by doing it in Linux and dismissing the possibility that the someone might want to do the same on Windows.  Strike 1 (or maybe 1,000,00) for the Linux community.  Is it any wonder that people see them as snobby elitist nerds lacking in social skills?  Could I work through his solution? Yes of course, it's not rocket science.  But like most people trying these things out time is a commodity rarer than gold.  So rather than faff with oem.tgz and redo-ing an ISO image with linux DD commands I borrowed another NIC from a friend.  Unfortunately that didn't work either so I have resigned myself to having to purchase an Intel Pro 1000 GT or similar compatible card.

Now my previous job wasn't as auspicious as my current employer, but it did come with some great perks such as having access to lots of "spare" hardware.  I vividly remember a draw brimming over with Intel NICs.... I can even remember the chipset number 82557.  Oh to be able to lay my hands on one of these now!  An Intel Pro 1000 GT comes in at around £22 for my cheapest local supplier, or free delivery.  Similar prices can be found for other, what I thought would be, common NICs.

Why?

Well the only answer I can come up with is Supply and Demand.  All home motherboards now come with an on-board gigabit NIC, some even 2 and they don't have to be high end boards.   All servers HP/Dell etc. come with dual port NICs on-board as standard too.  So the need or demand for seperate PCI cards must have dwindled.  A number of friends or colleagues I told or asked as a tested all responded the same "ahh you'll pick one of those up for a tenner..."  Only to be baffled when I told them that the rarity of these items means they are now like precious metals!  I also searched for a 3com905c-tx which came in at about £10 but that's one ancient Network Interface Card.  All in the name of compatibility.

The moral of the story?  Not sure there is one really, perhaps it's don't let anyone throw away working Tech and store it somewhere, even if it's a friends garage and you pay storage costs in beer.  It will come in handy one day...or may even accumulate value somehow!

And with that.......I'm off to research how to modify oem.tgz (coz I'm tight! :-)
