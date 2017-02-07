---
layout: post
title: Leveraging Keepass via Powershell to Keep Passwords Out of Scripts
excerpt: Using powershell to read in keepass entries - part 1 
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
Now let's Load the .net binaries in  
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
Ok, so as you can see you've got a DB there ready to be populated, but not with passwords, we're going to use it to read in an existing database.  But first we need to create a few variables before we try and open it.  We have an empty KP variable to store it in, next we need the location on disk and the existing Master KP Database password.  
Our file is in the same folder c:\temp it's called test.kdbx so c:\temp\test.kdbx and FOR THE PURPOSES OF THIS BLOG ONLY I've set the password to Password123 and I will be showing it in the script, for clarity #ironic  
{% highlight ruby %}
#Create Password Object
$MyMasterPassword = "Password123"
$MyKPKey = new-object KeePassLib.Keys.CompositeKey
$MyKPKey.AddUserKey((New-Object KeePassLib.Keys.KcpPassword($MyMasterPassword)));
#Create pointer to file on disk object
$IOConnectionInfo = New-Object KeePassLib.Serialization.IOConnectionInfo
$IOCOnnectionInfo.Path = "c:\temp\test.kdbx"
$KPNStatusLogger = New-Object KeePassLib.Interfaces.NullStatusLogger
#open up the Database, using the Open function of the MyKPDatabase object we created earlier
$MyKPDatabase.Open($IOCOnnectionInfo,$MyKPKey,$KPNStatusLogger)
{% endhighlight %}
All done but nothing!!!  Patience.  Now we read in all the objects in the DB to a variable, and loop round the elements in the array variable calling the ReadSafe method to read the 
{% highlight ruby %}
$KPObjects = $MyKPDatabase.RootGroup.GetObjects($true, $true)
#Now loop round and list them all to the command window
foreach($KPObject in $KPObjects)
{
  write-host $KPObject.Strings.ReadSafe("Title") $KPObject.Strings.ReadSafe("UserName")  $KPObject.Strings.ReadSafe("Password")
}
{% endhighlight %}
And you should get the list  
<IMG src="/public/kpass2.png">  
which should match your db  
<IMG src="/public/kpass3.png">  
Here endeth the lesson for today.  In Part 2 - we'll look at reading in a specific username/password based on title, and maybe writing back to the kdbx file!  
