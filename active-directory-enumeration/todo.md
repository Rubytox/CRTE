# Basic enum

## Import AD modules

```
Import-Module C:\AD\Tools\ADModule-master\Microsoft.ActiveDirectory.Management.dll
Import-Module C:\AD\Tools\ADModule-master\ActiveDirectory\ActiveDirectory.psd1
```

## List users

```
Get-ADUser -Filter *
```

## List and expand property

```
Get-ADUser -Filter * | Select -ExpandProperty sAMAccountName
Get-ADUser -Filter * | select -expand sAMAccountName
```

## List groups

```
Get-ADGroup -Filter *
Get-ADGroup -Identity 'Domain Admins' -Properties *
Get-ADGroup -Identity 'Domain Admins' -Properties * | select -expand members
```
