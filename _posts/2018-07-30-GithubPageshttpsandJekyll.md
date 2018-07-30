---
layout: post
title: Fixing Layout issues after forcing HTTPS on GitHub Pages for a Jekyll based site
excerpt: Tick the box, but don't forget to check your CSS
comments: true
date: 2018-07-30T22:00:00+00:00
image:
  feature: https://pages.github.com/images/logo.svg
tags: 
  - GithubPages
  - Jekyll
  - Https
  - LetsEncrypt
  - SSL
  - CSS
---
<img style="float: top;" src="https://jekyllrb.com/img/logo-2x.png">

<H2>Don't forget to update the CSS!</H2>
I had been meaning to set up my blog page to use SSL/HTTPS for a while.  Today I did it.
It's amazingly simple since Github Pages has teamed up with LetsEncrypt.  So I just had to tick one box.
Go to your Github pages Repo, in my case https://github.com/MrCNeale/mrcneale.github.io, and select the settings tab, or go to your repo /settings, 
https://github.com/MrCNeale/mrcneale.github.io/settings  
Once there simply scroll down to the Github Pages section and tick the box labelled "Enforce HTTPS".
Within 2-3 minutes Github will get a certificate for you (in my case for my custom domain name chrisneale.org!) and apply it.  
  
This was great, my website worked at https://www.chrisneale.org but wait, all the formatting and layout looked like HTML v1.0 and hideous!

I noticed in Chrome that it was complaining that it was only displaying the secure content....not the insecure...."What insecure content??"  
A quick trip into the developer console in Chrome by hitting F12 showed the culprit.  The CSS was building from information somewhere with an explicit http in.
I found that it was the _config.yml file which needed the "Site Wide Configuration" section URL value updating to be https://www.chrisneale.org

After editing that, 2 more minutes for Github Pages to recompile, and back to looking lovely!
