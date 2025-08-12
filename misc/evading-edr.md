# Evading EDR

There are multiple ways to evade EDR or EDR-like protections.



## Check DeviceGuard

First, check whether DeviceGuard is enabled:

```powershell
> Get-CimInstance -ClassName Win32_DeviceGuard -Namespace root\Microsoft\Windows\DeviceGuard
```

Then, retrieve the DeviceGuard policy:

```powershell
> copy \\us-jumpX.US.TECHCORP.LOCAL\c$\Windows\System32\CodeIntegrity\DG.bin.p7 C:\AD\Tools
> . C:\AD\Tools\CIPolicyParser.ps1
> ConvertTo-CIPolicy -BinaryFilePath C:\AD\Tools\DG.bin.p7 -XmlFilePath C:\AD\Tools\DG.bin.xml
```

We find in the XML file an allowed rule:

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

This means that all files that have their attribute `ProductName` to `Vmware Workstation` will not be blocked.
