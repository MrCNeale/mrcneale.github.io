---
layout: post
title: Leveraging Keepass via Powershell to Keep Passwords Out of Scripts
excerpt: 
tags: 
  - Powershell
  - Security
  - Scripting
date: 2017-02-06T22:00:00+00:00
comments: true
---

Passwords visible in scripts....just say that out loud.  If that doesn't make you itch with a compelling urge to take the author to one side and have seriously stern words with them, then feel free to move on to the next blog....but you're a bad person!
  
It's something no-one should have to see, but sometimes, for brevity it happens.  Someone hard codes a password in a script/program "just to test".  Well it's a bad habit to get in to.  But I'm not here to bring you problems, here's a solution.  
I use KeePass and I constantly find cool new features.  Like the paste to type, where it actually types in the username, tabs to the next entry field and types the password.  This alone has saved me hours of my life and much frustration.  
  
So I was even more impressed when I found you could make powershell calls to KeePass with a relative minimum of fuss and could use this feature to create KeePass password DBs from scratch or to read in existing ones to utilise for an script/activity.
  
Let's get stuck in.
First you need the Keepass installable version installed where you are running your script (or at least a copy of the installed files exe's and dll's).
Given those let's Load the .net binaries in  
{% highlight ruby %}
PS C:\temp\KeePass-2.31> (Get-ChildItem -recurse $script:myinvocation.mycommand.path | Where-Object {($_.Extension -EQ ".dll") -or ($_.Extension -eq ".exe")} | ForEach-Object { $AssemblyName=$_.FullName; Try {[Reflection.Assembly]::LoadFile($AssemblyName) } Catch{ }} ) | out-null
PS C:\temp\KeePass-2.31>
{% endhighlight %}
Now they are loaded let's test we can create a new KeePass object (an object not a kp database on disk.  The object is just a construct/pointer to an in-memory KP Database object), so try  
{% highlight ruby %}
$MyKPDatabase = new-object KeePassLib.PwDatabase  
$MyKPDatabase  
{% endhighlight %}

<IMG src="/public/kpass1.png" align="right">  

