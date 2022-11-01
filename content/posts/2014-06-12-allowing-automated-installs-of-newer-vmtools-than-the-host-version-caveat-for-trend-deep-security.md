---
author: chrisneale
categories:
- Deep Security
- dsm
- dsva
- ESXi
- Trend
- VCenter
- Virtualisation
- vmtools
- VMWare
- vsphere
comments: true
date: "2014-06-12T00:00:00Z"
title: Allowing automated installs of newer VMtools than the host version (Caveat
  for Trend Deep Security)
---
During a recent patching cycle VMtools across a set of VMs were updated to the latest version.  This was to patch security vulnerabilities in the versions which were running to ensure the environment was as secure as possible for a PenTest which would go towards IL3 Accreditation.

Shavlik was good enough to oblige here and went out and patched lots of VMs with new VMtools automatically to the latest version.

Then an issue occurred with Trend Deep Security.

Normally when doing a VMtools install for a VM managed by trend the following should be considered as per the <a href="http://files.trendmicro.com/documentation/guides/deep_security/DS%209.0%20Best%20Practice%20Guide.pdf">Trend Deep Security Best Practices PDF </a>

=============================

VMware includes the VMware vShield Endpoint Driver in VMware Tools 5.x, but the installation program

does not install it on Guest VMs by default. To install it on guest VM, review the installation options in the
<table dir="LTR" border="1" width="534" cellspacing="0" cellpadding="7">
<tbody>
<tr>
<td colspan="3" valign="TOP" height="7"><span style="font-size:small;">table below: </span><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;"><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;">Available VMware Tools Installation Options </span></span></td>
</tr>
<tr>
<td valign="TOP" width="33%" height="7"><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;"><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;">Installation Option </span></span></td>
<td valign="TOP" width="33%" height="7"><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;"><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;">vShield Endpoint </span></span></td>
<td valign="TOP" width="33%" height="7"><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;"><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;">Action </span></span></td>
</tr>
<tr>
<td valign="TOP" width="33%" height="14"><span style="font-size:small;">Typical </span></td>
<td valign="TOP" width="33%" height="14"><span style="font-size:small;">vShield Endpoint does </span><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;"><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;">NOT </span></span><span style="font-size:small;">install </span></td>
<td valign="TOP" width="33%" height="14"><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;"><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;">DO NOT </span></span><span style="font-size:small;">select this option </span></td>
</tr>
<tr>
<td valign="TOP" width="33%" height="14"><span style="font-size:small;">Complete </span></td>
<td valign="TOP" width="33%" height="14"><span style="font-size:small;">vShield Endpoint </span><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;"><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;">installs </span></span></td>
<td valign="TOP" width="33%" height="14"><span style="font-size:small;">Select if you want all features </span></td>
</tr>
<tr>
<td valign="TOP" width="33%" height="23"><span style="font-size:small;">Custom </span></td>
<td valign="TOP" width="33%" height="23"><span style="font-size:small;">You must </span><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;"><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;">explicitly </span></span><span style="font-size:small;">install vShield Endpoint </span></td>
<td valign="TOP" width="33%" height="23"><span style="font-size:small;">Expand VMware Device Drivers &gt; VMCI Driver </span></td>
</tr>
<tr>
<td colspan="3" valign="TOP" height="14"><span style="font-size:small;">Select vShield Drivers and choose </span><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;"><span style="font-family:Interstate-Light, Interstate-Light;font-size:small;">This feature will be installed on local drive. </span></span></td>
</tr>
</tbody>
</table>
=========================

Shavlik didn't consider that I wanted a Custom/Complete install and so VMCI was missing.

Ok, that would be fine but now we have VMs with newer VMtools than the ISO which is stored on the ESXi Host. So we can't just re-run the install/upgrade via vSphere

That's where <span style="color:#336699;">Andreas Peetz </span>over at http://www.v-front.de comes in with his blog post about how to effectively do 2 things

1. Place the newer VMtools iso onto a shared datstore

2. Tell the ESXi host to look at the new location when it does VMtools installs/upgrades

Now you can do automated installs/upgrades via the gui (powercli cannot pass through the custom parameters)

Here is Andreas' Blog Post <a href="http://www.v-front.de/2013/01/how-to-use-latest-vmware-tools-with.html">http://www.v-front.de/2013/01/how-to-use-latest-vmware-tools-with.html</a>

&nbsp;
