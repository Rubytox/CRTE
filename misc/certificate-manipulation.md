# Certificate manipulation

## Convert PEM to PFX

```powershell
> openssl.exe pkcs12 -in .\pawadmin.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out pawadmin.pfx
```

