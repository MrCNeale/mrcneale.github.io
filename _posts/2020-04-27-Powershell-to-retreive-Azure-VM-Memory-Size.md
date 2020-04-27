---
layout: post
title: Powershell to retreive Azure VM Memory Size
excerpt: joining the dots from Hardware profiles and component sizes
comments: true
date: 2020-04-26T22:00:00+00:00
image:
  feature: https://docs.microsoft.com/en-gb/azure/virtual-machines/linux/media/index/logo_powershell.svg
tags: 
  - Powershell
  - Azure
  - VM
  - IaaS
  - Memory
---
<img src="https://docs.microsoft.com/en-gb/azure/virtual-machines/linux/media/index/logo_powershell.svg" height="20%" width="20%">

<H1>"How much memory do my VMs have?"</H1>
It seems like an obvious thing to ask.  
It seems reasonable to me....However without additional tooling, such as inventory or configuration management software, you can't easily get a list of Azure VMs and their memory sizes that you can manipulate.  
If you just run "get-azvm" then it returns a property of the vm which is a "HardwareProfile".  This just has a sub property of the VM t-shirt size.  
e.g. Standard_E8_v3  
Unless you have an idetic memory, this doesn't mean much.  Even if you do, it doesn't help powershell to process the data.  
But you can get a list of the details of each t-shirt size, via the prosaicly named get-azvmsize command.  
Instead of telling you the size of a VM, it tells you the size of VMs you can deploy, or resize to, for a particular region/availability-set/vm.
  
Now you have the two parts of the puzzle, you just need to knit them together.
First get a list of all your VMs (you could filter if you wanted)  
```
$vms=get-azvm
```
Now get a list of VM t-shirt sizes (I use eastus as it's usually the most complete list of sizes)  
```
$vmsizelist = Get-AzVMSize -Location eastus  
```

Now we loop through each vm, get it's hardware profile. Read the VMSize property, then go find that row in the t-shirt size table, and find how much memory that t-shirt size has.  
```
foreach ($vm in $vms){
  $name = $vm.Name
  $size = $vm.HardwareProfile.VmSize
  $memory = ($vmsizelist | ?{$_.name -eq $size}).memoryinmb
}
```

Glue that all together adding in code to build up an array or hashtable of your own to use later, and you get.  
```
$arraywithheader=New-Object System.Collections.Generic.List[System.Object]
$vmsizelist = Get-AzVMSize -Location eastus
$vms=get-azvm
foreach ($vm in $vms){
 $name = $vm.Name
 $size = $vm.HardwareProfile.VmSize
 $memory = ($vmsizelist | ?{$_.name -eq $size}).memoryinmb
 $row=[pscustomobject]@{'Name'= $vm.Name;"Memory"=$memory}
 $arraywithheader.add($row)
 $row=$null
}
$arraywithheader
```
