---
comments: true
date: "2017-03-14T22:00:00Z"
excerpt: Using powershell to read in keepass entries - part 2
tags:
- Powershell
- Security
- Scripting
title: Leveraging Keepass via Powershell-Part 2
---
Last time round we did 3 things
<ol>
<li>Loaded the Keepass DLL/EXEs in so we could call their routines</li>
<li>Created an Empty KeePass DB construnct to read an existing DB in to</li>
<li>Read in an existing KeePass DB into the pointer created in Step #2</li>
</ol>
These are common tasks we will need to do multiple times....so??? Yep functions.  If you want to see the detail flip back to Part-1
<div class="edit-comment-hide">
<details>
<summary>Expand this for details of Function Code Blocks</summary>
{{< highlight ruby >}}
Function Load-KPBinaries {
    Param ($PathToKeePassFolder)
  	# Check if Keepass DLLs are already loaded
	  If (! (Test-Path $PathToKeePassFolder)) { $log.WritePSError("KeePass software could not be located in $($PathToKeePassFolder)"); Return $null }
	  #Load .NET binaries in the folder
  	(Get-ChildItem -recurse $PathToKeePassFolder|Where-Object {($_.Extension -EQ ".dll") -or ($_.Extension -eq ".exe")} | ForEach-Object { $AssemblyName=$_.FullName; Try {[Reflection.Assembly]::LoadFile($AssemblyName) } Catch{ }} ) | out-null
}

Function Open-KPDB {
	Param ($PathToKPDB, $KPMasterPassword)
	$MyKPDatabase = new-object KeePassLib.PwDatabase 
	#Create Password Object
	$MyKPKey = new-object KeePassLib.Keys.CompositeKey
	$MyKPKey.AddUserKey((New-Object KeePassLib.Keys.KcpPassword($KPMasterPassword)));
	#Create pointer to file on disk object
	$IOConnectionInfo = New-Object KeePassLib.Serialization.IOConnectionInfo
	$IOCOnnectionInfo.Path = $PathToKPDB
	$KPNStatusLogger = New-Object KeePassLib.Interfaces.NullStatusLogger
	#open up the Database, using the Open function of the MyKPDatabase object we created earlier
	$MyKPDatabase.Open($IOCOnnectionInfo,$MyKPKey,$KPNStatusLogger)
	$KPEntries = $MyKPDatabase.RootGroup.GetObjects($true, $true)
	$KpFound=@()
	foreach($KPEntry in $KPEntries)
	{
		$KPFoundEntry = "" | Select-Object Title,UserName,Password
		$KPFoundEntry.Title = $KPEntry.Strings.ReadSafe("Title")
		$KPFoundEntry.UserName = $KPEntry.Strings.ReadSafe("UserName")
		$KPFoundEntry.Password = $KPEntry.Strings.ReadSafe("Password")
		$KPFound += $KPFoundEntry
	}
	$MyKPDatabase.Close()
	Return $KPFound
}
{{< / highlight >}}
</details>

