---
layout: post
title: Network: North-South, East-West. Don't be embarrassed to ask
date: 2014-10-15 13:55
author: chrisneale
comments: true
categories: [Uncategorized]
---
<strong>Networking traffic described as points of the compass.</strong>

It's day 3 of vmworld for me and NSX is big. The keynotes feature it heavily, it's great new technology and the queues for standby to the breakout sessions where NSX is in the title are like queues for a new iPhone release.

So it took me this long to stop and say, wait! What is this north south east west stuff? Everyone refers to it to talk about traffic in data centres but no one has defined it. Is guessed but want sure. So I've checked and it is quite simple.

Because network diagrams historically always put the Core Switch at the top. It's the North.

So
<strong>North-South Traffic</strong> = Traffic in and out of the DC
<strong>East-West Traffic </strong>= Traffic between servers (VMs) within the DC

Each has it's own challenge and as workloads change from simple client server where almost all traffic was North-South, to now the traditional 3-tier web fronted application which generates lots more East-West traffice between the web, app and db layers.

More in the "Why does everyone presume you know this already?" series to come ;-)
