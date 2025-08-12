# AD Authentication

## Kerberos authentication

XXX



## NTLM authentication

XXX



## Tools

### WinRS

This opens a non-interactive session through WinRM. It works as Evil-WinRM on Linux:

```powershell
> winrs -r:server -u:.\local_user -p:<password> cmd
> winrs -r:server -u:domain_user -p:<password> cmd
```
